---
title: 'Git bisect &#8211; the awesome way to find the Mr. Bug commit'
author: Ivan Zlatev
layout: post
permalink: /2009/03/27/git-bisect-the-awesome-way-to-find-the-mr-bug-commit/
aktt_notify_twitter:
  - no
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 9380
dsq_thread_id:
  - 14487428
categories:
  - Coding
tags:
  - Git
---
I had my first experience with *git bisect* the other day and it&#8217;s awesome. What happened is that I introduced a regression which was uncovered by a failing test. Yes,  yes I know I should have ran the full test suite, but I did the mistake to only run the specific set of unit test for that particular component. Now, the tricky part is that the particular test had no direct connection with the last set of commits so I had absolutely no clue which commit had caused it. Thanks to our QA team and build bot I was supplied with a range of commits which caused the regression. This is when *git bisect* came into play.

Supplied a range of commits *git bisect* automatically

1. Given a supplied range of commits starts chopping sets of them and applying in a binary search fashion until you mark the current commit as good. Say if you have 50 commits to bisect it will apply the first 25 if you mark the 25th commit as good then the regression is in those 25 commits so it will apply the first 12 of those and then if you mark them as bad it will apply the next 13 and it will then keep subsetting until you hit the bad commit. Nice and quick.

2. Create a temporary bisect branch where it operates (which when done is automatically removed as well)

The workflow is as follows:

<pre>git bisect start

# Mark current commit as bad
git bisect bad

# Set the first good commit - the start of the bisect commit set
git bisect good some-commit-here

.. compile and run tests ...

# Mark the current commit as either bad or good
# Git will automatically apply the next subset of commits if bad
git bisect good / bad

# Done bisecting - we found the bad commit
git bisect reset</pre>

There are lots of other cool things that you can do like <tt><em>visualize</em> the bisect set, <em>skip</em> range of commits in side the set or even <em>log</em> your actions to a file so that if you make a mistake somewhere you can just reply the actions from the log file. Ah and last but not least you can also use g<em>it reset</em> to manually move up the bisect commits set. For more info check out the <a href="http://www.kernel.org/pub/software/scm/git/docs/git-bisect.html">manual</a>.<br /> </tt>