---
title: GSoC WinForms Designer Report 4
author: Ivan Zlatev
layout: post
permalink: /2007/06/25/gsoc-winforms-designer-report-4/
views:
  - 38
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1333069
categories:
  - Coding
---
**Summary**

This week I was really struggling to get this Design-Time CodeDom Serialization code done. I think I&#8217;ve got my head around most of the architecture and how the different bits and peaces fit together, so hopefully the next week will be much more productive than this week.

** Repository Data  
**  
Start of week revision: 40  
End of week revision: 43  
Visual ChangeSet: [http://monodt.ivanz.com/changeset?old\_path=trunk&old=40&new\_path=trunk&new=43][1]  
Plain Text ChangeSet: [http://monodt.ivanz.com/changeset?format=diff&new=43&old=40&new\_path=trunk&old\_path=trunk][2]  
**Details  
**  
1) I have been researching even more on the System.ComponentModel.Design.Serialization namespace.

2) I have implemented the following class library classes from the same namespace:

  * SerializerAbsoluteContext.cs
  * MemberCodeDomSerializer.cs
  * CodeDomSerializerBase.cs &#8211; no Resources serialization and no deserialization.
  * CodeDomSerializer.cs &#8211; stubbed

**Next Week**

  * Research on and implement: 
      * CodeDomSerializer
      * CodeDomSerializerException
      * CollectionCodeDomSerializer
      * TypeCodeDomSerializer
      * EventCodeDomSerializer : MemberCodeDomSerializer
      * PropertyCodeDomSerializer : MemberCodeDomSerializer
      * FieldCodeDomSerializer : MemberCodeDomSerializer

 [1]: http://monodt.ivanz.com/changeset?old_path=trunk&old=40&new_path=trunk&new=43 "http://monodt.ivanz.com/changeset?old_path=trunk&old=35&new_path=trunk&new=40"
 [2]: http://monodt.ivanz.com/changeset?format=diff&new=43&old=40&new_path=trunk&old_path=trunk "http://monodt.ivanz.com/changeset?format=diff&new=35&old=35&new_path=trunk&old_path=tags%2Fgsoc-begin"