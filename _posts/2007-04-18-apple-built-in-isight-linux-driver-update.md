---
title: Apple Built-In iSight Linux Driver Update
author: Ivan Zlatev
layout: post
permalink: /2007/04/18/apple-built-in-isight-linux-driver-update/
views:
  - 378
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1323675
categories:
  - Coding
---
I have updated the [iSight patch and bundle][1] to revision 94 of uvcvideo with updates to compile on newer kernels with the updates in Crypto API (thanks to J Sanon).

Update: I had taken out three & by accident thus breaking compilation on newer kernels. Fixed now. Thanks to D. Siegel for reporting.

 [1]: http://ivanz.com/projects/linux-kernel/