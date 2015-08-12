---
title: Visual Studio 2008 Team System Database Project Templates for SQL Server 2008
author: Ivan Zlatev
layout: post
permalink: /2009/05/04/visual-studio-2008-team-system-database-project-templates-for-sql-server-2008/
ratings_users:
  - 2
ratings_score:
  - 8
ratings_average:
  - 4
views:
  - 5396
dsq_thread_id:
  - 17320785
categories:
  - Coding
tags:
  - .NET
  - SQL Server
  - Visual Studio
---
The Database templates in Visual Studio 2008 Team System are used to e.g. create automagic test data generation plans which then can be utilized in WebTests using a DataSource binding for a field value and then use the WebTests to create a LoadTest. It&#8217;s absolutely awesome and impressive how this is done in Visual Studio (only available in Team System edition).

The only little problem is that Visual Studio 2008 only ships Database templates for SQL Server 2000 and 2005. The Database templates for SQL Server 2008 are at: [Microsoft® Visual Studio Team System 2008 Database Edition GDR R2][1] and require [Visual Studio 2008 SP1][2].

A screencast on how to generate test data for a database at: [How Do I: Generate Test Data using Visual Studio Team System Database Edition?][3]

 [1]: http://www.microsoft.com/downloads/details.aspx?FamilyID=bb3ad767-5f69-4db9-b1c9-8f55759846ed&displaylang=en
 [2]: http://www.microsoft.com/downloads/details.aspx?familyid=FBEE1648-7106-44A7-9649-6D9F6D58056E&displaylang=en
 [3]: http://msdn.microsoft.com/en-us/teamsystem/cc501309.aspx