---
title: Design-Time Progress Report
author: Ivan Zlatev
layout: post
permalink: /2006/07/29/design-time-progress-report/
views:
  - 23
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1343383
categories:
  - Coding
---
I have completed the code for the implementation of the [DesignSurface][1] (The DesignSurface class implements what the user perceives as a designer. DesignSurface is the user interface the user manipulates to change design-time features. DesignSurface provides a completely self-contained design surface) and [DesignSurfaceManager ][2].Net 2.0 classes for the Mono class library. Currently it consists of 33 classes. I&#8217;ve started the testing phase by hosting Micro$oft&#8217;s Windows.Forms control&#8217;s designers. The final testing/compitability testing phase I have planned is to compile and test a copy of [SharpDevelop 2][3] with my DesignSurface and related classes. I have also implemented the [ComponentDesigner][4] class and did a bit of the [ControlDesigner][5] class. I will make the source available once I have completed the testing/bugfixing/compitability improvements.

 [1]: http://msdn2.microsoft.com/en-us/library/system.componentmodel.design.designsurface.aspx
 [2]: http://msdn2.microsoft.com/en-us/library/system.componentmodel.design.designsurfacemanager.aspx
 [3]: http://www.icsharpcode.net/OpenSource/SD/Download/
 [4]: http://msdn2.microsoft.com/en-us/library/system.componentmodel.design.componentdesigner.aspx
 [5]: http://msdn2.microsoft.com/en-us/library/system.windows.forms.design.controldesigner.aspx