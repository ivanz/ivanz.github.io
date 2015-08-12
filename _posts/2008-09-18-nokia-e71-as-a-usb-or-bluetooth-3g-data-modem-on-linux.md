---
title: Nokia E71 as a USB or Bluetooth 3G Data Modem on Linux
author: Ivan Zlatev
layout: post
permalink: /2008/09/18/nokia-e71-as-a-usb-or-bluetooth-3g-data-modem-on-linux/
views:
  - 17325
ratings_users:
  - 2
ratings_score:
  - 4
ratings_average:
  - 2
aktt_notify_twitter:
  - no
dsq_thread_id:
  - 4722430
categories:
  - Coding
tags:
  - HowTo
  - Linux
---
1.) Install *wvdial*  
2.) Add the following to */etc/wvdial.conf* and adjust it for your operator settings:

<pre style="padding-left: 60px;"><code>[Dialer nokia-usb]
Modem = /dev/ttyACM0
Baud = 3600000
Init1 = ATZ
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2
Init3 =
Modem Type = USB Modem
Area Code =
Phone = *99#
Username = ppp
Password = ppp
Ask Password = 0
Dial Command = ATDT
Stupid Mode = 1
Compuserve = 0
Force Address =
Idle Seconds = 0
DialMessage1 =
DialMessage2 =
ISDN = 0
Auto DNS = 1
New PPPD = yes

[Dialer nokia-bluetooth]
Modem = /dev/rfcomm0
Baud = 3600000
Init1 = ATZ
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2
Init3 =
Area Code =
Phone = *99#
Username = ppp
Password = ppp
Ask Password = 0
Dial Command = ATDT
Stupid Mode = 1
Compuserve = 0
Force Address =
Idle Seconds = 0
DialMessage1 =
DialMessage2 =
ISDN = 0
Auto DNS = 1
New PPPD = yes</code></pre>

3.) For USB when you connect the phone **<span style="text-decoration: underline;">set it to &#8220;PC Suite&#8221; mode</span> **(else the modem won&#8217;t appear) and then:

<pre style="padding-left: 60px;">su -c "wvdial nokia-usb"</pre>

4.) For Bluetooth, where ### is the Bluetooth Address of you phone

<pre style="padding-left: 60px;">su -c "rfcomm release 0; rfcomm bind 0 ### `sdptool search \
--bdaddr=### dun | grep -i Channel | cut -c14-14` && wvdial nokia-bluetooth"</pre>

5.) Workaround for broken newer versions of NetworkManager where the DNS won&#8217;t be set properly can be found at <http://seife.kernalert.de/blog/2008/12/11/using-dialup-with-111-if-networkmanager-does-not-handle-your-device/>