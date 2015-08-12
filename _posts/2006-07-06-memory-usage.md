---
title: Memory Usage
author: Ivan Zlatev
layout: post
permalink: /2006/07/06/memory-usage/
views:
  - 34
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1338235
categories:
  - Coding
---
Check this out &#8230; A more or less standart session on my SuSE 10.1 tablet pc/convertible and the memory usage:  
1x GNOME 2.12.2  
3x GNU Emacs  
3x Nautilus 2.12.2 on local  
1x GNOME Terminal 2.12.0 with 3-4 tabs  
1x Opera 9.00  
1x Kate 2.5.1  
1x Evolution 2.6.0  
1x VMware with M$ Windows XP Pro, running M$ Visual C# Express 2005  
[[Image:software/mem-usage.png|center]]

More details:<!--more-->

<pre>USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.0    716    68 ?        S    18:27   0:01 init [5]  
root         2  0.0  0.0      0     0 ?        SN   18:27   0:00 [ksoftirqd/0]
root         3  0.0  0.0      0     0 ?        S   18:27   0:00 [events/0]
root         4  0.0  0.0      0     0 ?        S   18:27   0:00 [khelper]
root         5  0.0  0.0      0     0 ?        S   18:27   0:00 [kthread]
root         7  0.0  0.0      0     0 ?        S   18:27   0:00 [kblockd/0]
root         8  0.0  0.0      0     0 ?        S   18:27   0:00 [kacpid]
root       136  0.0  0.0      0     0 ?        S    18:27   0:00 [pdflush]
root       137  0.0  0.0      0     0 ?        S    18:27   0:00 [pdflush]
root       139  0.0  0.0      0     0 ?        S   18:27   0:00 [aio/0]
root       138  0.0  0.0      0     0 ?        S    18:27   0:00 [kswapd0]
root       345  0.0  0.0      0     0 ?        S   18:27   0:00 [cqueue/0]
root       346  0.0  0.0      0     0 ?        S   18:27   0:00 [kseriod]
root       380  0.0  0.0      0     0 ?        S   18:27   0:00 [kpsmoused]
root       782  0.0  0.0      0     0 ?        S   18:27   0:00 [reiserfs/0]
root       862  0.0  0.0   1860   352 ?        Ss  18:27   0:00 /sbin/udevd --daemon
root      1521  0.0  0.0      0     0 ?        S   18:27   0:00 [khubd]
root      1540  0.0  0.0      0     0 ?        S    18:27   0:00 [shpchpd_event]
root      1557  0.0  0.0      0     0 ?        S   18:27   0:01 [ipw2200/0]
root      1714  0.0  0.0      0     0 ?        S    18:27   0:00 [pccardd]
root      2357  0.0  0.0   1800   500 ?        Ss   18:27   0:00 /sbin/resmgrd
100       2364  0.0  0.1   3548   916 ?        Ss   18:27   0:13 /usr/bin/dbus-daemon --system
root      2365  0.0  0.0   1520   488 ?        Ss   18:27   0:00 /sbin/acpid
root      2371  0.0  0.0   1656   308 ?        Ss   18:27   0:00 /sbin/klogd -c 1 -x -x
root      2378  0.0  0.0   1892   508 ?        Ss   18:27   0:00 /sbin/syslog-ng
root      2455  0.0  0.1   2420   624 ?        Ss   18:27   0:00 /usr/sbin/dhcdbd --system
root      2460  0.0  0.3   4456  1692 ?        Ss   18:27   0:09 /usr/sbin/hald --daemon=yes --retain-privileges
root      2461  0.0  0.1   2784   996 ?        Ss   18:27   0:00 /usr/sbin/NetworkManagerDispatcher
mdnsd     2505  0.0  0.1   1972   648 ?        Ss   18:27   0:00 /usr/sbin/mdnsd -f /etc/nss_mdns.conf -b
nobody    2523  0.0  0.0   1556   376 ?        Ss   18:27   0:00 /sbin/portmap
root      2540  0.0  0.3  20240  1748 ?        Ssl  18:27   0:00 /usr/sbin/NetworkManager
root      2543  0.0  0.0   9860   452 ?        Ssl 18:27   0:00 /sbin/auditd
root      2545  0.0  0.0      0     0 ?        S   18:27   0:00 [kauditd]
root      2710  0.0  0.1   1816   680 ?        S    18:27   0:00 hald-addon-acpi
root      2852  0.0  0.1 106476   964 ?        Ssl  18:27   0:00 /usr/sbin/nscd
lp        2863  0.0  0.4   7560  2516 ?        Ss   18:27   0:00 /usr/sbin/cupsd
root      2995  0.0  0.2   5416  1484 ?        Ss   18:28   0:00 /usr/lib/postfix/master
postfix   3031  0.0  0.2   5412  1432 ?        S    18:28   0:00 qmgr -l -t fifo -u
root      3058  0.0  0.2   4288  1452 ?        S    18:28   0:06 /usr/sbin/powersaved -d -f /var/run/acpid.socket -v 3
root      3079  0.0  0.0   1820   480 ?        Ss   18:28   0:00 /usr/sbin/cron
root      3114  0.0  0.1   4504   692 ?        Ss   18:28   0:00 /usr/sbin/smpppd
root      3161  0.1  0.2   4200  1048 ?        S    18:28   0:26 /usr/sbin/iald
root      3163  0.0  0.1   4952   636 ?        Ss   18:28   0:00 /usr/sbin/sshd -o PidFile=/var/run/sshd.init.pid
root      3164  0.0  0.4  11368  2292 ?        S    18:28   0:00 /opt/gnome/sbin/gdm
root      3172  0.0  0.0   1548   336 ?        Ss   18:28   0:00 startpar -f -- xdm
root      3197  0.0  0.0   1368   108 ?        S    18:28   0:00 /usr/bin/vmnet-bridge -d /var/run/vmnet-bridge-0.pid /dev/vmnet0 eth0
root      3213  0.0  0.0   1368   108 ?        S    18:28   0:00 /usr/bin/vmnet-netifup -d /var/run/vmnet-netifup-vmnet1.pid /dev/vmnet1 vmnet1
root      3222  0.0  0.0   1364   108 ?        S    18:28   0:00 /usr/bin/vmnet-netifup -d /var/run/vmnet-netifup-vmnet8.pid /dev/vmnet8 vmnet8
root      3263  0.0  0.0   1620   440 ?        Ss   18:28   0:00 /usr/bin/vmnet-natd -d /var/run/vmnet-natd-8.pid -m /var/run/vmnet-natd-8.mac -c /etc/vmware/vmnet8/nat/nat.conf
root      3297  0.0  0.1   1680   524 ?        Ss   18:28   0:00 /usr/bin/vmnet-dhcpd -cf /etc/vmware/vmnet8/dhcpd/dhcpd.conf -lf /etc/vmware/vmnet8/dhcpd/dhcpd.leases -pf /var/run/vmnet-dhcpd-vmnet8.pid vmnet8
root      3319  0.0  0.0   1684   256 ?        Ss   18:28   0:00 /usr/bin/vmnet-dhcpd -cf /etc/vmware/vmnet1/dhcpd/dhcpd.conf -lf /etc/vmware/vmnet1/dhcpd/dhcpd.leases -pf /var/run/vmnet-dhcpd-vmnet1.pid vmnet1
root      3386  0.0  0.4  12164  2264 ?        S    18:28   0:00 /opt/gnome/sbin/gdm
root      3389  1.1  4.9  44800 25688 tty7     Ss+  18:28   3:49 /usr/X11R6/bin/X :0 -audit 0 -br -auth /var/lib/gdm/:0.Xauth -nolisten tcp vt7
root      3391  0.0  0.1   1956   656 tty1     Ss+  18:28   0:00 /sbin/mingetty --noclear tty1
root      3392  0.0  0.1   1956   636 tty2     Ss+  18:28   0:00 /sbin/mingetty tty2
root      3393  0.0  0.1   1956   632 tty3     Ss+  18:28   0:00 /sbin/mingetty tty3
root      3394  0.0  0.1   1956   636 tty4     Ss+  18:28   0:00 /sbin/mingetty tty4
root      3395  0.0  0.1   1960   632 tty5     Ss+  18:28   0:00 /sbin/mingetty tty5
root      3396  0.0  0.1   1960   636 tty6     Ss+  18:28   0:00 /sbin/mingetty tty6
root      3469  0.0  2.0  21932 10616 ?        Ss   18:28   0:01 /opt/gnome/bin/gnome-session
root      3511  0.0  0.0   2452   388 ?        Ss   18:28   0:00 /usr/bin/gpg-agent --sh --daemon
root      3512  0.0  0.1   4388   568 ?        Ss   18:28   0:00 ssh-agent /bin/bash /etc/X11/xinit/xinitrc
root      3516  0.0  0.1   3728   796 ?        Ss   18:28   0:00 dbus-daemon --fork --print-pid 8 --print-address 6 --session
root      3517  0.0  0.1   2728   624 ?        S    18:28   0:00 /usr/bin/dbus-launch --sh-syntax --exit-with-session /usr/X11R6/bin/gnome
root      3521  0.0  1.2  12348  6552 ?        S    18:28   0:01 /opt/gnome/lib/GConf/2/gconfd-2 4
root      3525  0.0  0.1   2376   680 ?        S    18:28   0:00 /opt/gnome/bin/gnome-keyring-daemon
root      3527  0.0  0.5   5580  2824 ?        Ss   18:28   0:00 /opt/gnome/lib/bonobo/bonobo-activation-server --ac-activate --ior-output-fd=17
root      3529  0.0  1.6  29056  8452 ?        Sl   18:28   0:05 /opt/gnome/lib/control-center-2.0/gnome-settings-daemon --oaf-activate-iid=OAFIID:GNOME_SettingsDaemon --oaf-ior-fd=25
root      3531  0.0  0.2   3148  1384 ?        Ss   18:28   0:00 /usr/bin/esd -terminate -nobeeps -as 2 -spawnfd 18
root      3533  0.0  0.6   5220  3464 ?        Ss   18:28   0:00 /usr/bin/esd -terminate -nobeeps -as 2 -spawnfd 18
root      3538  0.0  1.8  15112  9624 ?        Ss   18:28   0:17 metacity --sm-save-file 1151658221-3332-3845059074.ms
root      3545  0.0  4.2 134612 21808 ?        Ssl  18:28   0:19 nautilus --sm-config-prefix /nautilus-ilkgp5/ --sm-client-id 117f000002000115141760000000127790002 --screen 0 file:///root/Projects/mono-summer-2k6/design-time/src/System.Design/System.ComponentModel.Design file:///root/Projects/mono-summer-2k6/examples/hosting/csharp/DesignerHost file:///svn/mono/mcs
root      3550  0.1  3.6 131760 19052 ?        Ssl  18:28   0:36 gnome-panel --sm-config-prefix /gnome-panel-h1yZH6/ --sm-client-id 117f000002000115141759800000127790001 --screen 0
root      3557  0.0  0.7   9104  4044 ?        Sl   18:29   0:00 /opt/gnome/sbin/gnome-vfs-daemon --oaf-activate-iid=OAFIID:GNOME_VFS_Daemon_Factory --oaf-ior-fd=31
root      3559  0.0  3.0 134228 15968 ?        Ssl  18:29   0:04 gnome-terminal --sm-config-prefix /gnome-terminal-thFQD4/ --sm-client-id 117f000002000115165624200000032630005 --screen 0 --window-with-profile-internal-id=Default --show-menubar --role=gnome-terminal-3560--153943762-1151656243 --active --geometry 80x24 --working-directory /root --zoom 1
root      3566  0.0  1.6  66868  8612 ?        Ssl  18:29   0:01 /opt/gnome/lib/evolution/2.6/evolution-alarm-notify --sm-config-prefix /evolution-alarm-notify-3DcuY3/ --sm-client-id 117f000002000115147570000000127790088 --screen 0
root      3575  0.0  1.6  20908  8552 ?        Ss   18:29   0:02 gnome-power-manager --sm-disable
root      3585  0.0  0.7  17656  4128 ?        Ss   18:29   0:01 gnome-volume-manager --sm-disable
root      3590  0.0  0.1   2288   616 ?        S    18:29   0:00 /opt/gnome/lib/nautilus/mapping-daemon
root      3601  0.0  2.0 120660 10784 ?        Ss   18:29   0:02 nm-applet
root      3604  0.0  1.3  78532  6972 ?        Sl   18:29   0:00 /opt/gnome/lib/evolution-data-server-1.2/evolution-data-server-1.6 --oaf-activate-iid=OAFIID:GNOME_Evolution_DataServer_CalFactory:1.2 --oaf-ior-fd=37
root      3621  0.0  0.1   2504   536 ?        S    18:29   0:00 gnome-pty-helper
root      3622  0.0  0.5  15188  2684 ?        Ss   18:29   0:02 gnome-screensaver
root      3623  0.0  0.3   3236  1692 pts/0    Ss   18:29   0:00 bash
root      3739  0.0  0.2   2756  1052 ?        S    18:30   0:00 /bin/sh /usr/lib/vmware/lib/wrapper-gtk24.sh /usr/lib/vmware/lib /usr/lib/vmware/bin/vmware /usr/lib/vmware/libconf
root      3756  0.2  1.9  13712  9860 ?        S    18:30   0:39 emacs /root/Projects/mono-summer-2k6/design-time/src/System.Design/System.ComponentModel.Design/SelectionService.cs
root      3766  0.0  3.4  37240 17684 ?        S    18:30   0:09 /usr/lib/vmware/bin/vmware
root      3767  0.0  0.0   2756   488 ?        S    18:30   0:00 /bin/sh /usr/lib/vmware/lib/wrapper-gtk24.sh /usr/lib/vmware/lib /usr/lib/vmware/bin/vmware /usr/lib/vmware/libconf
root      3769  9.1 41.4 299184 213856 ?       Sl  18:30  29:24 /usr/lib/vmware/bin/vmware-vmx -@ pipe=/tmp/vmware-root/vmx8c9fc9d589f1f255;vm=8c9fc9d589f1f255 /root/vmware/Windows XP Professional/Windows XP Professional.vmx
root      3863  0.1  1.8  13224  9356 ?        S    18:41   0:21 emacs /root/Projects/mono-summer-2k6/design-time/docs/selection-howto
root      3892  0.0  1.6 115928  8704 ?        S    18:53   0:03 /opt/gnome/lib/notification-daemon-1.0/notification-daemon
root      4073  0.0  4.5  37512 23600 pts/0    S+   20:41   0:06 kate
root      4075  0.0  1.1  22804  6000 ?        Ss   20:41   0:00 kdeinit Running...             
root      4078  0.0  1.0  22372  5676 ?        S    20:41   0:00 dcopserver [kdeinit] --nosid --suicide
root      4080  0.0  1.3  22388  7064 ?        S    20:41   0:00 klauncher [kdeinit]            
root      4082  0.0  1.8  25808  9684 ?        S    20:41   0:00 kded [kdeinit]                 
root      4085  0.0  1.1  23300  6116 ?        S    20:41   0:00 kio_file [kdeinit] file /tmp/ksocket-root/klauncher280bda.slave-socket /tmp/ksocket-root/kate1961ub.slave-socket
root      4158  0.0  1.6  12648  8740 ?        S    21:20   0:01 emacs /root/Projects/mono-summer-2k6/design-time/classlib-patches/SelectionTypes.diff
postfix   4317  0.0  0.3   5376  1644 ?        S    23:27   0:00 pickup -l -t fifo -u
root      4412  0.3  1.6  12380  8560 ?        S    23:47   0:01 emacs /root/Desktop/new file
root      4415  2.0  5.1  63584 26484 ?        S    23:51   0:02 /usr/lib/opera/9.0-20060616.6/opera
root      4425  0.0  0.0      0     0 ?        Z    23:51   0:00 [operapluginwrap] defunct
root      4428  1.8  5.0 287480 26040 ?        Sl   23:51   0:02 evolution-2.6
root      4435  0.2  1.8  27668  9672 ?        Sl   23:51   0:00 /opt/gnome/lib/evolution/2.6/evolution-exchange-storage --oaf-activate-iid=OAFIID:GNOME_Evolution_Exchange_Component_Factory:2.6 --oaf-ior-fd=41
root      4443  0.0  0.3   3236  1832 pts/1    Ss   23:51   0:00 bash
root      4489  0.0  0.1   2392   852 pts/1    R+   23:53   0:00 ps aux


             total       used       free     shared    buffers     cached
Mem:           504        497          6          0          8        315
-/+ buffers/cache:        173        331
Swap:         1027         28        999
</pre>