---
title: Intel Mac ALSA Goodies
author: Ivan Zlatev
layout: post
permalink: /2007/05/29/intel-mac-alsa-goodies/
views:
  - 563
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1318500
categories:
  - Coding
---
I am revising and not avoiding work again, I swear! Actually I \*am\* revising for my last exam on Friday namely &#8220;Networking Systems and Application&#8221; boo.

In the meanwhile I have some ALSA goodies for us the ones that run Linux on their Intel Macs. Your sound doesn&#8217;t work? Your built-in microphone doesn&#8217;t work? No more!

I&#8217;ve got my hands on the latest pinconfigs and cooked up a patch for ALSA, which adds support for all Intel Macs (macbooks, imacs, macminis etc, except for the 24&#8221; iMac and the Mac Pro) and also fixes the current configs. The changeset in HG is <a href="http://hg.alsa-project.org/alsa-kernel/rev/ff3ed7049f84" title="ff3ed7049f84" target="_blank">ff3ed7049f84</a>. This patch enables the built-in microphone out of the box.

It took me a while to figure out how to get the microphone working, because, as I later found out, the Mux Capture control when maxed out disabled it. As I later found out after an investigation the Mux control is supposed to be limited to 2 step increase (+20db microphone boost), but was limited to 4 in ALSA. Without the boost the mic is pretty useless because the sound recorded has really low volume. The changeset to fix that in HG is <a href="http://hg.alsa-project.org/alsa-kernel/rev/6ee58da0b892" title="6ee58da0b892" target="_blank">6ee58da0b892</a>.

The last issue I stumbled upon is that when I had to unplug my speakers to plug in my headphones the microphone got disabled and I had to reload ALSA to fix it. The changeset to fix that in HG is <a href="http://hg.alsa-project.org/alsa-kernel/rev/200fc3a7ef62" title="200fc3a7ef62" target="_blank">200fc3a7ef62</a>.

At the end of the day thanks to Takashi Iwai and my pinconfigs patch **everything works and behaves perfectly out of the box**! Finally!