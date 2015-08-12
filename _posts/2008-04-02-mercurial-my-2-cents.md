---
title: 'Mercurial &#8211; my 2 cents'
author: Ivan Zlatev
layout: post
permalink: /2008/04/02/mercurial-my-2-cents/
views:
  - 344
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1353864
categories:
  - Coding
---
I was fixing a couple of Audacious bugs and adding session management support when I first had to use the [Mercurial][1] source control management system and <span style="text-decoration: underline;">initially</span> I quite liked it.

  * It&#8217;s fast, simple &#8211; comes with just enough commands
  * <span>It&#8217;s consistent &#8211; one executable to rule them all, unlike GIT and its gazillion of <span>executables</span>.</span>
  * It&#8217;s extremely easy to configure external tools for diff and merge, e.g kdiff3. For comparison in GIT merge tool is also as easy to configure, but there is no way except by an external script to set the diff tool. That&#8217;s mostly because most of the tools require 2 files to compare and not a patch.
  * <span>Has Bash completion out of the box. GIT has bash completion, but at least on <span>openSUSE</span> I had to download &#8220;it&#8221; and add it myself.</span>
  * It&#8217;s cross platform.

The initially spotted simplicity actually made me think it can help me to achieve the following work flow quite easily:

<p style="padding-left: 30px;">
  <span>Branch for each (or at least bigger) feature/bug &#8211;> Work over time on the bug/feature and once it&#8217;s ready &#8230; &#8211;> Squash merge the <span>bugfix</span>/feature branch into the main one (merge as a single commit and also drop the branch&#8217;s commit history in order to prevent pollution of the main branch with private intermediate work) -> Drop the temporary branch</span>
</p>

Now here comes my disappointment&#8230;

Even though Mercurial supports branches, the ones that can live inside the working copy (called &#8220;named&#8221; branches) cannot be dropped/deleted. Also squashing wasn&#8217;t supported, but cherry picking changes is via the shipped Transplant extension. So at the end of the day those long lived branches cannot be used for any temporary work. The short lived branches are in Subversion style &#8211; a clone in a different directory. Yuck!

Of course there are like 5-6 commands you can type in to do some magic to workaround this (delete a named branch), but I don&#8217;t like magic. Also there is an experimental 3rd party extension (not shipped with Mercurial) that provides  support for &#8220;local&#8221; branches. Experimental things to manage my code is not for me. And the last thing to mention is that Mercurial has decent built in support for patch queues, but again not much benefit work flow wise compared to Subversion. Also no good subversion &#8220;integration&#8221; is offered by Mercurial or its tools (only one-way).

<span>Looking on the good side of things reading about Mercurial wasn&#8217;t a complete waste of time and due to its simplicity it helped me to get my head around new concepts very quickly. </span>

<span>For now I am very happy of what GIT has to offer me and the two-way Subversion &#8220;integration&#8221; via git-<span>svn</span> is great.</span>

P.S: The Audacious/BMP/XMMS code is one of the worse I&#8217;ve ever seen. Looking forward to new Banshee.

 [1]: http://www.selenic.com/mercurial/wiki/