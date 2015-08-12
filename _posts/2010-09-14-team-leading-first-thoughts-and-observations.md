---
title: 'Team Leading &#8211; First Thoughts and Observations'
author: Ivan Zlatev
layout: post
permalink: /2010/09/14/team-leading-first-thoughts-and-observations/
ratings_users:
  - 4
ratings_score:
  - 16
ratings_average:
  - 4
views:
  - 291
dsq_thread_id:
  - 141900054
categories:
  - Coding
  - Diary
---
## Introduction

In March this year I joined IBM as a Software Engineer (Graduate). For my huge disappointment, even though I came with several years of development experience, they put me in Test. I wasn&#8217;t simply assigned to any test phase, but probably to one of the worse, where the test plan had no test procedures in it, because it had been written before the code. This can be good (test driven development), but not in the case where you are a Tester and you have to do semi black-box testing by injecting hardware errors for implementation-dependent corner cases. I can&#8217;t and won&#8217;t go into specifics (because they don&#8217;t matter anyway), but for what it&#8217;s worth &#8211;  it&#8217;s some really nasty stuff.

The ability to run a test depends on about five or more asynchronously changing variables. When I say variables I don&#8217;t mean &#8220;int a = 5;&#8221;, but things like &#8220;is the frontend code ready and is the backend code that it uses ready and is the firmware ready and is it integrated in the builds available and do we have the required hardware and gadgets&#8221; and so on. No fun. And the worse is that noone was tracking this. Probably the team leads were overloaded with other work (other test phases) already and didn&#8217;t notice that the testers were in an endless unproductive loop. They were of trying to test something that is not ready yet, filing bugs which obviously got returned then they try to test something else that turns out is not ready yet as well, but maybe the next thing they test works and so on and so on. Obviously this wouldn&#8217;t have happened under normal circumstances (I assume and truly hope so), but due the nature of the project and the time scale it did. Speaking up didn&#8217;t work, because the team leads weren&#8217;t up to date with the details and couldn&#8217;t fully grasp the seriousness of the issue.

Anyhow &#8211; it was a rather frustrating experience and it didn&#8217;t take me long to &#8220;give up&#8221;, sit down and spend several days trying to aggregate all of the variables from the different teams and figure out how to get their latest statuses in the least intrusive way and basically start tracking what the hell was going on. I should had seen this coming probably, but my team lead realized that I am getting into this and a week later basically offloaded managing those test phases to me.

## Observations

So here I am semi-formally managing or &#8220;team leading&#8221; about 10 people  for the first time ever in my life, only 6 months after starting this job. We are working together on very complex and hard to manage test phases.

So to summarize I think the prerequisite to get to here in a very simplified form are:

  * Low frustration tolerance, especially when you see an obvious lack of efficiency and desire to take action.
  * Grasp of the bigger picture and keeping on top of it.
  * Good understanding of the structure of the department and which are the key people in each team. Those won&#8217;t necessarily be the team leads, but the ones that are helpful by nature and don&#8217;t mind you pinging them an IM or dropping by their desk for a quick question. They are the ones that are happy to look into stuff that&#8217;s not theirs instead of passing you around to other people (if they can). Obviously this requires good communication skills, so that you don&#8217;t annoy people ;).
  * To not to be afraid to interrupt someone who you&#8217;ve never met before in the middle of whatever they are doing in order to extract the information you needs. Obviously not when that person is on the phone.
  * Dedication and motivation for crunching data and processing other team&#8217;s status spreadsheets and other administrative tasks. Unfortunately.
  * Ability to multi-task like crazy.

So far it&#8217;s been an amazingly interesting experience for me and I truly appreciate the opportunity to explore this type of work. I have already made several observations which I am going to share and log for future reference.

### People

People are different (O&#8217;rly?). Some take the initiative and simply go and start banging on. They don&#8217;t need you to hold their hand, because they like their independence. In fact, although not showing it, they don&#8217;t appreciate questions like &#8220;how&#8217;s the work going?&#8221;, because it makes them feel patronized, untrusted and underestimated. They also don&#8217;t like to be pat on the back when they&#8217;ve done a brilliant job, because they feel like you are doing it on purpose even if you aren&#8217;t. Special caution has to be taken when and how to express gratitude to those people &#8211; the context is especially important. Last observation is that it takes a while to earn their trust, because they don&#8217;t seem to believe you are up to the task.

Others are not so motivated and don&#8217;t really care. If they hit a problem they are more than happy just to file a bug, pass the responsibility to someone else and call it a day. They aren&#8217;t motivated and will stay quite if they don&#8217;t have any more work left to do. Hence it&#8217;s very important to be on top of everything and know who is doing what, what is blocking which work and so on. Those people don&#8217;t mind to be pushed gently and are quite happy to be just told what to do, unlike the previous type.

The third type I have observed is the shy and sometimes disoriented type. They are very quite when they hit a problem, but won&#8217;t pass the responsibility around. If they don&#8217;t understand something they get confused and disoriented. Those people need some time to feel relaxed and confident in whatever they are doing and sometimes require someone to hold their hand. I feel like providing those people with higher level information and more details of what&#8217;s going on in the team in regards to their work helps. When people from different teams are involved or required to help them out it&#8217;s best if they get introduced in person and also get told a bit more about that person and what is his role in the team. Third thing that helps is to constantly remind them that if they hit a problem or simply have a question, they should simply either ping you on IM or drop by your desk without any worry.

Again people are different &#8211; but more or less a combination of those three types at least in my team.

### Responsibilities

My team members keep coming to me for questions and guidance, so I feel those are part of my responsibilities to them:

  * Always keep on top of things and be aware of what they are talking about and the context of their question. Otherwise they won&#8217;t see the point in coming to me in the future, because they would think I don&#8217;t really know what&#8217;s going on.
  * Be able to admit that I don&#8217;t know the answer to a question and most importantly be able to point them to someone who does straight away. Otherwise they will get passed around like crazy, will get frustrated because their time is being wasted and won&#8217;t come back to me in the future.
  * Try to be in the office before the team. Quite often in the morning they have questions regarding what they were working on the day before, which I need to be prepared for.

Unfortunately keeping on top of things takes a lot of administrative work, reading through statuses and spreadsheets and also creating such in order to assist you. This is very, very time consuming. More so than I have ever expected.

### Random Observation

This is a very random observation, but some people are so &#8220;brainwashed&#8221; that unless you wear a shirt and trousers they won&#8217;t really feel like you are or see you as a lead/manager and will take you less seriously when you ask them to do something (also will feel less responsible). Really odd, but I guess this is what enterprise companies do to you? Those are only a few people and they don&#8217;t mind after they get to know you.

## Conclusion

Managing people is very challenging and I am quite enjoying it so far. I feel like it&#8217;s going to be weird going back to my normal duties once I am done managing the test phases that I am. Unfortunately I am sure that I won&#8217;t take credit for the effort I am putting in, because I doubt any of the management know about it. Nevertheless I truly appreciate the opportunity that I have and I am very grateful when the people around me appreciate my work. Yesterday one guy told me that the work I am doing is &#8220;freaking brilliant&#8221; and this is my true medal.