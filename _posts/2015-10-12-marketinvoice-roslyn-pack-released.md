---
title: MarketInvoice Roslyn Pack 0.1 released
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

# Innovation Mondays at MarketInvoice

At [MarketInvoice](http://tech.marketinvoice.com) we run "Innovation Mondays" on the first Monday of every Sprint (at the time of writing this post we are running two week sprints). It's an opportunity for the Engineering team to hack on new ideas and projects, explore new technologies and learn new things either in groups or individually. We discuss our achievements, prototypes and findings in our weekly two hour Wednesday lunch. We use this lunch to also have lunch and learn sessions (we recently had an F# and Akka.Net one), have an open discussion about our technology stack, processes, activities and anything that gets raised by team members prior to the team meeting.
   
One of the mini-projects from this week is the first release of our MarketInvoice Roslyn Pack, which is to become a collection of refactorings, code formatters, code analysis and code fixes. 

# Download and Install

**Download Visual Studio Extension**: [https://visualstudiogallery.msdn.microsoft.com/9a7f4b28-f068-4417-a021-8a770d352ea9](https://visualstudiogallery.msdn.microsoft.com/9a7f4b28-f068-4417-a021-8a770d352ea9)

**Source Code here**: [https://github.com/marketinvoice/MarketInvoice.CodeAnalysis](https://github.com/marketinvoice/MarketInvoice.CodeAnalysis)

# What's included

We've decided to stay minimal and have implemented two refactorings only:

## 1. Break parameters and arguments apart

![](/content/2015-10-12-marketinvoice-roslyn-pack-released/break-parameters-apart.png)
![](/content/2015-10-12-marketinvoice-roslyn-pack-released/break-arguments-apart.png)


## 2. Line-up parameters and arguments

![](/content/2015-10-12-marketinvoice-roslyn-pack-released/line-up-parameters.png)
![](/content/2015-10-12-marketinvoice-roslyn-pack-released/line-up-arguments.png)

# What about Refactoring Essentials?

We are aware (and are using) the [Refactoring Essentials](http://vsrefactoringessentials.com/), but we have decided that at least initially we would like to iterate rapidly and thus not be tied in to their release cycle. Having said that - we will definitely look into contributing some of our work to Refactoring Essentials.




