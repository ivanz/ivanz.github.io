---
title: GSoC WinForms Designer MidTerm Report
author: Ivan Zlatev
layout: post
permalink: /2007/07/09/gsoc-winforms-designer-midterm-report/
views:
  - 39
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1329942
categories:
  - Coding
---
**Summary  
**  
I am pleased to announce that I have completed most of what I had planned for the first part of the Google Summer of Code program.

I have underestimated the complexity of the design-time framework in general. A lot of the internal behaviour (several classes) was hidden by one public interface or in some cases a lot of behaviour was spread among many interfaces, which had to work together. MSDN documentation was okay, but at the end of the day it is not meant to be an implementer&#8217;s guide and it wasn&#8217;t exactly helpful. This made implementation and testing more difficult. At a point I felt like if I was starring at the outside of a car, but I was supposed to write its internal behaviour.

**Repository Data  
**  
Start of GSoC revision: 19 (/tags/gsoc-begin)  
End of Midterm revision: 49 (/tags/gsoc-midterm)  
Visual ChangeSet: [http://monodt.ivanz.com/changeset?old\_path=tags%2Fgsoc-begin&old=&new\_path=tags%2Fgsoc-midterm&new=][1]  
Plain Text ChangeSet: [http://monodt.ivanz.com/changeset?format=diff&new=49&old=49&new\_path=tags%2Fgsoc-midterm&old\_path=tags%2Fgsoc-begin][2]

**Details  
**  
I have been working on the following namespaces. The list below is a list of limitations and things that are not yet implemented. For everything else you can refer to MSDN.

  * [System.ComponentModel.Design][3] 
      * DesignSurfaceManager is fully implemented, but not fully tested yet
      * Transactions are not created.
      * UndoEngine not implemented.
      * No support for License contexts.
  * [System.Windows.Forms.Design][4] 
      * The current implementation of drag and drop and resizing (it&#8217;s part of the designer&#8217;s stack) is temporary, until I imlpementSystem.Windows.Forms.Design.Behavior.
      * Most of the stuff is 2.0 ready except for the dependancies on the Behavior namespace.
      * ComponentTray, DesignerOptions and WindowsFormsDesignerOptionService are not implemented yet.
      * Internal specific (cosmetic) designers are missing: e.g a PanelControlDesigner to draw a border around the panel it&#8217;s designing to make it visually recognizable.
  * [System.ComponentModel.Design.Serialization][5] 
      * Serialization not fully tested yet (in progress)
      * Serialization to Resources is not yet implemented
      * Deserialization is not yet implemented (in progress)
      * CodeDomComponentSerializationService not yet implemented (just stubbed)
      * EventBindingService is not implemented yet.
  * Overall Limitations: 
      * Extenders are not supported (there is an implementation of the ExtenderService though)
      * InheritanceService and inheritance is not supported. Not planning to support those, unless someone explains me what is meant with this inheritance.
      * ActiveX is not supported

**Next Term**

  * Finish off and test the codedom serialization/deserialization. Use parts of SharpDevelop for testing.
  * Implement the [System.Windows.Forms.Design.Behavior][6] namespace and finish off the drag and drop behaviour.
  * Create a small frontend, based on SharpDevelop&#8217;s FormsDesigner Addin source code.
  * More&#8230;

 [1]: http://monodt.ivanz.com/changeset?old_path=tags%2Fgsoc-begin&old=&new_path=tags%2Fgsoc-midterm&new= "http://monodt.ivanz.com/changeset?old_path=tags%2Fgsoc-begin&old=&new_path=tags%2Fgsoc-midterm&new="
 [2]: http://monodt.ivanz.com/changeset?format=diff&new=49&old=49&new_path=tags%2Fgsoc-midterm&old_path=tags%2Fgsoc-begin "http://monodt.ivanz.com/changeset?format=diff&new=49&old=49&new_path=tags%2Fgsoc-midterm&old_path=tags%2Fgsoc-begin"
 [3]: http://msdn2.microsoft.com/en-us/library/system.componentmodel.design.aspx "System.ComponentModel.Design"
 [4]: http://msdn2.microsoft.com/en-us/library/system.windows.forms.design.aspx "System.Windows.Forms.Design"
 [5]: http://msdn2.microsoft.com/en-us/library/system.componentmodel.design.serialization.aspx "System.ComponentModel.Design.Serialization"
 [6]: http://msdn2.microsoft.com/en-us/library/system.windows.forms.design.behavior.aspx "System.Windows.Forms.Design.Behavior"