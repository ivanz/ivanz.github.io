---
title: Avoiding university work
author: Ivan Zlatev
layout: post
permalink: /2007/04/30/avoiding-university-work/
views:
  - 9
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1408404
categories:
  - Coding
---
Yeah avoiding university work is what I have been doing for tha past few days, which essentially resulted me working at 1am on a coursework. <img src="http://ivanz.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> But there are a few things that happened too.

Firstly I found out that there was an equalizer element checked in GStreamer CVS, which resulted a second effort to finish off the **Banshee Equalizer** and hopefuly this time it is going to be the last one. As I&#8217;ve blogged before, the infrastructure was layed a few months ago and I had patched Helix and made my Xine media engine to support equalization. The show stopper was gstreamer and the non-existence of a equalizer plugin. I found some code, which was actually shared across quite a lot around different media applications (xmms, vlc, amarok, etc), unfortunatly GPL licensed. The new element in gstreamer&#8217;s CVS is LGPL which is a green light. You can monitor <a href="http://bugzilla.gnome.org/show_bug.cgi?id=426562" target="_blank">#426562</a> for progress.

Secondly I got tired of waiting for a fix to be released for the incredibly buggy (as in leaking memory + hanging) gnome-main-menu shipped with openSUSE 10.2. There has been a bug in bugzilla since 2006-12-15 (<a href="http://bugzilla.novell.com/show_bug.cgi?id=229190" target="_blank">#229190</a>)&#8230; That&#8217;s about 4 months since then. So I got my hands on that and cooked up my first RPMs ever (they work don&#8217;t be scared!). I backported the Factory to 10.2, which was quite challenging firstly because there have been some new macros introduced in Factory for gconf stuff and secondly and most importantly because I didn&#8217;t have any experience. **Now I am enjoying a very stable new gnome-main-menu**. <img src="http://ivanz.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> I have also decided to run a small RPM repository of mine &#8211; <a href="http://rpm.ivanz.com" target="_blank">http://rpm.ivanz.com</a> and continue learning about packaging. Hopefuly one day I will be more useful to openSUSE. I already go through bugzilla and to a bit of cleanup from time to time. Probably I can make this my hobby or something.

Meh&#8230; have to continue writing coursework now&#8230; <img src="http://ivanz.com/wp-includes/images/smilies/frownie.png" alt=":(" class="wp-smiley" style="height: 1em; max-height: 1em;" />

P.S: I found out that uvcvideo in openSUSE Factory **includes my isight.patch**. Sweet!

UPDATE: rpm.ivanz.com is no more. I have got an account for openSUSE Build Service now. newer gnome-main-menu is on GNOME:Unstable and my stuff is [here][1].

 [1]: http://ivanz.com/projects/opensuse