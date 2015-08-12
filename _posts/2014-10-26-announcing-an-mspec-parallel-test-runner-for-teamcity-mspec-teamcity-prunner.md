---
title: Announcing an MSpec Parallel Test Runner for TeamCity (mspec-teamcity-prunner)
author: Ivan Zlatev
layout: post
permalink: /2014/10/26/announcing-an-mspec-parallel-test-runner-for-teamcity-mspec-teamcity-prunner/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 267
dsq_thread_id:
  - 3158090619
categories:
  - Coding
tags:
  - MSpec
  - TeamCity
  - Testing
---
<a href="https://github.com/machine/machine.specifications" target="_blank">MSpec</a> is absolutely great &#8211; no argument about it. I am using it exclusively in a current project to specify and test top to bottom. However one thing that has been bugging me for a long time is the inability to run tests in parallel on TeamCity. Most Visual Studio tools support it (CodeRush, ReSharper, etc) &#8211; why shouldn&#8217;t it be possible to do it on the build server? And as test run time kept increasing I finally got fed up and did something about it.

I am pleased to announce **<a href="https://github.com/ivanz/mspec-teamcity-prunner" target="_blank">mspec-teamcity-prunner.exe</a>**. It is a drop-in replacement for the default console runner (mspec.exe) with the major difference that through the *&#8211;threads N* parameter you can specify the number of assemblies to run in parallel.

Drop-in replacement means that all you need to do is swap the path to mspec.exe with the one to mspec-teamcity-prunner.exe in your TeamCity Build Step.

For me this has lead to ~70% reduction in the time it takes to run our MSpec test suite.

Details on the project and installation instructions/NuGet package here:

<p style="text-align: center;">
  <strong><a href="https://github.com/ivanz/mspec-teamcity-prunner" target="_blank">https://github.com/ivanz/mspec-teamcity-prunner</a></strong>
</p>

&nbsp;