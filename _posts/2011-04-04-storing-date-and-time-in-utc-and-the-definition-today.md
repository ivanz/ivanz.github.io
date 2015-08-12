---
title: 'Storing Date and Time in UTC and the definition &#8220;Today&#8221;'
author: Ivan Zlatev
layout: post
permalink: /2011/04/04/storing-date-and-time-in-utc-and-the-definition-today/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 860
dsq_thread_id:
  - 271111000
categories:
  - Coding
tags:
  - .NET
  - HowTo
---
Storing date and times in UTC on the server side is all nice and dandy, however there are a few gotchas, one of which the definition of &#8220;Today&#8221; when you a dealing with time zones.

Say we are dealing with a system where a user can keep a history/log of all the food that he eats along with the date and time of when that happened. Current system times are:

> Current User Time (UTC+2): 4 April 01:45  
> Current Server time (UTC): 3 April 23:45

and we have two log entries:

> Log Entry User local time (UTC+2): 3 April 19:30  
> Log Entry Server UTC time: 3 April 17:30

> Log Entry User local time (UTC+2): 4 April 01:30  
> Log Entry Server UTC time: 3 April 23:30

If a user wants to see all the log entries for &#8220;Today&#8221; his perspective for today is 4 April, but if we use the current UTC date on the server (3 April) to do the query we are going to end up with two results, but only one of them (the second) is for the 4 April. This makes perfect sense on the server side, because from the server end perspective both of those log entries were logged on the 3rd, but that&#8217;s not the case with the user.

To solve this we need to calculate the start of user today day and end of user today day and use it as a range to query the log history.

The steps are more or less:

1. Get the current server time and convert it to user time.

2. &#8220;Rewind&#8221; that user time back to midnight (00:00)

3. Convert that to back UTC and you get the start of day for the user in server terms.

4. Add 23 hours, 59 minutes, 59 seconds to the above and you have the end of user day in server terms

5. Query for user\_start\_of\_day <= logentry.Date <= user\_end\_of\_day

A worked example for the above dates and times:

> Current Server time (UTC): 3 April 23:45

1. Server time UtcNow is 3 April 23:45 and the user is in UTC+2, so the user local time is 4 April 01:45

2. This gives start of day for the user local time at 4 April 00:00

3. Which is 3 April 22:00 in server UTC time &#8211; the user\_start\_of_day

4. + 23:59:59 gives us 4 April 21:59:59 as the end of day

5. We query for log entries where the log UTC timestamp is between 3 April 22:00 and 4 April 21:59:59 , which will return only one result in the above case, which is correct.

Here is also a C# TimeZone class I have for each of my users in one of my toy projects:

<pre class="brush: csharp; title: ; notranslate" title="">public class TimeZone : Entity
{
    protected TimeZone ()
    {
    }

    public TimeZone (string name, TimeZoneInfo timeZone)
    {
        if (timeZone == null)
            throw new ArgumentNullException ("timeZone", "timeZone is null.");
        if (String.IsNullOrEmpty (name))
            throw new ArgumentException ("name is null or empty.", "name");

        Name = name;
        this.TimeZoneInfo = timeZone;
    }

    public virtual string Name { get; set; }
    public virtual TimeZoneInfo TimeZoneInfo { get; set; }

    public virtual DateTime StartOfToday {
        get {
            DateTime serverNow = DateTime.UtcNow;
            DateTime userNow = ToLocalTime(serverNow);
            return ToUniversalTime (userNow.Date);
        }
    }

    public virtual DateTime EndOfToday {
        get { return StartOfToday.Add (new TimeSpan (23, 59, 59)); }
    }

    public virtual DateTime ToLocalTime (DateTime utcTime)
    {
        return TimeZoneInfo.ConvertTimeFromUtc (utcTime, this.TimeZoneInfo);
    }

    public virtual DateTime ToUniversalTime (DateTime localTime)
    {
        if (localTime.Kind == DateTimeKind.Utc)
            return localTime;

        return TimeZoneInfo.ConvertTimeToUtc (localTime, this.TimeZoneInfo);
    }
}</pre>

P.S: Some of you will spot a further implication of this and that is that we must always store the time as well as the date.