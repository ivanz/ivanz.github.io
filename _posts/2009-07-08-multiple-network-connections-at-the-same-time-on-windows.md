---
title: Multiple Network Connections at the Same Time on Windows
author: Ivan Zlatev
layout: post
permalink: /2009/07/08/multiple-network-connections-at-the-same-time-on-windows/
ratings_users:
  - 6
ratings_score:
  - 28
ratings_average:
  - 4.67
views:
  - 43659
dsq_thread_id:
  - 
categories:
  - Coding
tags:
  - Bugs
  - HowTo
  - Windows
---
In my scenario I have one Wireless connection for my Internet and one LAN connection to a small private network of my own with my NAS, PS3 and TV and I want to have them both at the same time. It was a major pain to get this setup working on Windows. When I had both connections enabled my Internet wasn&#8217;t working because Windows was routing through the LAN connection even though the Wireless connection had a higher priority set in *Network Connections* &#8211;> *Advanced* menu &#8211;> *Advanced settings.*

The solution is to go into the *Properties *of each connection (right click on it) &#8211;> select *Internet Protocol (TCP/IP) *&#8211;> click *Properties *-> click *Advanced *&#8211;> uncheck *Automatic metric *in the bottom and set a number between 1 and 9999 where the smaller the number the higher the connection priority. I have set my Wireless Internet connection to 1 and my LAN connection to 9999 and that works.

I hope this post will save someone else&#8217;s precious time in the future.