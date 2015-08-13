---
title: 'Triply: Day 11 Summary'
author: Ivan Zlatev
layout: post
permalink: /2011/05/12/triply-day-11-summary/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 91
dsq_thread_id:
  - 
categories:
  - Coding
  - Diary
  - Technology
tags:
  - Start-Up
  - Triply
---
11 days ago I [posted the launch notice][1] for [Triply][2]. The only other place where I publicized it was on the Foursquare Apps list [here][3]. It was very surprising for me that given no &#8220;advertisement&#8221;, no web design and very little features it managed for 11 days to score:

  * 103 user accounts &#8211; 83 Foursquare and 22 Facebook
  * 2226 actions performed around the site
  * 651 visits
  * 432 visitors
  * 10 Twitter followers
  * 14 Facebook page fans
  * 2 Feature requests in UserVoice (completed)

I used real time analytics ([Woopra][4]) to monitor the users navigation flow (not that there are many pages to navigate around) and that helped me to optimize the experience, especially in terms of login flow. For example instead of asking the user to sign in with Foursquare or Facebook to share a trip I redesigned it to show the actual page and only prompt for login when the user needs to import Checkins. As they say a picture is worth a thousand words, but the actual thing is priceless.

Other new features since launch are:

  * Facebook Places Checkins support
  * Browse trips interface where a user can browser trips, days out and experiences by City, Country and latest
  * Allow trips to be downloaded as a .GPX file which users can open with Google Earth and Garmin sat navs. Adding support for other geo formats for other satnavs won&#8217;t be hard either.
  * A lot of polish and fixes in the backend. Thanks god for [ELMAH][5], which helped me log all problems around the site.

What&#8217;s next?

I am on a cross road at the moment as I need:

  1. A web design (well overdue)
  2. Someone to bounce ideas off and vice-versa (co-founder?) in which direction to take Triply as I think there are many directions Triply can go. For example I can make it the bit.ly for sharing trips/days out, etc or I can make it a trip diary type of thing by pulling and displaying photos of the trip from Flickr (using EXIF location data and search), etc. All fun ideas.

But how do both of those fit together? Doesn&#8217;t the design depend on the future features and vice-versa?

 [1]: {{ site.url }}/2011/05/02/introducing-triply-for-foursquare/ "Introducing Triply for Foursquare"
 [2]: http://triply.net
 [3]: https://foursquare.com/app/triply
 [4]: http://woopra.com
 [5]: http://code.google.com/p/elmah/