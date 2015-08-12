---
title: Git automatic smart ChangeLog merging
author: Ivan Zlatev
layout: post
permalink: /2009/03/19/git-automatic-smart-changelog-merging/
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 1310
dsq_thread_id:
  - 14016282
categories:
  - Coding
tags:
  - Git
  - HowTo
---
This might be old news for some, but there is a merge driver for Git that can automatically perform smart merges of ChangeLogs. I&#8217;ve used it for months now and it has always worked flawlessly.

1) Install *git-merge-changelog* which is part of [*gnulib*][1] . For openSUSE you can use my repository at <http://download.opensuse.org/repositories/home:/i-nZ/>

2) Make Git aware of it by adding the following to your *~/.gitconfig*

<pre>[merge "merge-changelog"]
name = GNU-style ChangeLog merge driver
driver = /usr/bin/git-merge-changelog %O %A %B</pre>

3) For each repository where you want to use this driver make Git aware of it by adding to *$GIT_REPO/.git/info/attributes* the following (create the file if it doesn&#8217;t exist):

<pre>ChangeLog    merge=merge-changelog</pre>

That&#8217;s it! Enjoy.

 [1]: http://www.gnu.org/software/gnulib/