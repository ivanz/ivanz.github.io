---
title: "MSpec (Machine.Specifications) Visual Studio Adapter 1.7.0 released"
author: Ivan Zlatev
permalink: /2016/01/28/mspec-visualstudio-adapter-1-7-0-released
layout: post
categories:
  - Coding
---

In the last 2 weeks or so, I have given a helping hand to Jonathan Wilkins with the MSpec Visual Studio adapter project. My initial main focus was to get the adapter nuget ready, so that it can be used in Visual Studio Online (now Team Services) seamlessly. I have also set-up a CI environment in AppVeyor: https://ci.appveyor.com/project/machine-visualstudio/machine-vstestadapter

If the udpated Visual Studio extensions is not live yet at the time you read this blog post - you can grab it from the **GitHub release: https://github.com/machine-visualstudio/machine.vstestadapter/releases/tag/v1.7.0 **

Release notes below. If you are curious what is potentially cooking in the next 1.8.0 relase - you can look here: https://github.com/machine-visualstudio/machine.vstestadapter/issues?q=is%3Aopen+is%3Aissue+milestone%3A1.8.0

## What's new

* Upgraded to use Machine.Specifications 0.9.3

* The adapter now is distributed through a NuGet package in addition to the Visual Studio extension to enable Continuous Integration/Delivery scenario
   * https://www.nuget.org/packages/Machine.Specifications.Runner.VisualStudio/

* The adaptor is now set-up with Continuous Integration and builds of the NuGet package and VSIX extension can be downloaded from AppVeyor
    * https://ci.appveyor.com/project/machine-visualstudio/machine-vstestadapter

## Related Documentation

* [Using the adaptor in Visual Studio Team Services (Visual Studio Online) build and release](https://github.com/machine-visualstudio/machine.vstestadapter/wiki/Visual-Studio-Team-Services-(Visual-Studio-Online-VSO)-Support)

![image](https://cloud.githubusercontent.com/assets/79742/12643294/311c1d94-c5b3-11e5-90a9-741ca940ca36.png)
