---
title: Banshee Equalizer Reworked
author: Ivan Zlatev
layout: post
permalink: /2006/07/23/banshee-equalizer-reworked/
views:
  - 145
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1323333
categories:
  - Coding
---
I&#8217;ve redesigned my previous banshee equalizer as a core banshee component and also as a separate window. Screeny:  
[[Image:software/banshee-equalizer-3.png|center]]

The initial version of the equalizer was based on the addition of the equalizer-specific virtual members to the PlayerEngine abstract class (You can refer to my [previous post][1]). Aaron had the idea to move them out of the PlayerEngine and rather define an [IEqualizer][2] interface. Also decision was taken the equalizer to be a separate window and to be a core component. Also I have made the equalizer presetable.

The equalizer implementation consists of:  
1) Equalizer.cs &#8211; the equalizer widget.  
2) EqualizerDialog.cs &#8211; the equalizer dialog.  
3) EqualizerManager.cs &#8211; the widget manager, which implements the  
presets functioanllity.  
4) Changes to banshee.glade, UIManagerLayout.xml, ActionManager.cs,  
PlayerInteface.cs, GConfKeys.cs and makefiles in order to include the  
equalizer.

The actual patch for CVS Head &#8211; [Banshee_Equalizer.patch][3]

 [1]: http://ivanz.com/2006/07/16/the-banshee-equalizer
 [2]: http://cvs.gnome.org/viewcvs/banshee/src/Banshee.Base/MediaEngine/
 [3]: http://files.ivanz.com/projects/patches/banshee/Banshee_Equalizer.patch