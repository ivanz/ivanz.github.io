---
title: Xgl and gdm
author: Ivan Zlatev
layout: post
permalink: /2006/03/13/xgl-and-gdm/
views:
  - 8
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 2113734
categories:
  - Coding
---
I installed Xgl on my SuSE 10.0 following[ the notes from the SuSE wiki][1]. I can say I am really impressed from the performance on my tablet pc. I have a Toshiba M200 Tablet PC, 1.7ghz, 512 ram, nvidia g-force fx 5200 32mb. It runs perfectly and does not slow my system in any way. Now the only thing that the notes were missing is how to make gdm load Xgl instead of X. This can be done by:

1) Open the /etc/opt/gnome/gdm/gdm.conf

2) Uncomment *1=Standard* and replace in *[server-Standard]* the *command=&#8230;.* with *command=/usr/X11R6/bin/Xgl -accel glx:pbuffer -accel xv:fbo -audit 0 *, where all the arguments are the correct for your video card. Log out and Log in and that&#8217;s it.

3) To run compiz after loging in:

> gnome-window-decorator &#038; compiz &#8211;replace gconf decoration wobbly fade minimize cube rotate zoom scale move resize place menu switcher &#038;

 [1]: http://en.opensuse.org/Using_Xgl_on_SUSE_Linux