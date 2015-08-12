---
title: Selective import of SVN branches into a git/git-svn repository
author: Ivan Zlatev
layout: post
permalink: /2009/01/15/selective-import-of-svn-branches-into-a-gitgit-svn-repository/
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 17210
dsq_thread_id:
  - 9960772
categories:
  - Coding
tags:
  - Git
  - HowTo
  - Mono
---
### Use Case

You are using Git and git-svn on a big project which has lots of branches and tags but you only need to work with a small selection of those and you don&#8217;t want to import all of the rest.

### Import an individual SVN branch

1) Define the new branch in *.git/config* :

<pre>[svn-remote "release-branch"]
        url = svn+ssh://xxxx@mono-cvs.ximian.com/source/branches/mono-2-2/mcs
        fetch = :refs/remotes/git-svn-release-branch</pre>

2) Import the SVN branch. SVN\_BRANCHED\_REVISION is the the revision when the branch happened in SVN.

<pre>[~]$ git svn fetch release-branch -r SVN_BRANCHED_REVISION</pre>

3) Hook up a local Git branch to the remote branch:

<pre>[~]$ git branch --track release git-svn-release-branch</pre>

5) Checkout and update

<pre>[~]$ git checkout release
[~]$ git svn rebase</pre>

Done!

### Delete the branch checkout

The following command sequence will delete (locally only) the SVN branch and its history:

<pre>[~]$ git branch -D release
[~]$ git branch -D -r git-svn-release-branch
[~]$ rm -rf .git/svn/git-svn-release-branch
[~]$ git gc</pre>

### Now what?

Well, for starters given that you are ready to commit something to trunk and you also want to &#8220;backport&#8221; to the release branch you just have to do:

<pre>[~]$ git svn dcommit
[~]$ git checkout release && git svn rebase
[~]$ git cherry-pick master && git svn dcommit
[~]$ git checkout master</pre>