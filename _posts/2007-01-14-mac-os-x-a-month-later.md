---
title: 'Mac OS X: A Month Later'
author: Ivan Zlatev
layout: post
permalink: /2007/01/14/mac-os-x-a-month-later/
views:
  - 44
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 3166129
categories:
  - Coding
---
My main OS on the iMac is openSUSE Linux. I&#8217;ve decided to keep the Mac OS X to play with it when I am bored.

I have found workarounds for 2 [of the issues I&#8217;ve had][1]:

**Mouse Acceleration**

I found this &#8220;trick&#8221; while I was talking on the phone and clicking absolutly randomly around. :-). While it doesn&#8217;t fix the issue 100% it does make stuff more usable for me. Open *System Preferences -> Universal Access* and set the *Initial Delay* to *Short*. Screenshot:

[[Image:software/osx-accel-fix.png|center]]

**Keybindings in GUI**

To start using Ctrl instead of the Apple key go to *System Preferences -> Keyboard and Mouse -> Modifier Keys* swap *Control* and *Command*.

To fix page up/down/home/end create a file (and directory path if not exisiting yet) ~/Library/Keybindings/DefaultKeyBinding.dict with the following contents:

> `/* ~/Library/KeyBindings/DefaultKeyBinding.dict */<br />
{<br />
"^\010"    = "deleteWordBackward:";<br />
"\UF729"   = "moveToBeginningOfLine:";<br />
"^\UF729"  = "moveToBeginningOfDocument:";<br />
"$\UF729"  = "moveToBeginningOfLineAndModifySelection:";<br />
"$^\UF729" = "moveToBeginningOfDocumentAndModifySelection:";<br />
"\UF72B"   = "moveToEndOfLine:";<br />
"^\UF72B"  = "moveToEndOfDocument:";<br />
"$\UF72B"  = "moveToEndOfLineAndModifySelection:";<br />
"$^\UF72B" = "moveToEndOfDocumentAndModifySelection:";<br />
"^\UF702"  = "moveWordBackward:";<br />
"^\UF703"  = "moveWordForward:";<br />
"$^\UF702" = "moveWordBackwardAndModifySelection:";<br />
"$^\UF703" = "moveWordForwardAndModifySelection:";<br />
"\UF72C"   = "pageUp:";<br />
"\UF72D"   = "pageDown:";<br />
}<br />
`

**Keybindings in Terminal**

In the *Window Settings -> Keyboard* swap *shift xxx*keys with the equivalen without shift &#8211; e.g *shift page up* with *page up*. To get the backspace key working in the Terminal app &#8220;Delete key sends backspace&#8221; should be ticked. Also do not forget to click on *&#8220;Use Settings as Defaults*.

Create or append to *~/.inputrc* the following content:

> `# Be 8 bit clean.<br />
set input-meta on<br />
set output-meta on<br />
set convert-meta off`
> 
> \# allow the use of the Home/End keys  
> &#8220;\e[1~&#8221;: beginning-of-line  
> &#8220;\e[4~&#8221;: end-of-line
> 
> \# allow the use of the Delete/Insert keys  
> &#8220;\e[3~&#8221;: delete-char  
> &#8220;\e[2~&#8221;: quoted-insert
> 
> \# mappings for &#8220;page up&#8221; and &#8220;page down&#8221; to step to the beginning/end  
> \# of the history  
> &#8220;\e[5~&#8221;: beginning-of-history  
> &#8220;\e[6~&#8221;: end-of-history
> 
> \# alternate mappings for &#8220;page up&#8221; and &#8220;page down&#8221; to search the history  
> \# &#8220;\e[5~&#8221;: history-search-backward  
> \# &#8220;\e[6~&#8221;: history-search-forward

Source &#8211; [http://tech.inhelsinki.nl/gnu\_developement\_under\_mac\_os_x/][2]

 [1]: http://ivanz.com/2006/10/29/mac-os-x-not-for-me/
 [2]: http://tech.inhelsinki.nl/gnu_developement_under_mac_os_x/