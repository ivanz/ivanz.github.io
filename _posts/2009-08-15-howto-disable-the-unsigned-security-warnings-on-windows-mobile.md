---
title: HowTo Disable the Unsigned Security Warnings on Windows Mobile
author: Ivan Zlatev
layout: post
permalink: /2009/08/15/howto-disable-the-unsigned-security-warnings-on-windows-mobile/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 1806
dsq_thread_id:
  - 
categories:
  - Coding
  - Technology
tags:
  - .NET
  - HowTo
  - Visual Studio
---
The security warnings when deploying unsigned applications from Visual Studio to a real Windows Mobile device can get very annoying. Thankfully they can be disabled with a hack and here is how.

Using the Remote Registry Editor supplied with Visual Studio and with the Windows Mobile SDK installed change the following key value from 0 to 1 (and vise-versa to reset the change)

    HKEY_LOCAL_MACHINE\Security\Policies\Policies\0000101a = 1

That&#8217;s it.