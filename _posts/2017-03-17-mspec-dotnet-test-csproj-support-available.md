---
title: "MSpec (Machine Specifications) support for .csproj-based .NET Standard projects now available"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

I am pleased to announce that it is now possible to use and run [Mspec (Machine Specification) tests](https://github.com/machine/machine.specifications) when using .csproj based .NET Standard / .NET Core projects including  multi-targeting scenarios. 

The runner (`Machine.Specifications.Runner.VisualStudio` nuget package) supports:

* `dotnet test`
* Visual Studio's Test Explorer
* vstest.console
* Visual Studio Team Services and TFS

For information on how to migrate from `project.json` to `.csproj`-based mspec tests please refer to [this wiki page](https://github.com/machine/machine.specifications/wiki/.NET-Core-%28.NET-CLI%29). 

Here is an example .csproj targeting both .NET Core 1.1 and .NET Framework 4.6:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>    
    <TargetFrameworks>netcoreapp1.1;net46</TargetFrameworks>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Machine.Specifications" Version="0.11.0" />
    <PackageReference Include="Machine.Specifications.Runner.VisualStudio" Version="2.*" />
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.*" />
  </ItemGroup>
</Project>
```

Example output:

```
C:\code\mspec-test-sample\MSpecSampleSolution\Sample>dotnet test
Build started, please wait...
Build completed.

Test run for C:\code\mspec-test-sample\MSpecSampleSolution\Sample\bin\Debug\netcoreapp1.1\MSpecSample.dll(.NETCoreApp,Version=v1.1)
Microsoft (R) Test Execution Command Line Tool Version 15.0.0.0
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
Machine Specifications Visual Studio Test Adapter - Executing Specifications.
Machine Specifications Visual Studio Test Adapter - Executing tests in C:\code\mspec-test-sample\MSpecSampleSolution\Sample\bin\Debug\netcoreapp1.1\MSpecSample.dll
Complete on 1 assemblies

Total tests: 1. Passed: 1. Failed: 0. Skipped: 0.
Test Run Successful.
Test execution time: 0.8338 Seconds


Test run for C:\code\mspec-test-sample\MSpecSampleSolution\Sample\bin\Debug\net46\MSpecSample.dll(.NETFramework,Version=v4.6)
Microsoft (R) Test Execution Command Line Tool Version 15.0.0.0
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
Machine Specifications Visual Studio Test Adapter - Executing Specifications.
Machine Specifications Visual Studio Test Adapter - Executing tests in C:\code\mspec-test-sample\MSpecSampleSolution\Sample\bin\Debug\net46\MSpecSample.dll
Complete on 1 assemblies

Total tests: 1. Passed: 1. Failed: 0. Skipped: 0.
Test Run Successful.
Test execution time: 0.7364 Seconds
``` 



