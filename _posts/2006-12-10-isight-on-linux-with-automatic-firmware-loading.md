---
title: iSight on Linux with automatic firmware loading
author: Ivan Zlatev
layout: post
permalink: /2006/12/10/isight-on-linux-with-automatic-firmware-loading/
views:
  - 115
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1536589
categories:
  - Coding
---
I&#8217;ve been using the patched linux-uvc module with iSight support by Ronald S. Bultje ([clicky][1]). It required me to run a separate program to load the firwmare and then load the module. I&#8217;ve coded up and integrated a firmware loader in the module, which will automatically extract and load the firmware from /lib/firmware using the firmware_class kernel interface when the device is detected. Said otherwise &#8230; **no more need to run *extract*.**

[Download driver + firmware + installation instructions][2].

I&#8217;ve also merged Ronald&#8217;s patch with linux-uvc SVN Head, but unfortunatly it doesn&#8217;t work, because of &#8220;&#8221;uvcvideo: Failed to query (130)  
UVC control 1 (unit 0) : 2 (exp. 26).&#8221; errors. But if you want to play you can get it from [here][3].

 [1]: http://blogs.gnome.org/view/rbultje/2006/10/27/0
 [2]: http://files.ivanz.com/projects/isight/linux-uvc-isight.tar.gz
 [3]: http://files.ivanz.com/projects/isight/isight.patch