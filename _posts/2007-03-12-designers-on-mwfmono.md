---
title: Designers on MWF/Mono
author: Ivan Zlatev
layout: post
permalink: /2007/03/12/designers-on-mwfmono/
views:
  - 83
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1369305
categories:
  - Coding
---
Last Saturday I had planned for doing university work, but as always I found something more interesting to do. I&#8217;ve spend a few hours hacking on the WinForms designers. I managed to get them (actually referring to the designers + the DesignSurface here) running on Mono/MWF with a few small patches in MWF/Mono. And not to forget to mention how thankful I am to Rolf, who fixed form parenting for me! It works about okay. It flickers a bit when dragging controls around and also dragging controls over a container control (a control associated with a ParentControlDesigner) doesn&#8217;t reparent them. I am planning on doing more work starting from the end of next week (after handing in my Software Engineering coursework). The plan is to rewrite the IUISelectionService to use some sort of a visually and logically transparent overlay control over the designed surface and handle selection and dragging there instead of the current incredibly messy approach, which spreads through all of the designers. This will also help me to get rid of the Drag and Drop based logic and use mouse tracking instead for moving controls, also it would allow me to better control redrawing and thus I will get rid off the flickering. I am quite impatient to see if there will be a Google SoC project idea for a WinForms designer, so I can apply for it :P.

And of course a screenshot&#8230;

[[Image:software/mwf-designer-mono.png|center]]

For comparison on ms.net:

[[Image:software/mwf-designer-milestone-monkey.png|center]]