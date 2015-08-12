---
title: 'iNotesMaker &#8211; Canvas'
author: Ivan Zlatev
layout: post
permalink: /2005/12/20/inotesmaker-canvas/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 9
dsq_thread_id:
  - 1713319
categories:
  - Coding
---
Probably a month ago I started a project for a OneNote alike program for Linux using Mono. I tried to implement it in many ways, but I wasn&#8217;t really satisfied, because the drawing quality was poor and the object managment too. I used Gdk, after that System.Drawing on a GTK surface (actually X11 one) both using points and lines and points and bezier curves. The result wasn&#8217;t what I was expecting. I saw the Gnome namespace then, especially the Gnome.Canvas one, which seemed to be exactly what I needed &#8211; it had strokes and the managment of the different objects was really easy to do with the library. What happened was that I talked with some of the Mono guys about a problem I experienced while I was coding with Gnome and they told me that this lib is no more supported. Cheers! But also I&#8217;ve been told that the Cairo guys were working on a Canvas for their library or something. So I have the choice to wait for Cairo&#8217;s Canvas or implement my own in C#, using Cairo. I need to have a talk about the roadmap of the Cairo Canvas, so I will know if it will be consistent to wait for it. Probably I will implement my own&#8230; we will see.