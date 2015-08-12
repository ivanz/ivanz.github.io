---
title: MS Office 2007 SP2 OpenDocument Support
author: Ivan Zlatev
layout: post
permalink: /2009/05/01/ms-office-2007-sp2-opendocument-support/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 349
dsq_thread_id:
  - 17127042
categories:
  - Coding
tags:
  - Bugs
  - Linux
---
This seems to have gone unnoticed but Service Pack 2 for Microsoft Office 2007 introduced native support for loading from and saving to the OpenDocument file format. So I went ahead and opened my OpenDocument final year project which is a 57 pages document with custom styles, formatting, frames, images, embedded diagrams and spreadsheets and more. The results was pretty bad and the resulting document was unusable for me. Basically only the text and bullet points were displayed properly and at least the following were broken:

  * No page breaks.
  * Wrong Frame size and placement, including caption
  * Wrong image size and placement, including caption
  * Extra spacing for Headings surrounded by borders
  * Merge with next paragraphs ignored
  * No embedded drawings/speardsheets.
  * Table content placement not preserved.
  * More quirks

I suppose for people who work with simple text documents the support for OpenDocument might be sufficient though. I for one am not going to rely on the ability to safely open my work documents with Word 2007 or such produced by Word 2007 in OpenOffice.

Who is to blame? Is OpenOffice not following the OpenDocument standard well or is it Word 2007 interpreting it badly? Gotta love standards! I am looking forward to see both office suits interoperating nicely together some day.

The test documents:

  * [Original OpenDocument Report ODT][1]
  * [Original OpenDocument Report PDF  
    ][2]
  * [Report Opened in Word 2007][3]
  * [Report Re-Saved in Word 2007 and opened in OpenOffice][4]

Some screenshots:<a rel="attachment wp-att-535" href="http://ivanz.com/wp-content/uploads/2009/05/oo-word-3.png"><img class="aligncenter size-full wp-image-535" title="oo-word-3" src="http://ivanz.com/wp-content/uploads/2009/05/oo-word-3.png" alt="oo-word-3" width="583" height="375" /></a>

<a rel="attachment wp-att-534" href="http://ivanz.com/wp-content/uploads/2009/05/oo-word-2.png"><img class="aligncenter size-medium wp-image-534" title="oo-word-2" src="http://ivanz.com/wp-content/uploads/2009/05/oo-word-2-300x76.png" alt="oo-word-2" width="300" height="76" /></a>

<p style="text-align: center;">
  <a rel="attachment wp-att-533" href="http://ivanz.com/wp-content/uploads/2009/05/oo-word.png"><img class="size-medium wp-image-533 aligncenter" title="oo-word" src="http://ivanz.com/wp-content/uploads/2009/05/oo-word-300x139.png" alt="oo-word" width="300" height="139" /></a>
</p>

 [1]: http://ivanz.com/wp-content/uploads/2009/05/final-report.odt
 [2]: http://ivanz.com/wp-content/uploads/2009/05/report.pdf
 [3]: http://ivanz.com/wp-content/uploads/2009/05/final-report-word.pdf
 [4]: http://ivanz.com/wp-content/uploads/2009/05/final-report-word-ooo.pdf