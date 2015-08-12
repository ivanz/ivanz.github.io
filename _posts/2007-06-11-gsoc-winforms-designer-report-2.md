---
title: GSoC WinForms Designer Report 2
author: Ivan Zlatev
layout: post
permalink: /2007/06/11/gsoc-winforms-designer-report-2/
views:
  - 40
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1364863
categories:
  - Coding
---
**Introduction**

That was one hell of a week and I am glad I have achieved all I had planned and even a bit more.

I have tested, fixed and refactored what I define to be the core designer and host code. This is everything but the visual user manipulation on the design-time surface. I am ready to proceed to the the design-time serialization, which is the 3rd very important component of the core code. Essentially as my first deliverable for the Mid-Term review I am planning on delivering the core code and as a second deliverable at the end of theGSoC I will deliver a basic frontend to it.

On the base of the work done this week I have refined my schedule.

**Repository Data**

Start of week revision: 19  
End of week revision: 35  
Plain Text ChangeSet (~5000 lines): [http://monodt.ivanz.com/changeset?format=diff&new=35&old=35&new\_path=trunk&old\_path=tags%2Fgsoc-begin][1]  
Visual ChangeSet: [http://monodt.ivanz.com/changeset?old\_path=%2Ftags%2Fgsoc-begin&old=&new\_path=%2Ftrunk&new=][2]

**Details**

0.) In the beginning of the week I have spent some time to create an infrastructure for design-time testing and mainly to make my life easier. It has been implemented in the form of a *DesignerTestHelper* class, which allowed me to easily setup tests&#8217; SetUp/TearOff, initialize a design surface and use helper methods to interact with it. Most importantly it allowed me to do some hacks to test on 1.1 as well (All of the hosting/services code is 2.0, where as the designers are 1.1+2.0).

I have stumbled upon a number of problems with testing the design-time code in the NUnit environment.

  * Drag and drop registration on win32 required running the test in a STA &#8220;environment&#8221;, which took me a while to figure out how to do.
  * Multi-Threaded tests &#8211; I had a lot of problems with that in NUnit and ended up refactoring to single-threaded based tests. I suspect I will have to give the issue a bit of more thought when it comes up to testing the visual behaviour (e.g win32 message routing), if I have to.
  * I get some exceptions on TearDown of some tests (I think 2), more than likely due to some internal MS design-time behaviour, of which I am not aware.

1.) I have covered the core winforms designers stack with unit tests:

  * ComponentDesigner (ITreeDesigner, IDesignerFilter)
  * ControlDesigner
  * ParentControlDesigner
  * ScrollableControlDesigner
  * DocumentDesigner

2.) I have covered the design surface (host) and its services (listed by interface name) with unit tests:

  * DesignSurface (IServiceContainer)
  * IDesignerHost
  * IContainer
  * ISite
  * INestedSite
  * INestedContainer
  * IDictionaryService
  * ITypeDescriptorFilterService

3) The following are new implementations:

  * NestedContainer
  * NestedContainerTest.cs
  * DesignModeNestedContainer and DesignModeNestedSite
  * ReferenceService
  * ComponentDesignerTest.cs
  * ControlDesignerTest.cs
  * DesignSurfaceTest.cs
  * DocumentDesignerTest.cs
  * ParentControlDesignerTest.cs

4) The tests resulted:

  * A lot of refactoring and a lot of MS behavioural compatibility fixes.
  * I have found that bits of information from MSDN2 on different classes is wrong and fixed that.
  * Complete code review.
  * A very nice way to get my head around a lot of different small bits that were not clean before.
  * They all pass on Mono and MSNET.

5) On the base of the work done this week I have refined my schedule.

**Next Week**

I won&#8217;t have as much time to spend on the project next week, due to personal stuff, which hopefully will take about 2 days. On top of everything today I passed out and hit my head while falling of the chair in a lab, where I went for a blood probe. I&#8217;ve been having a terrible headache and haven&#8217;t been feeling very well today (Monday), so I haven&#8217;t managed to achieve anything but write this report .And no, I do not get sick from seeing mine or others blood, so passing out today was something I wasn&#8217;t expecting to happen.

  * Research on Design-Time Serialization and more specifically on the: 
      * Interaction between SerializationManager &#8211; DesignerLoader &#8211; Serializer
      * Code reuse in the specialized serializers (ImageListCodeDomSerializer, CollectionCodeDomSerializer, etc)
  * Classes to look at: 
      * DesignerSerializationManager
      * DesignerLoader
      * BasicDesignerLoader
      * CodeDomDesignerLoader
      * CodeDomSerializerBase
      * CodeDomSerializer
  * Begin writing unit tests for the above.

 [1]: http://monodt.ivanz.com/changeset?format=diff&new=35&old=35&new_path=trunk&old_path=tags%2Fgsoc-begin "http://monodt.ivanz.com/changeset?format=diff&new=35&old=35&new_path=trunk&old_path=tags%2Fgsoc-begin"
 [2]: http://monodt.ivanz.com/changeset?old_path=%2Ftags%2Fgsoc-begin&old=&new_path=%2Ftrunk&new= "http://monodt.ivanz.com/changeset?old_path=%2Ftags%2Fgsoc-begin&old=&new_path=%2Ftrunk&new="