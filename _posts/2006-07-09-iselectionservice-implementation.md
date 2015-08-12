---
title: ISelectionService implementation
author: Ivan Zlatev
layout: post
permalink: /2006/07/09/iselectionservice-implementation/
views:
  - 498
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1325826
categories:
  - Coding
---
I&#8217;ve been struggling with the SelectionService implementation for a while already. I found out that MSDN2 docs were missing the obsolete part of half of the SelectionTypes enum members. For the purpose of testing I replaced the M$&#8217;s default implementation in the DesignSurface with my own (ISelectionService is one of the replaceable services in the DesignSurface service container). The results were scary&#8230; Seems like the M$ implementation is doing either hardcoding crap or just pure magic. From the few tests made it doesn&#8217;t take advantage of the SelectionTypes enum at all. After a few days hacking on the service I gave up on coding a M$ DesignSurface compatible SelectionService. Infact I feel like I&#8217;ve wasted my time on that one &#8211; I don&#8217;t need a M$ DesignSurface compatible SelectionService. Here you go with the actual SelectionService source code. (it&#8217;s not final version of course 😉 )  
<!--more-->

<pre lang="csharp">//
// System.ComponentModel.Design.SelectionService
//
// Authors:		
//		Ivan N. Zlatev (contact i-nZ.net)
//
// (C) 2006 Ivan N. Zlatev

//
// Permission is hereby granted, free of charge, to any person obtaining
// a copy of this software and associated documentation files (the
// "Software"), to deal in the Software without restriction, including
// without limitation the rights to use, copy, modify, merge, publish,
// distribute, sublicense, and/or sell copies of the Software, and to
// permit persons to whom the Software is furnished to do so, subject to
// the following conditions:
// 
// The above copyright notice and this permission notice shall be
// included in all copies or substantial portions of the Software.
// 
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
// EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
// MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
// NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
// LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
// OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
// WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
//

#if NET_2_0

using System;
using System.Collections;
using System.ComponentModel.Design;
using System.Windows.Forms;

namespace System.ComponentModel.Design
{
	
	internal class SelectionService : ISelectionService
	{
		
		private ArrayList _selection;
		private IServiceProvider _serviceProvider;
		private IComponent _primarySelection;
		
		public event EventHandler SelectionChanging;
		public event EventHandler SelectionChanged;
		
		public SelectionService (IServiceProvider serviceProvider)
		{
			_selection = new ArrayList();
			_serviceProvider = serviceProvider;
		}
		
		public ICollection GetSelectedComponents() 
		{
			return _selection.ToArray ();
		}

		protected virtual void OnSelectionChanging ()
		{
			if (SelectionChanging != null) {
				SelectionChanging (this, EventArgs.Empty);
			}
		}
		
		protected virtual void OnSelectionChanged ()
		{
			if (SelectionChanged != null) {
				SelectionChanged (this, EventArgs.Empty);
			}
		}

		public object PrimarySelection 
		{
			get { return _primarySelection; }
		}
 		
		public int SelectionCount 
		{
			get { return _selection.Count; }
		}


		private IComponent _RootComponent
		{
			get {
				if (_serviceProvider != null) {
					IDesignerHost designerHost = _serviceProvider.GetService (typeof (IDesignerHost)) as IDesignerHost;
					if (designerHost != null) {
						return designerHost.RootComponent;
					}
				}
				throw new InvalidOperationException ("No RootComponent");
			}
		}
		
		public bool GetComponentSelected (object component) 
		{
			return _selection.Contains (component);
		}

		public void SetSelectedComponents (ICollection components) 
 		{
			SetSelectedComponents (components, SelectionTypes.Auto);
		}

		// If the array is a null reference or does not contain any components,
		// SetSelectedComponents selects the top-level component in the designer.
		//
		public void SetSelectedComponents (ICollection components, SelectionTypes selectionType)
		{
			bool primary, add, remove, replace, toggle, auto;
			primary = add = remove = replace = toggle = auto = false;
			
			OnSelectionChanging ();

			if (_selection == null) {
				throw new InvalidOperationException("_selection == null");
			}
			
			if (components == null || components.Count == 0) {
				components = new ArrayList ();
				((ArrayList) components).Add (_RootComponent);
				selectionType = SelectionTypes.Replace;
			}
			
			if (!Enum.IsDefined (typeof (SelectionTypes), selectionType)) {
				selectionType = SelectionTypes.Auto;
			}
			
			auto = ((selectionType &#038; SelectionTypes.Auto) == SelectionTypes.Auto);
			
			if (auto) {
				toggle = (((Control.ModifierKeys &#038; Keys.Control) == Keys.Control) || ((Control.ModifierKeys &#038; Keys.Shift) == Keys.Shift));
				if (!toggle) {
					if (components.Count == 1) {
						primary = true;
					}
					else {
						replace = true;
					}
				}
			}
			else {
				primary = ((selectionType &#038; SelectionTypes.Primary) == SelectionTypes.Primary);
				add = ((selectionType &#038; SelectionTypes.Add) == SelectionTypes.Add);
				remove = ((selectionType &#038; SelectionTypes.Remove) == SelectionTypes.Remove);
				replace = ((selectionType &#038; SelectionTypes.Replace) == SelectionTypes.Replace);
				toggle = ((selectionType &#038; SelectionTypes.Toggle) == SelectionTypes.Toggle);
			}

			
			if (replace) {
				_selection.Clear ();
				add = true;
			}
						
			if (add) {
				foreach (object component in components) {
					if (component is IComponent &#038;&#038; !_selection.Contains (component)) {
						_selection.Add (component);
						_primarySelection = (IComponent) component;
					}
				}
			}

			if (remove) {
				foreach (object component in components) {
					if (component is IComponent &#038;&#038; _selection.Contains (component)) {
						_selection.Remove (component);
						if (component == _primarySelection) {
							_primarySelection = _RootComponent;
						}
					}
				}
			}

			if (toggle) {
				foreach (object component in components) {
					if (component is IComponent) {
						if (_selection.Contains (component)) {
							_selection.Remove (component);
							if (component == _primarySelection) {
								_primarySelection = _RootComponent;
							}
						}
						else {
							_selection.Add (component);
							_primarySelection = (IComponent) component;
						}
					}
				}
			}
                
			if (primary) {
				object primarySelection = null;

				foreach (object component in components) {
					primarySelection = component;
					break;
				}
				
				if (_selection.Contains(primarySelection)) {
					_primarySelection = (IComponent) primarySelection;
				} 
			}				
						
			OnSelectionChanged ();
		}
		
	}

}
#endif</pre>