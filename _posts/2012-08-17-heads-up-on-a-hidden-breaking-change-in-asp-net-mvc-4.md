---
title: Heads up on a hidden breaking change in ASP.NET MVC 4
author: Ivan Zlatev
layout: post
permalink: /2012/08/17/heads-up-on-a-hidden-breaking-change-in-asp-net-mvc-4/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 2941
aktt_tweeted:
  - 1
dsq_thread_id:
  - 809405864
categories:
  - Coding
tags:
  - ASP.NET MVC
---
If you, like me, are using an Editor Template for DateTime and use the *DataTypeAttribute* with either or *DataType.Date* and *DataType.DateTime* in ASP.NET MVC 2 or 3 and happen to upgrade to ASP.NET MVC 4 **be aware!**

ASP.NET MVC 4 introduces two new internal editor templates for properties marked as *DataType.Date* and *DataType.DateTime *to render HTML5 input types (date and datetime accordingly).

The unexpected side-effect of this is that if you were previously capturing all *DateTime *type editing through a *DateTime* editor template now if your property is marked as *DataType.Date* ASP.NET MVC 4 will use the new internal *Date* editor template instead of your *DateTime *editor template.

The solution is to provide a *Date.cshtml *editor template along your* DateTime.cshtml* editor template.