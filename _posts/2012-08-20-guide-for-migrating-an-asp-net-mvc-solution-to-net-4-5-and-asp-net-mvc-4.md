---
title: Guide for migrating an ASP.NET MVC Solution to .NET 4.5 and ASP.NET MVC 4
author: Ivan Zlatev
layout: post
permalink: /2012/08/20/guide-for-migrating-an-asp-net-mvc-solution-to-net-4-5-and-asp-net-mvc-4/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 8574
aktt_tweeted:
  - 1
dsq_thread_id:
  - 812437821
categories:
  - Coding
tags:
  - ASP.NET MVC
---
You can find a very handy step by step guide for migrating to ASP.NET MVC 4 [here][1] . It might seem a bit long, but actually with a few multi-file search and replace and some regex you can do it pretty quickly.

I just wanted to add a few more tips:

#### Migrating to ASP.NET MVC 4

If you are still using bin deployables (&#8220;Add Deployable Assembly&#8221;) for your current ASP.NET MVC setup &#8211; you should remove it and replace it with the *AspNetMvc* nuget package.

If you are using *MvcContrib* or *MvcFutures* &#8211; it&#8217;s probably best to update to* MvcContrib.Mvc3-ci *and *Mvc3Futures . *Either case you will have to deal with the clash of extension methods ([Html.IdFor, Html.NameFor, etc][2]) which were previously in Mvc Futures (Microsoft.Web.Mvc.dll) but now are part of the core ASP.NET MVC assembly. Until updated versions of MvcContrib and MvcFutures come out I have resolved this by removing the  using statement from my View&#8217;s Web.config for the MvcFutures extensions methods namespace. This however means that you lose tiny things like Html.SubmitButton, which you can easily replace across the whole solution with html code using a Regex.

Be careful about underwater rocks like[ this one about the new Date and DateTime editor templates in MVC 4][3]. Also there seem to be model binding changes to do with *DateTime* &#8211; so far that has been the major pain in the ass for me.

#### Re-targeting for .NET 4.5

The fastest way to convert all projects is by doing a search and replace across all *.csproj files and replace *<TargetFrameworkVersion>v4.0</TargetFrameworkVersion>  *with* <TargetFrameworkVersion>v4.5</TargetFrameworkVersion> *(or client profile, etc)

also replace in your Web.config *compilation* element *targetFramework=&#8221;4.0&#8243;* with 4.5

The real issue are the NuGet packages &#8211; some have a .NET 4.5 specific version &#8211; a good example is Entity Framework. When the project is targeting version 4.0 of the framework and you install/update EF to version 5 you actually end up with EF 4.4 (which doesn&#8217;t have support for enums and so on). The only way to solve this is to remove all packages and install them again, which is a massive pain to do manually and unfortunately the current version of NuGet is not of much help there either, but the daily build is. [More details about retargeting the NuGet packages are available in this StackOverflow question.][4]

#### A few other notes

Things that I have personally tested and work fine: MiniProfiler works fine with an runtime assembly binding redirect for Entity Framework, ELMAH and Ninject work without any changes. Noticed a small regression in unobtrusive validation and checkboxed when pulling a form with AJAX, but I that ended caused by a jQuery Validation problem.

Oh and by the way, don&#8217;t forget to update your test assemblies *App.config *with the correct assembly redirects if you are running tests against ASP.NET MVC controllers, which use MvcFutures or MvcContrib, because you will end up seeing weird &#8220;Type A doesn&#8217;t match type A&#8221; errors.

To be honest I haven&#8217;t seen noticeable performance improvements with the MVC 4 + .NET 4.5 + EF5 combo so far, but time will tell.

 [1]: http://www.asp.net/whitepapers/mvc4-release-notes#_Toc303253806
 [2]: http://msdn.microsoft.com/en-us/library/hh429726(v=vs.108)
 [3]: {{ site.url }}/2012/08/17/heads-up-on-a-hidden-breaking-change-in-asp-net-mvc-4/ "Heads up on a hidden breaking change in ASP.NET MVC 4"
 [4]: http://stackoverflow.com/questions/12006991/retargeting-solution-from-net-4-0-to-4-5-how-to-retarget-the-nuget-packages