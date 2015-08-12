---
title: ASP.NET MVC Magical Error Logging with ELMAH
author: Ivan Zlatev
layout: post
permalink: /2011/05/08/asp-net-mvc-magical-error-logging-with-elmah/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 15464
dsq_thread_id:
  - 298640939
categories:
  - Coding
tags:
  - .NET
  - ASP.NET
  - ASP.NET MVC
  - HowTo
---
If you aren&#8217;t using ELMAH (Error Logging Modules and Handlers) you should be &#8211; <http://code.google.com/p/elmah/> It&#8217;s the best thing since sliced bread for logging ASP.NET errors with practically zero effort. It basically plugs into the ASP.NET pipeline and logs all unhandled exceptions thrown and HTTP error codes together with all sorts of other useful information such as request url, stacktrace, current user username, cookies and more. You can say it makes a snapshot of the Yellow Screen of Death with extra information. It then gives you a simple web access to the recent errors logged (by default at http//mysite.com/elmah.axd), exposing a RSS feed as well. It&#8217;s fully configurable (error filters, multiple backend log storage systems support, etc) and also can be set up to send emails on error.

Some screenshots:

<div style="width: 310px" class="wp-caption aligncenter">
  <a href="http://ivanz.com/wp-content/uploads/2011/05/elmah-log-1.png"><img title="elmah-log-1" src="http://ivanz.com/wp-content/uploads/2011/05/elmah-log-1-300x43.png" alt="" width="300" height="43" /></a>
  
  <p class="wp-caption-text">
    List of Errors using the Web Interface
  </p>
</div>

<div style="width: 310px" class="wp-caption aligncenter">
  <a href="http://ivanz.com/wp-content/uploads/2011/05/elmah-log-2.png"><img title="elmah-log-2" src="http://ivanz.com/wp-content/uploads/2011/05/elmah-log-2-300x186.png" alt="" width="300" height="186" /></a>
  
  <p class="wp-caption-text">
    Error Details in the web interface
  </p>
</div>

[][1]

You can pull ELMAH through nuget via the &#8220;Elmah&#8221; package., which will download it and add a reference. The steps to get it all set up for ASP.NET MVC are as follows.

1. Register Elmah configuration sections in the *Web.config*

<pre class="brush: xml; title: ; notranslate" title="">&lt;sectionGroup name="elmah"&gt;
      &lt;section name="security" requirePermission="false" type="Elmah.SecuritySectionHandler, Elmah" /&gt;
      &lt;section name="errorLog" requirePermission="false" type="Elmah.ErrorLogSectionHandler, Elmah" /&gt;
      &lt;section name="errorMail" requirePermission="false" type="Elmah.ErrorMailSectionHandler, Elmah" /&gt;
      &lt;section name="errorFilter" requirePermission="false" type="Elmah.ErrorFilterSectionHandler, Elmah" /&gt;
    &lt;/sectionGroup&gt;</pre>

2. Add the root level ELMAH section in *Web.config* to tell it where to store the logs and how. Easiest to set up is to have a directory where it stores xml file for each error, **but make sure the IIS process/AppPool has write access to that directory!** Also I have added a filter to ignore all HTTP 404 errors. Note that this filter won&#8217;t work for ASP.NET MVC and I will come back to that later.

<pre class="brush: xml; title: ; notranslate" title="">&lt;elmah&gt;
    &lt;security allowRemoteAccess="1" /&gt;
    &lt;errorLog type="Elmah.XmlFileErrorLog, Elmah" logPath="~/App_Data/ElmahLogs" /&gt;
    &lt;errorFilter&gt;
      &lt;test&gt;
        &lt;equal binding="HttpStatusCode" value="404" type="Int32" /&gt;
      &lt;/test&gt;
    &lt;/errorFilter&gt;
  &lt;/elmah&gt;</pre>

3. Register ELMAH with the ASP.NET pipeline **both** in the **system.web **section (when running locally) and the  **system.webServer** section when deployed to IIS. Just copy paste the below snippet in both sections. Notice how the default configuration says that ELMAH will be available at mysite.com/elmah.axd . You can change that if you want (in both places!)

<pre class="brush: xml; title: ; notranslate" title="">&lt;httpModules&gt;
      &lt;add name="ErrorLog" type="Elmah.ErrorLogModule, Elmah" /&gt;
    &lt;/httpModules&gt;
    &lt;httpHandlers&gt;
      &lt;add verb="POST,GET,HEAD" path="elmah.axd" type="Elmah.ErrorLogPageFactory, Elmah" /&gt;
    &lt;/httpHandlers&gt;</pre>

You are done and elmah will be available at http://mysite.com/elmah.axd , but it won&#8217;t work quite right with ASP.NET MVC, because of the way errors are handled there, so there are those additional things that need to be set up:

4. Log exceptions before ASP.NET MVC swallows them if using customErrors or some other code handles and swallows the error. To do this I&#8217;ve implemented the below exception filter that logs all handled exceptions. It&#8217;s a good example also of how to programatically log errors with ELMAH:

<pre class="brush: csharp; title: ; notranslate" title="">using System;
using System.Web.Mvc;
using Elmah;

namespace Triply.Extensions
{
	public class ElmahHandledErrorLoggerFilter : IExceptionFilter
	{
		public void OnException (ExceptionContext context)
		{
			// Long only handled exceptions, because all other will be caught by ELMAH anyway.
			if (context.ExceptionHandled)
				ErrorSignal.FromCurrentContext().Raise(context.Exception);
		}
	}
}</pre>

To register it add it in *Global.asax.cs*. The ordering is important &#8211; **add it before the HandleErrorAttribute is registered**.

<pre class="brush: csharp; title: ; notranslate" title="">public static void RegisterGlobalFilters (GlobalFilterCollection filters)
{
	filters.Add(new ElmahHandledErrorLoggerFilter());
	filters.Add(new HandleErrorAttribute());
}</pre>

5. One final note regarding filtering out HTTP 404 errors. In ASP.NET MVC they are raised as exceptions so if you want to ignore them unfortunately you can&#8217;t do that declaratively in the Web.config. Instead you have to add the following elmah hooks to your *Global.asax.cs* to do it programatically:

<pre class="brush: csharp; title: ; notranslate" title="">// ELMAH Filtering
		protected void ErrorLog_Filtering (object sender, ExceptionFilterEventArgs e)
		{
			FilterError404(e);
		}

		protected void ErrorMail_Filtering (object sender, ExceptionFilterEventArgs e)
		{
			FilterError404(e);
		}

		// Dismiss 404 errors for ELMAH
		private void FilterError404 (ExceptionFilterEventArgs e)
		{
			if (e.Exception.GetBaseException() is HttpException) {
				HttpException ex = (HttpException)e.Exception.GetBaseException();
				if (ex.GetHttpCode() == 404)
					e.Dismiss();
			}
		}</pre>

That&#8217;s it. Now you get super cool logging (and remember &#8211; RSS feed!)

[  
][1]

 [1]: http://ivanz.com/wp-content/uploads/2011/05/elmah-log-1.png