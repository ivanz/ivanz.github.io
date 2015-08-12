---
title: 'DataGridView revamp for Mono 2.4 &#8211; Data Binding and more'
author: Ivan Zlatev
layout: post
permalink: /2009/01/15/datagridview-revamp/
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 3416
dsq_thread_id:
  - 9917123
categories:
  - Coding
tags:
  - .NET
  - Mono
  - WinForms
---
In December I developed a simple student registration system, where I utilized DataGridView, BindingSources  and the WinForms DataBinding all over the place (developed fully in Visual Studio 2008). Then I ran it only to discover that it was utterly broken on Mono, which pushed me to get down and do some work.

Ladies and Gentlemen, with ~70 commits, 20+ officially filed bugzilla bugs fixed and many dozens of such that weren&#8217;t filed and discovered by extensive interactive testing of various test samples and my own application I have revamped the DataGridView and various pieces of DataBinding in Mono&#8217;s Windows Forms implementation. Your DataGridView applications should work out of the box (mine does :-)). Those fixes will be part of the Mono 2.4 release for which we will be branching this month.

Be nice and test your applications with Subversion HEAD or Mono 2.4 branch once it is available (<http://mono-project.com/Compiling_Mono>) and in case you find any problems be nice and report them to us (<http://mono-project.com/Bugs>), so that we can fix them for Mono 2.4.

Ciao!

<div id="attachment_269" style="width: 310px" class="wp-caption aligncenter">
  <a href="http://ivanz.com/wp-content/uploads/2009/01/screenshot-student-administration-system.png"><img class="size-medium wp-image-269" title="screenshot-student-administration-system" src="http://ivanz.com/wp-content/uploads/2009/01/screenshot-student-administration-system-300x270.png" alt="DataGridView SVN HEAD caught in action" width="300" height="270" /></a>
  
  <p class="wp-caption-text">
    DataGridView SVN HEAD caught in action
  </p>
</div>