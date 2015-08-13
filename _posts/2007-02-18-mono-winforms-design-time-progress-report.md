---
title: Mono WinForms Design-Time Progress Report
author: Ivan Zlatev
layout: post
permalink: /2007/02/18/mono-winforms-design-time-progress-report/
views:
  - 89
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1345555
categories:
  - Coding
---
**Introduction **

As some of you might know I&#8217;ve been working on the Windows.Forms Design-Time implementation for Mono. Basically I&#8217;ve started researching on the topic last summer and had some code done. It took me a while to get my head around the thing and to compile the documentation in my head to something that actually makes more sense. <img src="{{ site.url }}/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> For the past few months I&#8217;ve been busy with university and related. I decided to revamp my efforts a few weeks ago and started refactoring and reviewing my old code, which lead to purging my old repository and to a new code base.

The idea is not just to implement a hacked up windows forms designer, but rather to build the ground for porting or even best case scenario &#8211; for simply running an already existing one. This includes implementing stuff in the System.Design assembly in the System.ComponentModel.Design and System.Windows.Forms.Design namespaces. With the .Net 2.0 framework most of the design-time behaviour is part of the class library itself &#8211; the designers, the serializers and the host.

**Status **

Currently I have almost achieved my first milestone (called &#8220;Monkey&#8221; <img src="{{ site.url }}/wp-includes/images/smilies/simple-smile.png" alt=":-)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> ). It includes the .Net 1.1-ready core designers stack : ComponentDesigner, ControlDesigner, ParentControlDesigner, ScrollableControlDesigner, DocumentDesigner and .Net 2.0 DesignSurface (the host). I&#8217;ve had lots of &#8220;fun&#8221; working on the Drag and Drop cr*p and those annoying rectange trackers. Those althought not being perfect, work and could be refactored if there is need to. The deliverables from this milestone are as already mentioned the core designers stack and host and a small designer to allow testing of the hosting, dnd, selection, resizing, etc. A screenshot:

[[Image:software/mwf-designer-milestone-monkey.png|center]]

**The Future**

The next milestone &#8220;Monkey Returns&#8221; will target at the implementation of the serializers: DesignerLoader, BasicDesignerLoader, CodeDomDesignerLoader and introduce a bugfixing cycle for Design-Time and MWF stuff in Mono. I am expecting problems with the Design-Time behaviour in the class library. I&#8217;ve already sent a few patches related to that last year, but I am expecting more issues.

With the next milestone &#8220;Monkey&#8217;s Revenge&#8221; I am hoping to update the designers to 2.0 and possibly refactor SharpDevelop&#8217;s FormDesigner Addin as a standalone application.

I just hope I will have enough free time. I might also propose a winforms designer project for this year&#8217;s Google SoC.

I keep track of the code using Trac at <a href="http://monodt.ivanz.com" target="_blank">http://monodt.ivanz.com</a>.