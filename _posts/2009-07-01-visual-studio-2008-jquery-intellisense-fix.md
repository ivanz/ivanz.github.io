---
title: Visual Studio 2008 jQuery IntelliSense Fix
author: Ivan Zlatev
layout: post
permalink: /2009/07/01/visual-studio-2008-jquery-intellisense-fix/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 1826
dsq_thread_id:
  - 
categories:
  - Coding
tags:
  - .NET
  - ASP.NET
  - Bugs
  - jQuery
  - Visual Studio
---
I am tinkering with ASP.NET MVC and jQuery and making my first baby steps in a whole new horrible world of web development. I found out that the JavaScript IntelliSense in Visual Studio 2008 is broken out of the box. The error is:

<pre>Warning    2    Error updating JScript IntelliSense: jquery-1.3.2.js:
   Object doesn't support this property or method @ 2173:1</pre>

The fix for Visual Studio 2008 SP1 by Microsoft can be found here:

[KB958502 &#8211; JScript Editor support for “-vsdoc.js” IntelliSense doc. files][1]

 [1]: http://code.msdn.microsoft.com/KB958502