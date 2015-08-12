---
title: openSUSE 10.2 Network Boot and Installation HOWTO
author: Ivan Zlatev
layout: post
permalink: /2007/01/31/opensuse-102-network-boot-and-installation-howto/
views:
  - 3759
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1312695
categories:
  - Coding
tags:
  - HowTo
  - openSUSE
---
**Introduction**

The **P**reboot E**x**ecution **E**nvironment (in short PXE) is a protocol that enables a computer to be booted on the network. PXE is saved in ROM on newer network cards and, depending on the boot sequence configured in the computer&#8217;s BIOS, loaded and executed after turning on the computer.

This HOWTO describes how to setup an openSUSE 10.2 machine to act as a network boot and install server. I have used this method to install openSUSE 10.2 on my Tablet PC (Toshiba Portege M200).

The plan is to have a DHCP server assigning IPs, TFTP server for PXELINUX to be loaded (to load the openSUSE installer) and an NFS share to supply the installation files.

**Prerequests**

  * PXE boot capable machine
  * Packages &#8211; all of them can be found on the DVD or in the main openSUSE repository. 
      * dhcp-server &#8211; to automatically asign IPs.
      * tftp &#8211; to share the boot image
      * syslinux &#8211; to supply the boot image (pxelinux.0)
      * yast2-tftp-server &#8211; Graphical configurator for a TFTP server
      * yast2-dhcp-server &#8211; Graphical configurator for a DHCP server
      * yast2-nfs-server &#8211; Graphical configurator for a NFS server.
  * A network cable
  * openSUSE 10.2 DVD

All of the graphical configurators used in this HOWTO can be found in Yast2 in the Network Services section.

[[Image:software/suse-netboot-screens/yast2-network-services.png|center|640|480]]

**Network Interface Setup**

For the purpose of this HOWTO I will set and use the IP 192.168.69.1 for the network interface. This can be configured in Yast in the Network Devices section using the Network Card configurator.

[[Image:software/suse-netboot-screens/card-setup-1.png|center|640|480]]

[[Image:software/suse-netboot-screens/card-setup-2.png|center|640|480]]

[[Image:software/suse-netboot-screens/card-setup-3.png|center|640|480]]

The last step is to bring the interface up:

    ifconfig eth0 up 192.168.69.1

**NFS Configuration**

For the purpose of this HOWTO I am going to share the openSUSE 10.2 DVD as shown. This is going to be the installation source supplied for the installation as you will see later.

[[Image:software/suse-netboot-screens/nfs-1.png|center|640|480]]

[[Image:software/suse-netboot-screens/nfs-2.png|center|640|480]]

**TFTP Configuration**

Time to configure PXELINUX to be offered by the TFTP server.

[[Image:software/suse-netboot-screens/tftp.png|center|640|480]]

We will also tell PXELINUX to boot the linux kernel we will copy from the openSUSE 10.2 DVD.

    cp /usr/share/syslinux/pxelinux.0 /tftpboot
    cp /media/SU1020.001/boot/i386/loader/initrd /tftpboot/
    cp /media/SU1020.001/boot/i386/loader/linux /tftpboot/
    mkdir /tftpboot/pxelinux.cfg
    cd /tftpboot/pxelinux.cfg
    touch default
    chmod 777 default

The boot configuration in the file *default* should be as follows:

    default linux
    label linux
    kernel linux
    append initrd=initrd install=nfs://192.168.69.1/media/SU1020.001
    implicit         0
    display         message
    prompt          1
    timeout         1
    

**DHCPD Configuration**

[[Image:software/suse-netboot-screens/dhcpd-1.png|center|640|480]]

[[Image:software/suse-netboot-screens/dhcpd-2.png|center|640|480]]

[[Image:software/suse-netboot-screens/dhcpd-3.png|center|640|480]]

[[Image:software/suse-netboot-screens/dhcpd-4.png|center|640|480]]

The configurator will create the */etc/dhcpd.conf* file, but with this configuration the dhcp server won&#8217;t know about the existance of the tftp server, so we have to add 2 the *next-server* and *filename* options. Finally the configuration should look like this:

    option domain-name "mydomainname";
    ddns-update-style none;
       default-lease-time 14400;
       subnet 192.168.69.0 netmask 255.255.255.0 {
       range 192.168.69.2 192.168.69.254;
       default-lease-time 14400;
       max-lease-time 172800;
       next-server 192.168.69.1;
       filename "pxelinux.0";
    }

[[Image:software/suse-netboot-screens/dhcpd-5.png|center|640|480]]

**Booting**

You can now boot the machine and the result should be something simliar to:

    Trying to load: pxelinux.cfg/01-88-99-aa-bb-cc-dd
    Trying to load: pxelinux.cfg/C000025B
    Trying to load: pxelinux.cfg/C000025
    ...
    Trying to load: pxelinux.cfg/default

and then the system should boot.

**References and More Information **

  * <a href="http://ivanz.com/wp-admin/post.php#%20ftp://download.intel.com/design/archives/wfm/downloads/pxespec.pdf" target="_blank">ftp://download.intel.com/design/archives/wfm/downloads/pxespec.pdf</a>
  * <a href="http://en.wikipedia.org/wiki/Tftp" target="_blank">http://en.wikipedia.org/wiki/Tftp</a>
  * <a href="http://en.wikipedia.org/wiki/Network_File_System_%28Sun%29" target="_blank">http://en.wikipedia.org/wiki/Network_File_System_%28Sun%29</a>
  * <a href="http://en.opensuse.org/SDB:Network_Installation_of_SuSE_Linux_via_PXE_Boot" target="_blank">http://en.opensuse.org/SDB:Network_Installation_of_SuSE_Linux_via_PXE_Boot</a>
  * <a href="http://syslinux.zytor.com/pxe.php" target="_blank">http://syslinux.zytor.com/pxe.php</a>