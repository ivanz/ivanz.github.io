---
title: 'openSUSE 11.1 &#8211; The Workarounds'
author: Ivan Zlatev
layout: post
permalink: /2009/01/13/opensuse-111-the-workarounds/
aktt_notify_twitter:
  - no
ratings_users:
  - 1
ratings_score:
  - 5
ratings_average:
  - 5
views:
  - 2074
dsq_thread_id:
  - 9849864
categories:
  - Diary
tags:
  - Bugs
  - openSUSE
---
Due to popular demand (basically people asking me for the workarounds for the openSUSE 11.1 problems listed in my previous post) I have decided to make the workarounds available in this post.

### DVD Burning

Edit as root */usr/share/PolicyKit/policy/org.freedesktop.hal.device-access.policy *and change

<pre>&lt;action id="org.freedesktop.hal.device-access.removable-block"&gt;
  &lt;description&gt;Directly access removable block devices&lt;/description&gt;
  &lt;message&gt;System policy prevents access to removable block devices&lt;/message&gt;
  &lt;defaults&gt;
    &lt;allow_inactive&gt;no&lt;/allow_inactive&gt;
    &lt;allow_active&gt;no&lt;/allow_active&gt;
  &lt;/defaults&gt;
&lt;/action&gt;</pre>

to

<pre>&lt;action id="org.freedesktop.hal.device-access.removable-block"&gt;
  &lt;description&gt;Directly access removable block devices&lt;/description&gt;
  &lt;message&gt;System policy prevents access to removable block devices&lt;/message&gt;
  &lt;defaults&gt;
    &lt;allow_inactive&gt;yes&lt;/allow_inactive&gt;
    &lt;allow_active&gt;yes&lt;/allow_active&gt;
  &lt;/defaults&gt;
&lt;/action&gt;</pre>

Restart.

### Skype

Delete/move as root the file */etc/asound-pulse.conf* and reboot.

### wvdial

A neat workaround is [described here][1].

### **VirtualBox USB**

1. Uninstall the VirtualBox packages openSUSE provides in its repositories

2. Download and install the openSUSE RPM of the non-OSE edition (full edition, but not fully open source) from http://virtualbox.org

3. Add to */etc/fstab* :

<pre>/sys/bus/usb/drivers /proc/bus/usb usbfs devgid=1000,devmode=664 0 0</pre>

4. Add to */etc/init.d/boot.local* :

<pre>mount -a</pre>

Reboot.

### Dropbox

Create a *~/.xinitrc* file with the following content:

<pre>xhost local:root
/etc/X11/xinit/xinitrc</pre>

### X Annoying Beep

In */etc/X11/xorg.conf* change

<pre>Option       "ZapWarning" "on"</pre>

to

<pre>Option       "ZapWarning" "off"</pre>

### tty Annoying Beep

Create an *~/.inputrc* file with the following content:

<pre>set bell-style none</pre>

 [1]: http://seife.kernalert.de/blog/2008/12/11/using-dialup-with-111-if-networkmanager-does-not-handle-your-device/