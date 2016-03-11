---
title: "Machine.Specifications.Runner.VisualStudio 2.0.0-rc1 is out with many improvements"
author: Ivan Zlatev
permalink: /2016/03/11/machine-specifications-runner-visualstudio-2-0-0-rc1
layout: post
categories:
  - Coding
---

This is a significant update to the Test Adapter with substantial improvements on which I have been working over the past weeks.

**Get it here:** [https://github.com/machine-visualstudio/machine.vstestadapter/releases/tag/v2.0.0-rc1](https://github.com/machine-visualstudio/machine.vstestadapter/releases/tag/v2.0.0-rc1) 

* New test discovery engine built directly on top of MSpec replaces old Mono.Cecil based one:

 * Full support for `[Behavior]` and `Behaves_like<>` and everything MSpec
 * Full support for assembly binding (`.config`) during test discovery (and execution)

* Out of the box support for Visual Studio Online, TFS, Visual Studio Team Services through the NuGet package

 * Major performance improvements when running whole test assemblies
 * Much clearer test result output
 * Fully compatible with VS parallel assembly execution (enabled through `MaxCpuCount`  set to 0 in `*.runsettings`)


* Inner exceptions are now displayed in failed test results

* Tests in Visual Studio IDE are now displayed as English text


![image](https://cloud.githubusercontent.com/assets/79742/13714426/5b1428be-e7c6-11e5-9afd-60486c5c3482.png)

![image](https://cloud.githubusercontent.com/assets/79742/13713366/88985202-e7c0-11e5-9019-5295190ba649.png)

![image](https://cloud.githubusercontent.com/assets/79742/13713375/923e9f32-e7c0-11e5-8689-045ca9949486.png)

