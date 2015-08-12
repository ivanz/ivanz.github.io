---
title: F-spot bug fixing day
author: Ivan Zlatev
layout: post
permalink: /2006/03/17/f-spot-bug-fixing-day/
views:
  - 10
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1333386
categories:
  - Coding
---
I am feeling so annoyed at the moment. Today without a reason I played with [f-spot&#8217;s][1] source and fixed bugs [334805][2], [334706][3] and [316752][4]

With those patches:  
[FormClient-WebProxy.patch][5]  
[GalleryExport-AccountDialog-URL-Validation-fix.patch][6]  
[GalleryExport-AddAlbum-Crash.patch][7]

And I played a few hours (including a wine drinking break) to implement proxy support. Added gui and code in preferences, saving in gconf, validation and everything and than I realized that there is WebProxy.GetDefaultProxy() !!!! Such an idiot am I! Well the good thing is that I am much more familiar with the f-spot sources than before&#8230;

 [1]: http://f-spot.org
 [2]: http://bugzilla.gnome.org/show_bug.cgi?id=334805
 [3]: http://bugzilla.gnome.org/show_bug.cgi?id=334706
 [4]: http://bugzilla.gnome.org/show_bug.cgi?id=316752
 [5]: http://ivanz.com/files/projects/patches/f-spot/FormClient-WebProxy.diff
 [6]: http://ivanz.com/files/projects/patches/f-spot/GalleryExport-AccountDialog-URL-Validation-fix.diff
 [7]: http://ivanz.com/files/projects/patches/f-spot/GalleryExport-AddAlbum-Crash.diff