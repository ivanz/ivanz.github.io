---
title: i-Emulator
author: Ivan Zlatev
layout: post
permalink: /2005/11/13/i-emulator/
views:
  - 27
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1451348
categories:
  - Coding
---
Recentry I&#8217;ve started work on a cross platform x86 emulator in C for the open source anti-virus program ClamAV. Actually it is not a fully functional emulator, but a emulator for viruses. I am targeting the polymorphic and metamorphic viruses, as well as a generic unpacker for exe packers like UPX, FSG, MEW, AsPack, etc. I hope one day it will be able to emulate protectors too, but I might have a \*little\* problem with emulating threads and all these protectors which do self-debugging :(. Anyway I managed to finalize in some way the Intel instruction decoder ( it decodes the ModRM and SiB even 😛 ) and the memory manager. I also have a basic PE file loader, but it is really just for testing purposes. For example it is assuming the ImageBase is always 0x00400000 and some more little things must be fixed too. I call this the &#8220;kernel&#8221;, because I am trying to make the emulator layer based and each part should be separated from the others. I&#8217;ve put it on my svn &#8211; http://www.wush.net/trac/i-nZ/browser/i-Emulator/ . The problem is that if don&#8217;t have time to work on it, because I&#8217;ve got some courseworks to do :). I hope I won&#8217;t get raped for using GTK# and Mono;s C# compiler for my ACW in Programming :P. I will write a small article soon on how to write a MasterMind game using GTK#.