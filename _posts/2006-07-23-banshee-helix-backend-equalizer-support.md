---
title: Banshee Helix backend Equalizer support
author: Ivan Zlatev
layout: post
permalink: /2006/07/23/banshee-helix-backend-equalizer-support/
views:
  - 12
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1504106
categories:
  - Coding
---
The following patches add the required in order to make the Helix backend in Banshee equalizable. It consists of 2 parts. The first one patches the [Helix DBus Server][1] and the second the [Helix-Remote Engine][2] itself. The patches:  
[Helix_IEqualizer.patch][3]  
[helix-dbus-server_Equalizer.patch][4]

 [1]: http://svn.banshee-project.org/helix-dbus-server/
 [2]: http://cvs.gnome.org/viewcvs/banshee/src/Banshee.MediaEngine/Helix/
 [3]: http://files.ivanz.com/projects/patches/banshee/Helix_IEqualizer.patch
 [4]: http://files.ivanz.com/projects/patches/banshee/helix-dbus-server_Equalizer.patch