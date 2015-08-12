---
title: Support for 100+ webcams in Linux 2.6.27 (USB Video Class Driver)
author: Ivan Zlatev
layout: post
permalink: /2008/07/01/support-for-100plus-webcams-in-linux-2627-usb-video-class-driver/
views:
  - 1223
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1512776
categories:
  - Coding
---
**Update:** <span style="text-decoration: underline;">The driver actually managed to get squeezed in in 2.6.26-rc</span><span style="text-decoration: underline;">9.</span>

According to <a href="http://lists.berlios.de/pipermail/linux-uvc-devel/2008-June/003682.html" target="_blank">a </a><a href="http://lists.berlios.de/pipermail/linux-uvc-devel/2008-June/003682.html" target="_blank">thread</a> on the Linux <a href="http://linux-uvc.berlios.de" target="_blank"><em>uvcvideo</em> driver</a> mailing list if everything goes well we will see it included in the 2.6.27 kernel!

What is this uvcdriver and why is this **great news**? According to Wikipedia:

> &#8220;The USB video device class (also USB video class or UVC) is a USB device class that describes devices capable of streaming video like webcams, digital camcorders, analog video converters, television tuners, and still-image cameras.&#8221;

So, yeah, *uvcvideo* is a driver for the above for Linux.

And this is **great news**, because:

  1. UVC is a standard and according to a source from the mentioned thread it is required for Microsoft Vista certification, so hardware vendors &#8220;have to&#8221; implement.
  2. It means <span style="text-decoration: underline;">out of the box support</span> for all those standard-compliant webcams, which is pretty much all of the webcams integrated in laptops it seems. Also most new USB webcams(?).
  3. 100+ webcams are already reported to work out of the box and probably twice as much work as well, but aren&#8217;t reported to the author of the driver.

Kudos to <span style="text-decoration: underline;">Laurent Pinchart</span> the main developer of *uvcdriver*.