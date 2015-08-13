---
title: Skype, Wireless Internet, Windows XP SP2
author: Ivan Zlatev
layout: post
permalink: /2006/02/24/skype-wireless-internet-windows-xp-sp2/
views:
  - 475
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1316212
categories:
  - Coding
---
I am a GNU/Linux user, but I needed to use Skype on Windows yesterday&#8230; and then it began. I always got disconnected after a minute or so during my call, also my internet seemed to be blocked for a couple of minutes. It took me a bit to find out how to fix that. My configuration was:

  * Skype 2.0.0.81
  * Windows XP Service Pack 2
  * Intel(R) PRO/Wireless 2200BG Wireless Card
  * D-Link DSL-G604T Wireless Router
  * Bandwidth 512kbit down/ 256kbit up

The problems with this configuration are:

  1. Windows seems to check the connections every few minutes (may be even every single min?), which could cause a slow connection if the bandwidth is low as in my case.
  2. Windows XP SP2 has a limitation of only 10 TCP/IP connections or something (I am not exactly what it is), which can cause problems.
  3. The D-Link router blocks the internet connection, because it thinks you are running a port scan.

The ways to fix those issues are:

  1. Get the Intel PROSet/Wireless Software if you don&#8217;t have it. When you run it you can tell it to replace the Windows wireless agent. Configure your wireless internet connection and you&#8217;re done. If you want an icon to be shown in the task bar for your connection go to Tools -> Application Settings and click on &#8220;Show application icon in the taskbar&#8221;. This will fix 1.
  2. Get [[this]][1], run it and type &#8220;Y&#8221; for yes when it asks &#8220;Do you really want to change the limit to 50? [Y/N]&#8221;. This will fix 2.
  3. Open the control panel of the D-Link router and log in. Go to Advanced -> Port Forwarding. Select your IP, select &#8220;Audio/Video&#8221; from the categories and then add Net2Phone. Click on &#8220;Apply&#8221; and this will fix 3.
  4. Reboot.

Hopefuly now you have a properly working Skype with Windows XP SP2, using wirless connection.

 [1]: {{ site.url }}/files/blog/Windows%20XP%20SP2%20TCP_IP%20Patcher.exe