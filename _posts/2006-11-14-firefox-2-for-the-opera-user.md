---
title: Firefox 2 for the Opera user
author: Ivan Zlatev
layout: post
permalink: /2006/11/14/firefox-2-for-the-opera-user/
views:
  - 15
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1349874
categories:
  - Coding
---
If you&#8217;ve been using Opera for a while there are a few things that you&#8217;ve probably are used to. I&#8217;ve been using Opera for so many years now and I&#8217;ve finally made my mind switching to Firefox 2. Here is how i configured my Firefox to offer me more or less the same functionality as Opera

**Session Saving**

It&#8217;s now a built-in feature in Firefox. Open the Preferences (&#8220;Main&#8221;) and set in &#8220;Startup&#8221; the &#8220;When Firefox starts:&#8221; to be &#8220;Show my windows and tabs from last time&#8221;.

**Tabs &#8211; ONLY!**

If you want to restrict no new windows to be opened, but tabs only you can set that in the Preferences &#8220;Tabs&#8221; &#8211; &#8220;New pages should be opened in:&#8221; &#8230; &#8220;new tab&#8221;.

**Pop-ups Blocking**

This is a built-in feature that is enabled by default. Can be disabled in the Preferences &#8220;Content&#8221; by unticking the &#8220;Block pop-up windows&#8221;.

**Spell Checking**

This is a built-in feature that is enabled by default.

**Mouse Gestures**

Firefox doesn&#8217;t have built-in mouse gestures support but there is an extension called [All-in-One Gestures][1]. By default it leaves a red trail when making a gesture and the click -> move down will not open a new tab. To fix those two.  
Go in &#8220;Tools&#8221; -> &#8220;Add-ons&#8221; -> Click on &#8220;All-in-One Gestures&#8221; -> &#8220;Preferences&#8221;  
-> &#8220;General Preferences&#8221; -> Untick the &#8220;Enable Gestures Trails&#8221; &#8211; for no trail  
-> &#8220;Gestures Customization&#8221; -> &#8220;Open new Tab in Foreground&#8221; -> &#8220;Edit Gesture&#8221; -> Repalce &#8220;U&#8221; with &#8220;D&#8221; (for down) -> &#8220;Ok&#8221; and if it asks to swap gestures just say yes. &#8211; for new tab opening on click -> down gesture.

**Closed tabs history**

There is an extension available for that called [Undo Closed Tabs Button][2]. After installing it you will have to right click on Firefox&#8217;s Toolbar select &#8220;Customize&#8221; and then drag and drop the button to very right (where it is in Opera). By default it will save up to 5 closed tabs and will add a &#8220;undo closed tab&#8221; right click menu entries. To fix that:  
&#8220;Tools&#8221; -> &#8220;Add-ons&#8221; -> Click on &#8220;Undo Closed Tabs Button&#8221; -> &#8220;Preferences&#8221;  
-> Tick &#8220;Remove &#8220;Undo Last Closed Tab&#8221; from the right-click context menu&#8221;  
-> Set the number of closed tabs to remember to the desired one.

**Tab Preview**

Firefox doesn&#8217;t provide a preview of the page when a tab is hovered, but there is a very nice extension called [Tab Catalog][3]. You can add it as a button to the toolbar or just hit F8 and it will show you a screen with large thumbnails of all opened tabs so you can just select the one you want.

**Content Blocking**

Install [Adblock Plus][4] and just forget about ads.

**Image Filtering (Cached/All/None)**

There is an extension available for that called [ImgLikeOpera][5]. After installing it you will have to right click on Firefox&#8217;s Toolbar select &#8220;Customize&#8221; and then drag and drop the button to the toolbar. By default it will make pages show only cached images that can be changed in &#8220;Tools&#8221; -> &#8220;Add-ons&#8221; -> Click on &#8220;ImgLikeOpera&#8221; -> &#8220;Preferences&#8221; -> Set the &#8220;Default policy for new window/tab&#8221; to &#8220;Load all images&#8221;

**Side Bar**

There is an extension available for that called [All-in-One Sidebar][6].

 [1]: https://addons.mozilla.org/firefox/12/
 [2]: https://addons.mozilla.org/firefox/3082/
 [3]: https://addons.mozilla.org/firefox/1937/
 [4]: https://addons.mozilla.org/firefox/1865/
 [5]: https://addons.mozilla.org/firefox/1672/
 [6]: https://addons.mozilla.org/firefox/1027/