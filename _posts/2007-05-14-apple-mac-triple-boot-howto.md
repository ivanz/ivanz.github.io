---
title: Apple Mac Triple Boot HowTo
author: Ivan Zlatev
layout: post
permalink: /2007/05/14/apple-mac-triple-boot-howto/
views:
  - 4979
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1315117
categories:
  - Coding
tags:
  - HowTo
---
**Introduction**

In this HowTo I will cover how to configure a triple boot Apple Mac machine to boot openSUSE, Windows Vista and Mac OS X.

**The Problems**

Apple Macs are <a href="http://en.wikipedia.org/wiki/Extensible_Firmware_Interface" target="_blank">EFI</a> systems and as such they do not have BIOS. Even though EFI does support the standard partition scheme (<a href="http://en.wikipedia.org/wiki/Master_boot_record" target="_blank">MBR</a>), Apple is using the added support for a GUID partition table (<a href="http://en.wikipedia.org/wiki/GUID_Partition_Table" target="_blank">GPT</a>). GPT has a limit of 4 partitions and 1 is already used.

Linux supports the GPT and can boot by using <a href="http://elilo.sf.net" target="_blank">elilo</a> as a bootloader. However the lack of BIOS will not allow the video card drivers to operate properly.

Windows doesn&#8217;t support GPT.

**The Solutions**

<span style="text-decoration: underline;">Video</span>

Apple has updated the firmware to allow emulation of BIOS for its <a href="http://www.apple.com/macosx/bootcamp/" target="_blank">Boot Camp</a> tool to work. It should be noted that while Boot Camp offers an easy way to set up a dual boot Windows/OS X machine it does not come in handy for the purpose of this HowTo.

<span style="text-decoration: underline;">Partitioning</span>

While GPT is a new scheme, it doesn&#8217;t fully replace the MBR, but rather keeps it in the beginning for compatibility reasons. Fortunately one can abuse that and create a MBR partitioning table to match the GPT one. It is very important that the MBR partioning table is kept in sync with the GPT one, because else it will provide false information for the actual physical layout to the software reading it.

Since there are only 3 free GPT partition table slots one can install the three OSes. Linux works just fine with swap on a file in /.

<span style="text-decoration: underline;">Booting and MBR/GPT syncing</span>

There is an awesome tool available called <a href="http://refit.sourceforge.net/" target="_blank">rEFIt</a>. It includes is not only a boot loader, but also has a built in MBR/GPT partition synchronization tool. It automatically recognizes bootable OSes by scanning the partitions (also scans external devices).

**The HowTo**

<span style="text-decoration: underline;">Partitioning</span>

Boot from your Mac OS X CD 1 and use Disk Utility from the menu on top to create one partition for Mac OS X, one Unix type partition and one FAT32 type partition. It appears that <span style="text-decoration: underline;">it is crucial to have the Windows partition as the last one</span>, else Vista will complain and won&#8217;t even install and Windows XP will install, but will throw missing hal.dll error and won&#8217;t boot.

<span style="text-decoration: underline;">Install Mac OS X</span>

Install Mac OS X and update it to the latest version. Reboot and install rEFIt. Reboot and use the sync tool (called &#8220;Partition Tool&#8221; in the menu) in the rEFIt menu to sync MBR to GPT.

<span style="text-decoration: underline;">Install Windows Vista (setup NTFS partition)<br /> </span>

In Vista&#8217;s setup select the FAT32 partition, format it and install there. After the first reboot during the installation use rEFIt sync tool again. This is just to get the NTFS partition in place. Do not finish off the installation now, but rather procceed to the Linux installation. Windows won&#8217;t be able to boot after the sync anyway.

<span style="text-decoration: underline;">Install Linux (openSUSE)</span>

Crucial elements during the installation of openSUSE before the first reboot:

  * Install Linux (yes, everything) on the single UNIX type partition. Use custom partitioning to format the UNIX type partition to ext3 and set mount point to /. Do not create a swap partition and do not even think about resizing partitions.
  * Set the boot loader to LILO, because GRUB will fail to install.
  * Install the boot loader in the boot record for the partition. REFIt can boot it fine and also the master boot record will be free for Windows later on.
  * Remove the Windows entry from the lilo menu, because it causes LILO&#8217;s installation to fail.

Unfortunatly for unknown reasons to me after the first reboot during the installation the MBR partitioning table will be gone, so one has to use rEFIt to resync GPT and MBR. This of course will break lilo&#8217;s previous setup, so we have to reinstall it before continuing the installation. This can be done by boot from the openSUSE cd/dvd again and once the setup has loaded Ctrl + Fx to a terminal. From that point on we have to mount the partition where we have Linux installed and rerun lilo:

    mkdir /media/suse;
    mount /dev/sda3 /media/suse
    mount --bind /dev /media/suse/dev
    chroot /media/suse
    lilo -v

Reboot and resync GPT/MBR once more (so that rEFIT sets proper partition types) and then reboot for the last time. You should see OS X, Windows (won&#8217;t boot) and Linux icons in the rEFIt menu now. Select the Linux one and finish your openSUSE installation.

<span style="text-decoration: underline;">Install Windows Vista</span>

Run the Windows Vista installation from the start (again by formatting the partition) to end now. Do not sync MBR/GPT after!

**Additional Notes**

<span style="text-decoration: underline;">parted, fdisk, Boot Camp<br /> </span>

Do not use *parted* nor *fdisk* once you have a working system. I have had very bad experiences with both of those. As bad as in missing GPT and/or MBR partition table after. Do not use Boot Camp as well.

<span style="text-decoration: underline;">Linux swap on a file</span>

It works fine and can be done as follows. Example sets 2gb swap.

    dd if=/dev/zero of=/swap bs=1024 count=2097152
    mkswap /swap
    swapon /swap
    echo "/swap swap swap pri=1 0 0" >> /etc/fstab

<span style="text-decoration: underline;">Linux Built-in iSight WebCam Driver</span>

<a href="{{ site.url }}/projects/linux-kernel/" target="_blank">Here</a> or possibly supplied by your distribution of choice as uvcvideo patched with my isight patch.

<span style="text-decoration: underline;">Apple Mac Windows Driver CD iso</span>

After installing Boot Camp the driver cd image is here: /Applications/Utilities/Boot Camp Assistant.app/Contents/Resources/DiskImage.dmg.

**Have fun!**