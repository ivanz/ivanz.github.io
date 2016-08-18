---
title: "New features and improvements coming to C# in Visual Studio Code"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

Visual Studio Code is my editor of choice (even) for C#/.NET (unless I need to do some serious refactoring or software archeology exercise in the Visual Studio IDE with ReSharper). I've noticed a couple of opportunities for imporovements in the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) ([GitHub](https://github.com/OmniSharp/omnisharp-vscode)) and a couple of merged pull requests all of the below updates are now available in the latest 1.4 beta of the C# extension and will land in the next release.


## New: Navigate and view code of symbols for which source code is unavailable

The ability to dive into the structure of any type even such without source code was a major feature I missed in Visual Studio Code when working with a lot of third party dependencies. Now it's here and supported are both "Go to definition" as well as "Peek Definition":

![](/content/2016-08-18-visual-studio-code-csharp-improvements/goto-reference.gif)

![](/content/2016-08-18-visual-studio-code-csharp-improvements/peek-reference.png)


## New: C# 6 Expression bodied members syntax highlighting


### Before

Note how the last declaration used to break the highlighting:

![](/content/2016-08-18-visual-studio-code-csharp-improvements/ex-before.png)

### After

![](/content/2016-08-18-visual-studio-code-csharp-improvements/ex-after.png)


## Improved: Namespace and usings syntax highlighting

### Before

![](/content/2016-08-18-visual-studio-code-csharp-improvements/ns-before.png)


### After

![](/content/2016-08-18-visual-studio-code-csharp-improvements/ns-after.png)


## Improved: Method parameters highlighting

### Before

![](/content/2016-08-18-visual-studio-code-csharp-improvements/arg-before.png)

### After

![](/content/2016-08-18-visual-studio-code-csharp-improvements/arg-after.png)


## Fixed: Reserved word highlighting (@class, etc)

Using `@class` used to break the highlighting for example in this case:

![](/content/2016-08-18-visual-studio-code-csharp-improvements/reserved-kw.png)




