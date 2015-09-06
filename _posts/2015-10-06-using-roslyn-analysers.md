---
title: Using, configuring and distributing Roslyn analysers in teams
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

# What are the Roslyn analysers?

As you should already know Roslyn is the name of new C# 6.0 (and Visual Basic) compiler, written entirely in C# (and VB for the VB one). The old compiler was written in C++ and this is a complete re-implementation. You can find Roslyn's source code [on GitHub](https://github.com/dotnet/roslyn).
 
However Roslyn is not just a compiler. It's also and SDK (".Net Compiler Platform") which exposes the compiler as a service. Tools (such as Visual Studio, ReSharper, CodeRush, LinqPad) can directly hook into the syntax tree information exposed by the compiler to achieve various jobs such as code auto-completion, refactoring and code analysis. Previously those tools had to implement their own C# code parsing, effectively duplicating the compiler's job. It's a non-trivial task, so that's why libraries such as [NRefactory](https://github.com/icsharpcode/SharpDevelop/wiki/NRefactory) became popular. You will find it used across various tools - e.g. LinqPad, Xamarin Studio (I think).

In simplified terms Roslyn analysers are plug-ins that plug into the Roslyn compilation (parsing) pipeline and are able to spit out warnings and errors whilst editing code in Visual Studio and during compilation. They can also provide "code fixes"/refactorings.

I am not going to drill into the details of how the analysers are implemented or how to implemented one in this post, but focus more on how to use them across a team and continuous integration.

You can get all source code from this post at: [https://github.com/ivanz/RoslynAnalysersUsageSample](https://github.com/ivanz/RoslynAnalysersUsageSample)

# Using Roslyn analysers as Visual Studio Extensions

The easiest way to get some extra analysers is to install the really good [VS Refactoring Essentials](http://vsrefactoringessentials.com/) analysers pack, which contains around [100 of them](http://vsrefactoringessentials.com/Features/All#collapseCSharpAnalyzers) (and lots of refactoring tools).

![](/content/2015-10-06-using-roslyn-analysers/extensions.png)

Once you have them installed you will start seeing some warnings, such as this one:

![](/content/2015-10-06-using-roslyn-analysers/what-it-looks-like.png)

*Update:* You may see sporadic `MissingAnalyzerReference` warnings with Refactoring Essentials and StyleCop to do with Visual Basic, which you can safely ignore. This bug is tracked [in this GitHub issue](https://github.com/icsharpcode/RefactoringEssentials/issues/98).

This is great, however it's not really useful unless everyone in the team has the extension installed, which on its own can be difficult to enforce or manage consistently.

# Note on Solution Error Visualizer

I am using the Solution Error Visualizer plug-in as part of the free
[Productivity Power Tools for Visual Studio 2015 extension](https://visualstudiogallery.msdn.microsoft.com/34ebc6a2-2777-421d-8914-e29c1dfa7f5d) by Microsoft.  This is where the underlining in the Solution Explorer of errors and warnings comes from.

# Using Roslyn analysers as NuGet packages in a project

This brings us to the second option of using Roslyn analysers, which is to install them as a NuGet package in all projects in the Visual Studio solution.

Note that Visual Studio/Roslyn will handle that you could have the same analyser installed both as an extension (globally) and as a NuGet package (locally) and this won't lead to duplicate warnings. However there is a bug where code fixes will be duplicated which is tracked [here](https://github.com/dotnet/roslyn/issues/4030).

![](/content/2015-10-06-using-roslyn-analysers/nuget.png)

Once installed you will notice the analyser under the new *Analysers* section under *References*:

![](/content/2015-10-06-using-roslyn-analysers/references.png)

And if we look at the content of the `.csproj` file we will see this:

```xml
 <ItemGroup>
    <Analyzer Include="..\packages\RefactoringEssentials.1.2.0\analyzers\dotnet\RefactoringEssentials.dll" />
  </ItemGroup>
```

The downside is that we have to ensure that every new project added to the solution has the analyser NuGet package added to it, but automating that is not that hard (e.g. using pre-commit hooks in Git).

# Configuring Roslyn analyser rules for a single project

That's all great, but what if we don't want to remove `private` from our fields, because it doesn't conform to our code style? Or alternatively - we want it to be a hard error, so it conforms to our code style?

We can edit the current rules set by right clicking on `Analysers` and then changing the warning level:

![](/content/2015-10-06-using-roslyn-analysers/ruleset-menu.png)

![](/content/2015-10-06-using-roslyn-analysers/edit.png)

Once saved a new file will shop up in the project `RoslynAnalysersUsageSample.ruleset` and if we open it we will see that the configuration has been added to the XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RuleSet Name="Rules for RoslynAnalysersUsageSample" Description="Code analysis rules for RoslynAnalysersUsageSample.csproj." ToolsVersion="14.0">
  <Rules AnalyzerId="RefactoringEssentials" RuleNamespace="RefactoringEssentials">
    <Rule Id="RECS0145" Action="Error" />
  </Rules>
</RuleSet>
```

This is the same Rules Set file that we know from previous version of Visual Studio, which used to contain FxCop based rules. However an important note for those familiar with FxCop based Code Analysis rule sets is that all Roslyn analyser rules are enabled by default (in FxCop you had to add the rules explicitly).

# Configuring Roslyn analyser rules for the whole solution

Let's add a new project to the solution with the Refactoring Essentials NuGet package installed and copy paste the class above into the new project. We will notice that our custom `Error` level for the `RECS0145` rule is not applied. And that's because our previous customization only applied for that single project we edited:

![](/content/2015-10-06-using-roslyn-analysers/second-project.png) 

To remedy that we need to:

1. Move out the `RoslynAnalysersUsageSample.ruleset` file in a more sensible place such as the root of the solution, because it will no longer apply for a single project.
2. Add it to the solution as a "Solution Item"
3. Configure the solution-level Code Analysis to use the rule set for all projects in the solution

![](/content/2015-10-06-using-roslyn-analysers/rules.png) 

That's it!

# Suppressing warnings in special cases

In those rare cases when there is a warning, but there is a valid reason why we have done something the way we have done it the `System.Diagnostics.CodeAnalysis.SuppressMessage` attribute can be used (or evil evil #pragma).

```
[SuppressMessage("Potential Code Quality Issues", 
                 "RECS0022:A catch clause that catches System.Exception and has an empty body", 
                 Justification = "This is an explanation why I have suppressed this warning.")]
void Start()
{
    try {
        throw new NotSupportedException();
    } catch { // warning here, becase the catch is empty
    }
}
```

# Continuous Integration

No extra work is required if the code analysers are added to the solution as NuGet packages and included under `Analysers`. 

There are two things to be aware of though:

* You may see build errors if you don't do a `nuget restore .sln` on the build server before calling `msbuild`. It appears the VS build is trying to read analyser info before nuget is able to restore the packages.
* Roslyn analyser warnings are standard compiler warnings so you can use the `TreatWarningsAsErrors` MsBuild property too.

# Final words

In this post we have covered:

* What are Roslyn Analysers
* How to use them in a team or individually
* How to configure them across a project and a solution

I hope it was useful to you guys. Let me know in the comments!





