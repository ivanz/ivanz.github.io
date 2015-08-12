---
title: Mono.Design (WinForms Designer) Report
author: Ivan Zlatev
layout: post
permalink: /2007/08/23/monodesign-winforms-designer-report/
views:
  - 3064
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1310899
categories:
  - Coding
---
<h1 style="text-align: center; text-decoration: underline">
  Mono.Design Report
</h1>

<p style="text-align: center">
  <span style="font-weight: bold">Ivan N. Zlatev <contact at ivanz.com></span>
</p>

<p style="text-align: right">
  <span style="font-weight: bold">Revision 3</span>
</p>

# Summary

### Project Description

This project targets at implementing the .Net Design-Time stack for the Mono Class Library, mostly hosted in the System.Design assembly and the \*.Design.\* namespaces.  
The stack consists of:

  * Design Surface, which includes a host and a list of design-time services
  * Windows.Forms Designers
  * ASP.Net Designers
  * Serializers and Deserializers

### Timeline

28 May &#8211; Start  
9 July &#8211; Mid-Term Review  
20 August &#8211; End

### **Delivered**

<span style="text-decoration: underline; font-weight: bold">Planned</span>

  * DesignSurface, host and core design-time services
  * Windows.Forms core designers stack
  * Design-Time serialization stack
  * A simple frontend (the designer) to demonstrate how the above fit together. This includes drag and drop, resizing, selection, as well as source code serialization and deserialization.

<span style="font-weight: bold; text-decoration: underline">Extra</span>

  * .Net 2.0 Updates
  * MonoDevelop Addin

### Schedule

Per-week tasks-based schedule with weekly reports.

<table id="yts6" border="1" cellpadding="3" cellspacing="0">
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      <span style="font-weight: bold">Week</span>
    </td>
    
    <td style="text-align: center" valign="top" width="90%">
      <span style="font-weight: bold">Activity</span>
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <span style="font-weight: bold">Report</span>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      1
    </td>
    
    <td valign="top" width="90%">
      University Exams
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%201.html" id="govb" title="Link">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      2
    </td>
    
    <td valign="top" width="90%">
      Cover the existing code with unit tests to verify matching behavior with MS.Net.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%202.html" id="hpv." title="Link">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      3, 4, 5, 6
    </td>
    
    <td valign="top" width="90%">
      Research on and implement the serialization stack: designer loaders, serialization manager, serializers and all the missing classes in Mono&#8217;s class library required.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%203.html" id="vf8v" title="Link">Link</a><br /> <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%204.html" id="pet:" title="Link">Link</a><br /> <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%205,%206.html" id="ca97" title="Link">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      7
    </td>
    
    <td valign="top" width="90%">
      Implement the deserialization.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%204.html" id="pet:" title="Link">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      8
    </td>
    
    <td valign="top" width="90%">
      <span style="text-decoration: line-through">MonoDevelop integration work.</span><br /> <span style="font-style: italic">Revised to: </span>&#8220;Port&#8221; to Mono &#8211; bugfixes in the class library, implementing missing classes.<br /> <span style="font-style: italic">Reason: </span>Missing classes and bugs preventing the code form working on Mono.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%208.html" id="f161" title="Link">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      9
    </td>
    
    <td valign="top" width="90%">
      <span style="text-decoration: line-through">Implement System.Windows.Forms.Design.Behavior.</span> <span style="font-style: italic"><br /> Revised to:</span> MWF-in-GTK integration work.<br /> <span style="font-style: italic">Reason: </span>No transparency (WS_EX_TRANSPARENT) on X11, which is required for the implementation.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%205,%206.html" id="ca97">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      10
    </td>
    
    <td valign="top" width="90%">
      MonoDevelop Windows.Forms designer Addin.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%205,%206.html" id="ca97">Link</a>
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      11
    </td>
    
    <td valign="top" width="90%">
      Vacation
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      n/a
    </td>
  </tr>
  
  <tr>
    <td style="text-align: center" valign="top" width="8%">
      12
    </td>
    
    <td valign="top" width="90%">
      <span style="text-decoration: line-through">MonoDevelop Addin improvement work.</span><br /> <span style="font-style: italic">Revised to:</span> Standalone WinForms Designer.<br /> <span style="font-style: italic">Reason:</span> Unexpected, unresolvable issues with MWF-in-GTK.
    </td>
    
    <td style="text-align: center" valign="top" width="1%">
      <a href="http://www.ivanz.com/files/projects/gsoc2007/reports/GSoC%20Report%2011,%2012.html" id="annd" title="Link">Link</a>
    </td>
  </tr>
</table>

### Development Environment

Project managment: [http://monodt.ivanz.com][1]{#p1:q}  
Source code repository: [http://svn.ivanz.com/monodt][2]{#kwir}  
Editor: [SlickEdit][3]{#m_ja}  
Testing: [NUnit][4]{#wgtt}  
OS: [openSUSE 10.2 Linux][5]{#p5.c}  
Virtualization: [VirtualBox][6]{#lcmj}

# Status

### Mono.Design

Currently Mono.Design, but I hope the code will land in the Mono class library as soon as possible.

Implemented are:

  * DesignSurface and DesignSurfaceManager with all the documented in MSDN services offered.
  * ComponentDesigner, ControlDesigner, ParentControlDesigner, ScrollableControlDesigner, DocumentDesigner, SplitContainerDesigner
  * System.ComponentModel.Design.Serialization namespace

### MonoDevelop Addin

The few implementation approaches were broken in different ways.

The initial approach was to use a RemoteProcessObject to host the visual part of the designing process (just the UI) as provided by MonoDevelop and as utilized by the AspNetAddin. But my design involved design surface initialization (not loading) on the non-remote part and passing non-serializable objects to the remote part, which ended up being impossible as one can imagine.

The next approach was to drop remoting and run the editor process on a separate thread with it&#8217;s own GTK+ and MWF loops. It took me two days to figure out that I can&#8217;t have two gtk loops in one process, while wondering what is going badly wrong with the code.

The last approach utilizes remoting again, but the surface initialization and services logic will be completely redesigned. Unfortunately I stumbled upon DnD issues in MWF-in-GTK in MWF, which is not able to handle the specifics imposed by the XEMBED protocol. Also the last time I tried running the MWF-in-GTK code it wasn&#8217;t working.

I finally gave up on wasting more time on that, so the monodevelop designer addin code in SVN is obsolete and deprecated.

### MWF Designer

[[Image:software/mwf-designer.png|center]]

I have designed and developed a small standalone frontend to the Design-Time code. I have followed the KISS principle and I am quite happy with the resulting code &#8211; it&#8217;s simple and works. It deals in terms of a Workspace with loaded Documents. The Workspace has a list of References (to e.g assemblies with custom components). A Document is a unit of a codebehind file, user code file and in the future (once resources serialization and deserialization is implemented) a resource file.

Currently the designer:

  * Can load (in tabs) souce code in .Net 2.0 form &#8211; partial classes with a .Designer codebehind file. It is also able to save changes back to source code.
  * Has a PropertyGrid, a simple Toolbox and components dropdown list.
  * Maintains a user-modifiable References list for the user to reference assemblies with custom components and other.

### Tasks Left

An up-to-date list can be found on [http://monodt.ivanz.com/wiki/Tasks][7]{#jmj1}  
<span style="font-weight: bold"></span><span style="font-weight: bold"><br /> Surface</span>

  * DesignerOptions, WindowsFormsDesignerOptionService
  * Extender Providers &#8211; http://msdn2.microsoft.com/en-us/library/ms171835.aspx
  * IDesignerHost.Activate is invoked after the root component is added to the host. This is just my guess. It has to be further investigated when, where and by what should that be invoked.
  * Implement MenuCommandService : IMenuCommandService, MenuCommands, MenuCommand
  * LicenseContext
  * Transactions are not created &#8211; must be fixed before implementing the UndoEngine

<span style="font-weight: bold">Serialization</span>

  * Resources serialization and deserialization
  * ControlCodeDomSerializer &#8211; Resume/SuspendLayout
  * ContainerCodeDomSerializer &#8211; to serialize the container for the components and its disposing
  * UndoEngine, ComponentSerializationService, CodeDomComponentSerializationService, CodeDomDesignerLoader.IDesignerSerializationService
  * No creation expression is serialized for nested components by the ComponentCodeDomSerializer. I can&#8217;t think of a use case where this would be required.
  * InheritanceService, InheritanceAttribute &#8211; what are those about?
  * AmbientAttribute &#8211; no checks for that currently.
  * Make use of the IUIService for reporting errors

<span style="font-weight: bold"></span><span style="font-weight: bold"><br /> Designer Frontend<br /> </span>

  * Doesn&#8217;t support multi-component selection, because it only handles the ISelectionService.PrimarySelection and not the GetSelectedComponents.
  * Implement New -> *
  * Implement Close Document
  * Implement IUIService for user-friendly error reporting by the surface
  * Implement Undo/Redo &#8211; make use of the UndoEngine
  * Implement Copy, Paste, Cut, Delete &#8211; use the MenuCommandService
  * Implement Format -> *

<span style="font-weight: bold"><br /> </span><span style="font-weight: bold">mcs<br /> </span>

  * Implement EventsTab and add EventsTab support in MWF&#8217;s PropertyGrid
  * Update TypeDescriptor to .Net 2,0
  * Implement WS\_EX\_TRANSPARENT for MWF&#8217;s X11 backend &#8211; [http://bugzilla.ximian.com/show_bug.cgi?id=81135][8]{#hz2.}
  * A lot of PropertyGrid Editors are missing in Mono and the PropertyGrid &#8220;dies&#8221; easily due to the exceptions thrown

<span style="font-weight: bold">Designers</span>

  * System.Windows.Forms.Behavior namespace to replace IUISelectionService. Requires transparency (WS\_EX\_TRANSPARENT), which is not yet supported by the MWF X11 backend.
  * Implement ComponentTray
  * Make use of the IUIService for reporting errors
  * Menu has a designer in a Visual Studio assembly (Microsoft.VisualStudio.Windows.Forms.MenuDesigner) &#8211; fix that.
  * Implement the stack of specialized WinForms designers 
      * AxHostDesigner
      * BindingNavigatorDesigner
      * BindingSourceDesigner
      * ButtonBaseDesigner
      * ComboBoxDesigner
      * DataGridDesigner
      * DataGridViewColumnDesigner
      * DataGridViewComboBoxColumnDesigner
      * DataGridViewDesigner
      * DateTimePickerDesigner
      * FlowLayoutPanelDesigner
      * FolderBrowserDialogDesigner
      * GroupBoxDesigner
      * ImageListDesigner
      * LabelDesigner
      * ListBoxDesigner
      * ListViewDesigner
      * MaskedTextBoxDesigner
      * MonthCalendarDesigner
      * NotifyIconDesigner
      * OpenFileDialogDesigner
      * PanelDesigner
      * PictureBoxDesigner
      * PrintDialogDesigner
      * PropertyGridDesigner
      * RadioButtonDesigner
      * SaveFileDialogDesigner
      * ScrollableControlDesigner
      * SplitContainerDesigner
      * SplitterDesigner
      * SplitterPanelDesigner
      * StatusBarDesigner
      * TabControlDesigner
      * TableLayoutPanelDesigner
      * TabPageDesigner
      * TextBoxBaseDesigner
      * TextBoxDesigner
      * ToolBarButtonDesigner
      * ToolBarDesigner
      * ToolStripContainerDesigner
      * ToolStripContentPanelDesigner
      * ToolStripDesigner
      * ToolStripDropDownDesigner
      * ToolStripItemDesigner
      * ToolStripMenuItemDesigner
      * ToolStripPanelDesigner
      * TrackBarDesigner
      * TreeViewDesigner
      * UpDownBaseDesigner
      * WebBrowserBaseDesigner

 [1]: http://monodt.ivanz.com/ "http://monodt.ivanz.com"
 [2]: http://svn.ivanz.com/monodt "http://svn.ivanz.com/monodt"
 [3]: http://slickedit.com/ "SlickEdit"
 [4]: http://nunit.sourceforge.net/ "NUnit"
 [5]: http://www.opensuse.org/
 [6]: http://virtualbox.org/ "VirtualBox"
 [7]: http://monodt.ivanz.com/wiki/Tasks "http://monodt.ivanz.com/wiki/Tasks"
 [8]: http://bugzilla.ximian.com/show_bug.cgi?id=81135 "http://bugzilla.ximian.com/show_bug.cgi?id=81135"