---
title: Windows Mobile 5 device as a bluetooth modem for the Nokia N800
author: Ivan Zlatev
layout: post
permalink: /2007/09/14/windows-mobile-5-device-as-a-bluetooth-modem-for-the-nokia-n800/
views:
  - 165
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1320605
categories:
  - Coding
---
In order to use your Windows Mobile 5 device as a Bluetooth modem for your Nokia N800 some extra effort is required. Blame Microsoft for that, because they are the ones who removed Bluetooth DUN (supported by all phones on the market?) and forced usage of Bluetooth PAN (only MS Windows Mobile devices and PCs?), which being a non-standard connection is not yet supported by the Maemo connection manager.

Get *ssh *and* xterm* from the <a href="http://maemo.org/community/wiki/applicationrepositories/" target="_blank">Maemo Software Repositories</a>. You might have to enable <a href="http://maemo.org/community/wiki/ApplicationManagerRedPillMode" target="_blank">Red Pill Mode</a>. The root password is &#8220;rootme&#8221;.

Firstly one has to create a dummy connection (required only once) via:  
`ssh root@localhost<br />
gconftool -s -t string /system/osso/connectivity/IAP/DEFAULT/type DUMMY`

From that point on one can use the following two commands to establish the connection with the Windows Mobile Device.

`pand -Q10 -n<br />
udhcpc -i bnep0`