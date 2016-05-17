---
title: "Farewell project.json - hello MSBuild and .csproj (.NET Core)"
author: Ivan Zlatev
permalink: /2016/05/17/farewell-project-json-hello-msbuild-and-csproj
layout: post
categories:
  - Coding
---

## Farewell project.json

There was a fairly important update from Microsoft last week, which I personally felt went a bit under the cover.

The .NET Core (and related) teamshave decided to  **drop `project.json` and go back to MSBuild and `*.csproj`**. This is not something that's already happened in the just released .Net Core RC2 and ASP.NET Core RC2, but it's happening.

Wondering where that announcement was made? As far as I know - there wasn't one. It was mentioned in the stand-up (see the video at the end of this post).

This is fairly disappointing, because the `project.json` was a breath of fresh air. However it's understandable (to an extend):

* It would make the transition of existing Visual Studio solutions to .NET Core straighforward
* It's a huge change. It will enable leveraging existing investment in CI/RM based around MSBuild.
* There is *a lot* going on under the hood during build in MSBuild. Think incremental compilation, resolving build-time dependencies, configuration management and tons more (hidden behind the e.g. `Microsoft.CSharp.targets` imported by every project)
* Too much work to ship the whole thing on top of `dotnet cli` on time. Remember that it's no longer just about ASP.NET Core, but also build and release of console apps, mobile apps, UWP apps, Xamarin.
* The .NET core team would have eventually ended up probably implementing/re-implementing a project model system and parts of msbuild.

Still that doesn't make me happier right now.

## Hello MSbuild and .csproj

Appears this is what will happen in future releases:

* `*.xproj` will be replaced by `*.csproj`
* Features in `project.json` will start getting merged back into the the `*.csproj`
* It is not yet clear what they are going to do about the packages list, but it was mentioned they might keep it as json under `nuget.json` or merge it into the `.csproj`. GitHub issue tracking this here: [https://github.com/aspnet/Home/issues/1433](https://github.com/aspnet/Home/issues/1433)
* Supposedly that transition should be smooth and potentially automatic if using Visual Studio

The good news is that:

* MSBuild is open source and [available on GitHub](https://github.com/Microsoft/msbuild) and is bound to become fully cross-platform
* They is an effort to dramatically simplify and trim the structure of the `.csproj` - e.g. remove the requirement to have a list of files. GitHub issue here: [https://github.com/dotnet/roslyn-project-system/issues/40](https://github.com/dotnet/roslyn-project-system/issues/40)
* There is a new revamped project system coming which will enable a lot of scenarios without the need for Visual Studio: [https://github.com/dotnet/roslyn-project-system/]
(https://github.com/dotnet/roslyn-project-system/)
* The goal is that even with the MSBuild set-up working with builds and project should be as seamless in Visual Studio IDE as well as outside of it.


## Announcement video

<iframe width="560" height="315" src="https://www.youtube.com/embed/P9HqMZviaMg?start=1713" frameborder="0" allowfullscreen></iframe>
