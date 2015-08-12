---
title: The Patch again
author: Ivan Zlatev
layout: post
permalink: /2005/12/06/the-patch-again/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 9
dsq_thread_id:
  - 2175627
categories:
  - Coding
---
After discussing the System.Drawing.Graphics patch in my previous post with Duncan Mak we found it better to actually patch the libgdiplus code, because the Graphics.cs acts only as a wrapper and no calculations should be done there. Also we spotted that the &#8220;offset&#8221; parameter is ignored too. In the following days he will fix all these things.