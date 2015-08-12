---
title: How to import a database schema into SQL Server Express with Visual Studio
author: Ivan Zlatev
layout: post
permalink: /2010/09/22/how-to-import-a-database-schema-into-sql-server-express-with-visual-studio/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 3330
dsq_thread_id:
  - 145339854
categories:
  - Coding
tags:
  - HowTo
---
Today I needed to import my SQL Server schema into a SQL Server Express instance on a different machine. I only have Visual Studio on this machine and no SQL Server Management Studio installed, so it took me a bit to figure out how to import the schema, so here it goes.

  1. Add the SQL Server Express instance to the Server Explorer. It&#8217;s located at &#8220;.\SQLEXPRESS&#8221;.
  2. Open the T-SQL database schema. It will say &#8220;not connected&#8221; at the end of the tab caption.
  3. In the &#8220;Data&#8221; menu you will have to &#8220;Connect&#8221; and then Validate and Execute SQL.
  4. Done.

[<img class="aligncenter size-full wp-image-724" title="connect" src="http://ivanz.com/wp-content/uploads/2010/09/connect.png" alt="" width="694" height="335" />][1]

I bet the same can be done with <a href="http://msdn.microsoft.com/en-us/library/ms365247.aspx" target="_blank">SQL Server Management Studio Express</a> but I have yet to install it.

 [1]: http://ivanz.com/wp-content/uploads/2010/09/connect.png