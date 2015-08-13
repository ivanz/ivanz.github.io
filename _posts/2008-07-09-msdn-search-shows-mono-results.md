---
title: MSDN Search shows Mono results
author: Ivan Zlatev
layout: post
permalink: /2008/07/09/msdn-search-shows-mono-results/
views:
  - 120
ratings_users:
  - 1
ratings_score:
  - 3
ratings_average:
  - 3
dsq_thread_id:
  - 1743242
categories:
  - Coding
---
  * Today I accidentaly MSDN searched for *Win32SetClipboardData* (our P/Invoke method name in Mono WinForms) instead of *SetClipboardData* and guess what&#8230; I ended up with 5 [Mono][1] results in front of me. I found it amusing considering the fact that this was MSDN Search after all. A screenshot in case they &#8220;fix&#8221; it. (Click bottom-left &#8220;*Full Size*&#8221; after the pop-up shows)

<p style="text-align: center;">
  <a href="{{ site.url }}/wp-content/uploads/2008/07/msdn-search-mono.png"><img class="size-medium wp-image-175" title="MSDN Search shows Mono results" src="{{ site.url }}/wp-content/uploads/2008/07/msdn-search-mono-300x292.png" alt="MSDN Search shows Mono results" width="300" height="292" /></a>
</p>

  * You might think I have a lot of tabs opened in Firefox, but I should mention that I closed twice more prior to taking the screenshot. BTW, **thank you**, [Tab Mix Plus][2] guys, for the multi-line tab bar.
  * Holy cow! [GNOME&#8217;s Nautilus with tabs][3] (still looks butt ugly, but oh well)&#8230;
  * Fixed my last Mono WinForms regression caused by my recent parented forms fixes.
  * Have been poking Mono WinForms Clipboard code all day trying to get my head around it and see how it would be best to implement custom formats support. On X11 I can implement that reasonably &#8220;easy&#8221; with some minor XPlatUI API change and some extra cases to binary serialize/deserialize the clipboard, which will surely involve some trickery. For Win32 the one important question, which probably Jonathan Chambers can answer best (he worked/works on our COM support), is whether we support MS COM. It seems when Win32 DnD support was written that wasn&#8217;t the case, so if you want to see some impressive code that does COM without actually doing COM (yes, you heard me) take a look at our [Win32DnD.cs][4].
  * I felt loved today. Last week I received a Mono T-Shirt and stuffed mono monkey (thanks [Jonathan][5]), an openSUSE 11 box (thanks openSUSE guys for valuing my quite small contribution of filing bugs) and today I almost got another UPS package. I am saying almost, because I missed the delivery. I am wondering what&#8217;s it going to be. I haven&#8217;t ordered anything and I have a very pure guess that it might be an openSUSE T-Shirt. Can&#8217;t wait to see what it is and then make a photo session of all my new stuff, hehe.
  * I promised my girlfriend I will be running with her tomorrow bright and early morning. Holy spaghetti monster, please help me.

 [1]: http://mono-project.com
 [2]: http://tmp.garyr.net/
 [3]: http://blogs.gnome.org/cneumair/2008/07/08/its-done/
 [4]: http://anonsvn.mono-project.com/viewcvs/trunk/mcs/class/Managed.Windows.Forms/System.Windows.Forms/Win32DnD.cs?view=markup
 [5]: http://jpobst.blogspot.com/