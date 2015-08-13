---
title: Introducing Triply for Foursquare
author: Ivan Zlatev
layout: post
permalink: /2011/05/02/introducing-triply-for-foursquare/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 227
dsq_thread_id:
  - 293323943
categories:
  - Coding
tags:
  - Start-Up
  - Triply
---
During my trip to [Dubrovnik ][1]last week whilst checking in on Foursquare an idea struck me &#8211; can&#8217;t I use the Foursquare checkin history and checkin comments to document the places I visit and also my route. This is how [Triply][2] was born after a two day hackathon.

In a single sentence [Triply][2] lets you **quickly create and share your trips, experiences and days out** in **three simple steps** using your **existing Foursquare checkins**.

You pick a name and description for the trip/day out/experience:

[<img class="aligncenter size-full wp-image-738" title="about-triply-screenshot-1" src="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-1.png" alt="" width="423" height="196" />][3]

You then pick start/end dates to load the Foursquare Checkins

[<img class="aligncenter size-full wp-image-739" title="about-triply-screenshot-2" src="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-2.png" alt="" width="547" height="344" />][4]

You then organize (reorder and remove) the checkins and that&#8217;s it!

<p style="text-align: center;">
  <a href="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-3.png"></a><a href="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-3.png"><img class="aligncenter size-full wp-image-740" title="about-triply-screenshot-3" src="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-3.png" alt="" width="692" height="314" /></a>
</p>

<p style="text-align: left;">
  Notice the funky AJAX drag and drop goodness:
</p>

<p style="text-align: left;">
  <a href="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-4.png"><img class="aligncenter size-full wp-image-741" title="about-triply-screenshot-4" src="{{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-4.png" alt="" width="413" height="330" /></a>Once saved you get a trip page like the one below, where others can see the route, places and your comments and ask questions, etc.
</p>

<p style="text-align: center;">
  <a href="{{ site.url }}/wp-content/uploads/2011/05/triply-sample-trip.png"><img class="aligncenter size-full wp-image-742" title="triply-sample-trip" src="{{ site.url }}/wp-content/uploads/2011/05/triply-sample-trip.png" alt="" width="728" height="668" /></a>
</p>

<p style="text-align: center;">
  &nbsp;
</p>

<p style="text-align: left;">
  Triply is ASP.NET MVC 3 and Entity Framework with an SQL Express server at the backend right now. It&#8217;s basically version 0.1 &#8220;Get it out there&#8221; release without even a proper web design, because I am intrigued to hear what people think about the idea.
</p>

<p style="text-align: left;">
  I have personally have many things in mind that can be added in the future such as:
</p>

  * A Web design, yo know.
  * Have an indicator of the type of place was visited (museum, gallery, beach, etc)
  * Allow to get info about a place quickly such as wikipedia entry for historical places and hotel rating/reviews for hotels.
  * Facebook Places/Checkins support
  * Maybe go down the &#8220;trip diary&#8221; route as well with auto-mapped/auto-linked photos to places and mobile apps.
  * Maybe go down the &#8220;share a fun experience to do&#8221; route (e.g. go to this pub, then to this museum, then to that other fun place to spend a great day).
  * and more &#8211; basically I feel like this has potential.

&#8230; but it&#8217;s just me, so if someone is enthusiastic about the idea and is familiar with the Microsoft technology stack I would be more than happy to team up. You never know some day we might be able to afford to buy the .com or even .ly domain <img src="{{ site.url }}/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

 [1]: http://en.wikipedia.org/wiki/Dubrovnik
 [2]: http://triply.net
 [3]: {{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-1.png
 [4]: {{ site.url }}/wp-content/uploads/2011/05/about-triply-screenshot-2.png