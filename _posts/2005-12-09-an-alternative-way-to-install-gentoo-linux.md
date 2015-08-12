---
title: An alternative way to install Gentoo Linux
author: Ivan Zlatev
layout: post
permalink: /2005/12/09/an-alternative-way-to-install-gentoo-linux/
views:
  - 95
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1575751
categories:
  - Coding
---
I wanted to install Gentoo on a tablet pc without cd-rom, floppy, no network or usb boot capitabilities. All i had was preinstalled Windows XP Tablet Edition. My first option was to remove the harddisk and use it with another pc to install gentoo on it, but well I didn&#8217;t want to touch the hardware of the tablet pc, so here is how I installed gentoo:

1) Download http://ext2fsd.sourceforge.net/ . This is a system driver for Win32 for reading and writing to ext2 partitions. It even has an option to mount partition as a normal windows drive.

2) Use any tool for windows to partition your harddrive and make one more small partition&#8230; like 1gb ext2 type.

3) mount the small partition with mount from ext2fsd

4) Download http://www.geocities.com/lode_leroy/grubinstall/ . This is a grub installer for win32.

5) Install grub

6) Download gentoo minimal installation cd (or universal one, but then the small partition I was talking about should be bigger)

7) Unpack the .iso in the mounted small partition

8) Reboot and choose GRUB

9) Type in

> root (your hd number, small\_partition\_number)  
> kernel /isolinux/gentoo initrd=gentoo.igz root=/dev/ram0 init=/linuxrc looptype=squashfs loop=/livecd.squashfs cdroot=/dev/small_partition dokeymap  
> initrd /isolinux/gentoo.igz  
> boot

10) You&#8217;ve just booted the installation disk using the small partition. Continue the installation as a normal gentoo installation.