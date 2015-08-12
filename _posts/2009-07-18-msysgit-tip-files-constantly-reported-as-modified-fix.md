---
title: 'msysGit Tip: Files Constantly Reported as Modified Fix'
author: Ivan Zlatev
layout: post
permalink: /2009/07/18/msysgit-tip-files-constantly-reported-as-modified-fix/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 842
dsq_thread_id:
  - 
categories:
  - Coding
tags:
  - Git
---
I copied my Mono mcs Git working copy from Linux to Windows for use with MSysGit only to find out that even after *git reset &#8211;hard* almost all files were being reported as modified by *git status*. It turns out it has to do with the file permissions (executable bit I think) and the inability of msysgit to apply them on Windows.

<pre>$ git diff class/Accessibility/makefile.build
diff --git a/class/Accessibility/makefile.build b/class/Accessibility/makefile
old mode 100755
new mode 100644</pre>

The fix is to make git ignore some file mode stuff via:

<pre>git config core.fileMode false</pre>

Another useful setting on Windows is:

<pre>git config core.autocrlf true</pre>