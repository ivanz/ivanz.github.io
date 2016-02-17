---
title: "CorrelatorSharp: Your one stop shop for context-aware logging and diagnostics"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

In the world of microservices and distributed systems it is very important to relate logging and diagnostic information of multiple services in the context of an operation to quickly diagnose issues.

## What is CorrelatorSharp?

[CorrelatorSharp](https://github.com/ivanz/CorrelatorSharp) is a .NET library that enables context-aware logging and correlation with no code changes or minimal such.

[CorrelatorSharp](https://github.com/ivanz/CorrelatorSharp) is **async/await aware** and the current activity **flows across tasks and threads automatically**.

[CorrelatorSharp](https://github.com/ivanz/CorrelatorSharp) supports out of the box (at the time of writing this post):

* ASP.NET MVC 5
* ASP.NET Web API
* RestSharp
* NLog
* Application Insights

Support for the following is also planned: WCF server/client, Azure Service Bus, Azure Storage Queue, CoreCLR.

## Using CorrelatorSharp in 5 minutes

### 1. Enable in Web API in Service "B"

Use the CorrelatorSharp.WebApi package:

```csharp
config.Filters.Add(new CorrelationIdActionFilter());
``` 

### 2. Log the current correlation id

You can do it manually (or through one of the CorrelatorSharp packages such as *CorrelatorSharp.NLog* or *CorrelatorSharp.ApplicationInsights*):

```csharp
Console.WriteLine("Current Activity Id: " + ActivityScope.Current.Id);
Console.WriteLine("Current Activity Name: " + ActivityScope.Current.Name);
Console.WriteLine("Current Activity ParentId: " + ActivityScope.Current.ParentId);
```

### 3. Pass the correlation id from Service "A" to Service "B"

If you are using RestSharp you can use the *CorrelatorSharp.RestSharp*  or just add a `X-Correlation-Id` header to your requests:

```csharp
RestRequest request;

request.AddCorrelationHeader();
```

### 4. Manage activity scopes

```csharp
using (ActivityScope scope = new ActivityScope("Operation")) {
      Console.WriteLine("Current Activity Id: " + ActivityScope.Current.Id);
      Console.WriteLine("Current Activity Name: " + ActivityScope.Current.Name);
      Console.WriteLine("Current Activity ParentId: " + ActivityScope.Current.ParentId);

  using (ActivityScope nestedScope = new ActivityScope("Nested Operation")) {
      Console.WriteLine("Current Activity Id: " + ActivityScope.Current.Id);
      Console.WriteLine("Current Activity Name: " + ActivityScope.Current.Name);
      Console.WriteLine("Current Activity ParentId: " + ActivityScope.Current.ParentId);
  }
}
```

Example output:

```csharp
Current Activity Id: 4050a075-51db-4e62-8a92-17720a73045e
Current Activity Name: Operation
Current Activity ParentId:

Current Activity Id: 2d7aa66d-62b4-406d-91e4-ea3305690675
Current Activity Name: Nested Operation
Current Activity ParentId: 4050a075-51db-4e62-8a92-17720a73045e
```

# Get it now

* [NuGet](https://www.nuget.org/packages?q=correlatorsharp)
* [GitHub](https://github.com/ivanz/CorrelatorSharp)