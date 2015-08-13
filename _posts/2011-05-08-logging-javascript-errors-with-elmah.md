---
title: Logging JavaScript Errors with ELMAH in ASP.NET MVC
author: Ivan Zlatev
layout: post
permalink: /2011/05/08/logging-javascript-errors-with-elmah/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 5085
dsq_thread_id:
  - 298649946
categories:
  - Coding
tags:
  - .NET
  - ASP.NET MVC
  - HowTo
---
One final piece of information I wanted to add to [my last blog post about setting up ELMAH for ASP.NET MVC][1] is how to log Client Side/JavaScript errors with [ELMAH][2].

To do that we need to:

1. Because ELMAH works with exceptions we will need to define a *JavaScriptErrorException *to represent our client side errors in the logs:

<pre class="brush: csharp; title: ; notranslate" title="">using System;

namespace Triply.Extensions
{
	[Serializable]
	public class JavaScriptErrorException : Exception
	{
		public JavaScriptErrorException (string message) : base(message)
		{
		}
	}
}</pre>

2. Define a Controller Action to be invoked by the client side that takes in an error message:

<pre class="brush: csharp; title: ; notranslate" title="">using System;
using System.Web.Mvc;
using Elmah;
using Triply.Extensions;

namespace Triply.Controllers
{
	public class HomeController : Controller
	{
		[HttpPost]
		public void LogJavaScriptError(string message)
		{
			ErrorSignal.FromCurrentContext().Raise(new JavaScriptErrorException(message));
		}
	}
}</pre>

3. Use an AJAX post with a message to log the error

<pre class="brush: jscript; title: ; notranslate" title="">function logError(details) {
    $.post("/Home/LogJavaScriptError", { message: details });
}
</pre>

Done!

 [1]: {{ site.url }}/2011/05/08/asp-net-mvc-magical-error-logging-with-elmah/ "ASP.NET MVC Magical Error Logging with ELMAH"
 [2]: http://code.google.com/p/elmah/