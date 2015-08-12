---
title: Resize a VirtualBox Virtual Disk HowTo
author: Ivan Zlatev
layout: post
permalink: /2009/04/30/resize-a-virtualbox-virtual-disk-howto/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 14082
dsq_thread_id:
  - 17052738
categories:
  - Coding
tags:
  - HowTo
  - Virtualization
---
It is not possible to increase the maximum virtual size of a VirtualBox VDI HDD once it is created. The only way I found to do it is to create a new larger VirtualBox HDD and use GParted to copy the old one to the new one. The steps to do that are:

1. Create a new VirtualBox HDD with the desired size and add it as a secondary HDD (Primary Slave) in the Virtual Machine HDD settings.

2. Download the GParted LiveCD and boot from it inside the Guest &#8211; <http://gparted.sourceforge.net/>

3. Right click on the partitions of the original HDD in GParted and click &#8220;*Copy*&#8220;. Select the second HDD and &#8220;Paste&#8221;.  This will ask to create a &#8220;*msdos*&#8221; partition table first so click *&#8220;OK&#8221; *and &#8220;*Paste*&#8221; again. Adjust partition sizes as you wish and hit &#8220;*Apply*&#8220;.

4. Mark the new partition as bootable in right click -> &#8220;*Manage Flags*&#8221; -> tick &#8220;*boot*&#8220;. Once done shutdown the VM and release the gparted Live CD.

5. Because the HDD characteristics have changed you might not be able to boot at this point. Insert the Windows/Linux OS installation CD/DVD and run in repair mode. For Windows this would be the recovery console and the &#8220;*fixboot c:*&#8221; or fixmbr commands. On Linux just reinstall the boot loader.

That&#8217;s it. It&#8217;s not very convenient doing this but at least it works.

P.S: If you get an error &#8220;FATAL: INT18 Boot Failure&#8221; you have forgotten to mark the new partition as bootable.