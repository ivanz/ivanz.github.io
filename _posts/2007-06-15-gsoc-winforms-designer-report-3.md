---
title: GSoC WinForms Designer Report 3
author: Ivan Zlatev
layout: post
permalink: /2007/06/15/gsoc-winforms-designer-report-3/
views:
  - 26
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1332544
categories:
  - Coding
---
**Summary**

I am sending my report earlier in, because I will be out of town for the weekend.

This week I have been researching on the logic and workings of the Design-Time Serialization and I&#8217;ve got scared. Seriously now. I have implemented a bunch of classes in the System.ComponentModel.Design.Serialization namespace. I have laid the foundations for building the CodeDom* part of the Design-Time Serialization. I am looking forward to implementing it, so I can start generating code!

**Repository Data  
**  
Start of week revision: 35  
End of week revision: 40  
Plain Text ChangeSet (~2000 lines): [http://monodt.ivanz.com/changeset?format=diff&new=40&old=35&new\_path=trunk&old\_path=trunk][1]  
Visual ChangeSet: [http://monodt.ivanz.com/changeset?old\_path=trunk&old=35&new\_path=trunk&new=40][2]

**Details  
**  
1.) As planned I have been researching on the System.ComponentModel.Design.Serialization namespace, which resulted a few pretty diagrams (and not only).

2.) I have implemented the following class library classes from the same namespace:

  * ComponentSerializationService.cs
  * ObjectStatementCollection.cs
  * RootContext.cs
  * StatementContext.cs
  * ExpressionContext.cs
  * SerializationStore.cs
  * MemberRelationship.cs
  * MemberRelationshipService.cs
  * BasicDesignerLoader.cs
  * ContextStack &#8211; Refactored and updated to 2.0
  * CodeDomDesignerLoader.cs &#8211; implemented just the explicit INameCreationService interface.

3.) I did some refactoring in the loading/unloading behaviour of the design surface to result better interaction between the DesignerHost, DesignSurface and DesignerLoader.

**Next Week**

  * Research on and implement: 
      * CodeDomSerializer
      * CodeDomSerializerBase
      * CodeDomSerializerException
      * CollectionCodeDomSerializer
      * MemberCodeDomSerializer
      * InstanceDescriptor
      * TypeCodeDomSerializer

 [1]: http://monodt.ivanz.com/changeset?format=diff&new=40&old=35&new_path=trunk&old_path=trunk
 [2]: http://monodt.ivanz.com/changeset?old_path=trunk&old=35&new_path=trunk&new=40 "http://monodt.ivanz.com/changeset?old_path=trunk&old=35&new_path=trunk&new=40"