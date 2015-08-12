---
title: 'Traktor Pro .tsi file format reverse-engineered  &#8211; docs on GitHub'
author: Ivan Zlatev
layout: post
permalink: /2014/10/20/traktor-pro-tsi-file-format-reverse-engineered-docs-on-github/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 473
dsq_thread_id:
  - 3134946091
categories:
  - Coding
  - Technology
---
I&#8217;ve reverse-engineered the .tsi binary file format specification, which Traktor Pro by Native Instruments uses to store its Controller Mappings. I am hoping this should open the gates to new power tooling for MIDI controller mapping power tools (Traktor&#8217;s own being rather sub-optimal).

The project, documentation and instructions are here: **<https://github.com/ivanz/TraktorMappingFileFormat/wiki>**

Can&#8217;t sing enough praises of <a href="http://www.sweetscape.com/010editor/" target="_blank">010 Editor by SweetScape</a> which enabled me to write scripts and define data structures incrementally on top of a binary blob.

[<img class="aligncenter size-medium wp-image-1013" src="http://ivanz.com/wp-content/uploads/2014/10/2014-10-20_01-53-45-300x212.png" alt="2014-10-20_01-53-45" width="300" height="212" />][1]

 [1]: http://ivanz.com/wp-content/uploads/2014/10/2014-10-20_01-53-45.png