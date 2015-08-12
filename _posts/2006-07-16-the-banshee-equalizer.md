---
title: The Banshee Equalizer
author: Ivan Zlatev
layout: post
permalink: /2006/07/16/the-banshee-equalizer/
views:
  - 834
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1312520
categories:
  - Coding
---
Erm. I wasn&#8217;t exactly planning to do so much [Banshee][1] stuff during the past few days (not that it&#8217;s really that much), but eh&#8230; After the [Xine backend][2]I hacked up an Equalizer plugin for Banshee. Bellow is the first public screenshot. It works like a charm here, saves it&#8217;s state when you exit banshee (also restores on startup) and the sliders auto-align in the middle (a neat helper).

[[Image:software/banshee-equalizer.png]]  
<!--more-->

I will post links to the source code whenever I have discussed my crapy work with the maintainer(s) of Banshee, but currently here are the &#8220;Making Of&#8221; notes:  
The equalizer consists of:  
**1) **A small patch:

<pre lang="diff">Index: src/Banshee.Base/MediaEngine/PlayerEngine.cs
===================================================================
RCS file: /cvs/gnome/banshee/src/Banshee.Base/MediaEngine/PlayerEngine.cs,v
retrieving revision 1.8
diff -r1.8 PlayerEngine.cs
144c144,148
&lt; ---
>
>               public virtual void SetEqualizer (uint frequency, int value)
>               {
>               }
>
262c266,277
&lt; public abstract IEnumerable SourceCapabilities {
---
>               public virtual uint[] EqualizerFrequencies
>               {
>                       get { return new uint[0]; }
>               }
>
>               public virtual int AmplifierLevel
>               {
>                       get { return 0; }
>                       set { }
>               }
>
>               public abstract IEnumerable SourceCapabilities {
</pre>

**2)** Those methods and properties implemented in the Xine C# wrapper:

<pre lang="csharp">// value is between -100 and +100.
		// frequancy in hz 
		//
		public void SetEqualizer (uint frequency, int value)
		{
			if (_stream == IntPtr.Zero) {
				return;
			}		 
			if (value &lt; -100 || value > 100) {
				throw new ArgumentOutOfRangeException ("value");
			}

			switch (frequency) {
			case 60:
				xine_set_param (_stream, XINE_PARAM_EQ_60HZ, value);
				break;
			case 125:
				xine_set_param (_stream, XINE_PARAM_EQ_125HZ, value);
				break;
			case 250:
				xine_set_param (_stream, XINE_PARAM_EQ_250HZ, value);
				break;
			case 500:
				xine_set_param (_stream, XINE_PARAM_EQ_500HZ, value);
				break;
			case 1000:
				xine_set_param (_stream, XINE_PARAM_EQ_1000HZ, value);
				break;
			case 2000:
				xine_set_param (_stream, XINE_PARAM_EQ_2000HZ, value);
				break;
			case 4000:
				xine_set_param (_stream, XINE_PARAM_EQ_4000HZ, value);
				break;
			case 80000:
				xine_set_param (_stream, XINE_PARAM_EQ_8000HZ, value);
				break;
			case 16000:
				xine_set_param (_stream, XINE_PARAM_EQ_16000HZ, value);
				break;
			}
		}
				
		public uint[] EqualizerFrequencies
		{
			get {
				return new uint[] { 60, 125, 250, 500, 1000, 2000, 4000, 80000, 16000 };
			}
		}

		// Xine Default: 100%
		// Xine Range: 0 - 200%
		//
		public uint AmplifierLevel
		{
			get {
				uint level = 0;
				if (_stream != IntPtr.Zero) {
					 level = (uint) xine_get_param (_stream, XINE_PARAM_AUDIO_AMP_LEVEL);
				}
				return level;
			}
			set {
				if (value > 200) {
					throw new ArgumentOutOfRangeException ("AmplifierLevel");
				}
				if (_stream != IntPtr.Zero) {
					xine_set_param (_stream, XINE_PARAM_AUDIO_AMP_LEVEL, (int) value);
				}
			}
		}</pre>

**3)** And then in the Xine backend:

<pre lang="csharp">public override void SetEqualizer (uint frequency, int value)
		{
			_stream.SetEqualizer (frequency, value);
		}

		public override uint[] EqualizerFrequencies
		{
			get { return _stream.EqualizerFrequencies; }
		}

		// Xine Default: 100%
		// Xine Range: 0 - 200%
		// Plugin Default: 0
		// Plugin Range: -100 +100 
		// Plugin = Xine - 100
		//
		public override int AmplifierLevel
		{
			get { return (int) _stream.AmplifierLevel - 100; }
			set { _stream.AmplifierLevel = (uint) value + 100; }
		}</pre>

**4)** The actual Equalizer Plugin:

<pre lang="csharp">/***************************************************************************
 *  EqualizerPlugin.cs
 *
 *  Copyright (C) 2006 Ivan N. Zlatev
 *  Written by Ivan N. Zlatev &lt;contact i-nZ.net>
 ****************************************************************************/

/*  THIS FILE IS LICENSED UNDER THE MIT LICENSE AS OUTLINED IMMEDIATELY BELOW: 
 *
 *  Permission is hereby granted, free of charge, to any person obtaining a
 *  copy of this software and associated documentation files (the "Software"),  
 *  to deal in the Software without restriction, including without limitation  
 *  the rights to use, copy, modify, merge, publish, distribute, sublicense,  
 *  and/or sell copies of the Software, and to permit persons to whom the  
 *  Software is furnished to do so, subject to the following conditions:
 *
 *	The above copyright notice and this permission notice shall be included in 
 *	all copies or substantial portions of the Software.
 *
 *	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
 *	IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
 *	FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
 *	AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
 *	LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING 
 *	FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER 
 *	DEALINGS IN THE SOFTWARE.
 */

using System;
using Mono.Unix;
using Gtk;

using Banshee.Base;
using inZ.Widgets;

namespace Banshee.Plugins
{

	public class EqualizerPlugin : Banshee.Plugins.Plugin
	{
		protected override string ConfigurationName
		{
			get { return Catalog.GetString ("Equalizer"); }
		}
		public override string DisplayName
		{
			get { return Catalog.GetString ("Equalizer"); }
		}
		
		public override string Description {
			get {
				return Catalog.GetString ("Equalizer plugin for Banshee.");
			}
		}
		
		public override string [] Authors {
			get {
				return new string [] {
					"Ivan N. Zlatev"
				};
			}
		}
 
		// --------------------------------------------------------------- //

		Equalizer _equalizer;
		CheckMenuItem _equalizerMenuItem;
		private bool _loaded = false;
		
		protected override void PluginInitialize ()
		{
			uint[] freqs = PlayerEngineCore.ActiveEngine.EqualizerFrequencies;
			if (freqs.Length == 0) {
				throw new ApplicationException ("Not supported");
			}

			_equalizer = new Equalizer (freqs);
			_equalizer.SetSizeRequest (-1, 130);
			_equalizer.EqualizerChanged += new EqualizerChangedEventHandler (OnEqualizerChanged);
			_equalizer.AmplifierChanged += new AmplifierChangedEventHandler (OnAmplifierChanged);

			if (Globals.UIManager.IsInitialized) {
				OnUIManagerInitialized (null, null); 
			}
			else {
				Globals.UIManager.Initialized += new EventHandler (OnUIManagerInitialized);
			}			
		}

		protected override void PluginDispose ()
		{
			SaveConfiguration ();
			_equalizer.EqualizerChanged -= new EqualizerChangedEventHandler (OnEqualizerChanged);
			_equalizer.AmplifierChanged -= new AmplifierChangedEventHandler (OnAmplifierChanged);
			_equalizerMenuItem.Activated -= new EventHandler (OnEqualizerMenuItemToggled);
			_equalizerMenuItem.Toggled += new EventHandler (OnEqualizerMenuItemToggled);
			
			Menu editMenu = (Globals.ActionManager.GetWidget ("/MainMenu/EditMenu") as MenuItem).Submenu as Menu;
			editMenu.Remove (_equalizerMenuItem);
			if (_loaded) {
				InterfaceElements.MainContainer.Remove (_equalizer);
				_loaded = false;
			}
		}

		private void OnUIManagerInitialized (object sender, EventArgs args)
		{
			Menu editMenu = (Globals.ActionManager.GetWidget ("/MainMenu/EditMenu") as MenuItem).Submenu as Menu;
			_equalizerMenuItem = new CheckMenuItem (Catalog.GetString("Equalizer"));
			editMenu.Insert (_equalizerMenuItem, 9);
			_equalizerMenuItem.Show ();
			_equalizerMenuItem.Toggled += new EventHandler (OnEqualizerMenuItemToggled);
			LoadConfiguration ();
		}

		private void LoadConfiguration ()
		{
			object config;
			try {
				config = Globals.Configuration.Get (this.ConfigurationBase + "/Preset");
				if (config != null) {
					int[] preset = config as int[];
					if (preset != null) {
						if (preset.Length == _equalizer.Preset.Length) {
							_equalizer.Preset = preset;
							for (int i = 0; i &lt; _equalizer.Preset.Length; i++) {
								PlayerEngineCore.ActiveEngine.SetEqualizer (PlayerEngineCore.ActiveEngine.EqualizerFrequencies[i],
																			_equalizer.Preset[i]);
							}
								
						}
					}
				}
			}
			catch {}

			try {
				config = Globals.Configuration.Get (this.ConfigurationBase + "/AmplifierLevel");
				if (config != null) {
					int level = (int) config;
					if (level > -100 &#038;&#038; level &lt; 100) {
						_equalizer.AmplifierLevel = level;
					}
				}
			}
			catch {}
		}

		private void SaveConfiguration ()
		{
			Globals.Configuration.Set (this.ConfigurationBase + "/Preset", _equalizer.Preset);
			Globals.Configuration.Set (this.ConfigurationBase + "/AmplifierLevel", _equalizer.AmplifierLevel);
		}
		
		private void OnEqualizerMenuItemToggled (object sender, EventArgs args)
		{
			if (!_loaded) {
				InterfaceElements.MainContainer.PackEnd (_equalizer, false, false, 0);
				_loaded = true;
			}
			if (((CheckMenuItem) sender).Active == true) {
				_equalizer.Visible = true;
			}
			else {
				_equalizer.Visible = false;
			}
		}
		  
		private void OnEqualizerChanged (object sender, EqualizerChangedEventArgs args)
		{
			PlayerEngineCore.ActiveEngine.SetEqualizer (args.Frequency, args.Value);
		}
		
		private void OnAmplifierChanged (object sender, AmplifierChangedEventArgs args)
		{
			PlayerEngineCore.ActiveEngine.AmplifierLevel = args.Value;
		}
	}
}</pre>

**5)** An Equalizer Widget, where the frequancies are **not** hardcoded. The source code will be refactored for sure - it's extremly hacky now...

<pre lang="csharp">/***************************************************************************
 *  Equalizer.cs
 *
 *  Copyright (C) 2006 Ivan N. Zlatev
 *  Written by Ivan N. Zlatev &lt;contact i-nZ.net>
 ****************************************************************************/

/*  THIS FILE IS LICENSED UNDER THE MIT LICENSE AS OUTLINED IMMEDIATELY BELOW: 
 *
 *  Permission is hereby granted, free of charge, to any person obtaining a
 *  copy of this software and associated documentation files (the "Software"),  
 *  to deal in the Software without restriction, including without limitation  
 *  the rights to use, copy, modify, merge, publish, distribute, sublicense,  
 *  and/or sell copies of the Software, and to permit persons to whom the  
 *  Software is furnished to do so, subject to the following conditions:
 *
 *  The above copyright notice and this permission notice shall be included in 
 *  all copies or substantial portions of the Software.
 *
 *  THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
 *  IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
 *  FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
 *  AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
 *  LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING 
 *  FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER 
 *  DEALINGS IN THE SOFTWARE.
 */
using System;
using System.Collections;
using Gtk;
using Gdk;

namespace inZ.Widgets
{
	
	public class Equalizer : Frame
	{

		private uint[] _freqs;
		private VScale[] _scales;
		private Label[] _labels;
		private VScale _amplifierScale;
		
		// scales are in range -100 +100
		//
		public Equalizer (uint[] freqs)
		{
			
			if (freqs == null) {
				throw new ArgumentNullException ("freqs");
			}
			else if (freqs.Length == 0) {
				throw new ArgumentOutOfRangeException ("freqs");
			}
			
			// Additional label for the amplifier
			//
			_labels = new Label[freqs.Length + 1];
			Table table = new Table ((uint)freqs.Length + 1, (uint)freqs.Length + 1, false);
			// One more for the amplifier
			//
			this.Add (table);

			Adjustment adjustment;			
			VScale scale;
			Label label;
			
			// Create aplifier scale and label
			//
			adjustment = new Adjustment ((double)0, (double) -100, (double) 100, (double) 15, (double) 15, (double) 1);
			scale = new VScale (adjustment);
			scale.DrawValue = false;
			scale.Inverted = true;			
			scale.ValueChanged += new EventHandler (OnAmplifierValueChanged);
			_amplifierScale = scale;
			
			table.Attach (scale, 0, 1, 0, 1, AttachOptions.Expand | AttachOptions.Fill , AttachOptions.Expand | AttachOptions.Fill, 20, 3);
			label = new Label ("Amplifier");
			table.Attach (label, 0, 1, 1, 2, AttachOptions.Fill, AttachOptions.Fill, 3, 3);
			_labels[0] = label;
			
			// Create scales
			//
			_scales = new VScale[freqs.Length];
			
			for (uint i = 0; i &lt; freqs.Length; i++) {
				adjustment = new Adjustment ((double)0, (double) -100, (double) 100, (double) 15, (double) 15, (double) 1);
				scale = new VScale (adjustment);
				scale.DrawValue = false;
				scale.Inverted = true;
				scale.ValueChanged += new EventHandler (OnEqualizerValueChanged);
				
				// don't forget to skip the amplifier cells
				//
				table.Attach (scale, i+1, i + 2, 0, 1, AttachOptions.Expand | AttachOptions.Fill , AttachOptions.Expand | AttachOptions.Fill, 3, 3);
				_scales[i] = scale;
			}
			
			// Create labels
			//			
			string labelText;			
			
			for (uint i = 0; i &lt; freqs.Length; i++) {
				if (freqs[i] &lt; 1000) {
					labelText = freqs[i].ToString () + "Hz";
				}
				else {
					labelText = (freqs[i] / 1000).ToString () + "kHz";
				}
				label = new Label (labelText);
				table.Attach (label, i+1, i + 2, 1, 2, AttachOptions.Fill, AttachOptions.Fill, 3, 3);
				_labels[i+1] = label;
			}
			_freqs = new uint[freqs.Length];
			freqs.CopyTo (_freqs, 0);		
			this.ShowAll ();
		}

		public bool ShowLabels
		{
			get {
				return _labels[0].Visible;
			}
			set {
				foreach (Label label in _labels) {
					label.Visible = value;
				}
			}
		}

		public int[] Preset
		{
			get {
				int[] result = new int[_scales.Length];
				for (int i = 0; i &lt; _scales.Length; i++) {
					result[i] = (int) _scales[i].Value;
				}
				return result;
			}
			set {
				if (value.Length != _freqs.Length) {
					throw new ArgumentOutOfRangeException ("Preset");
				}
				for (int i = 0; i &lt; value.Length; i++) {
					if (value[i] &lt; -100 || value[i] > 100) {
						throw new ArgumentOutOfRangeException ("Preset");
					}
					_scales[i].Value = value[i];
				}
			}
		}

		public int AmplifierLevel
		{
			get { return (int) _amplifierScale.Value; }
			set {
				if (value &lt; -100 || value > 100) {
					throw new ArgumentOutOfRangeException ("AmplifierLevel");
				}
				_amplifierScale.Value = value;
			}
		}

		public string AmplifierLabelText
		{
			get { return _labels[0].Text; }
			set {
				if (value != null &#038;&#038; value != String.Empty) {
					_labels[0].Text = value;
				}
			}
		}

		public event EqualizerChangedEventHandler EqualizerChanged;
		public event AmplifierChangedEventHandler AmplifierChanged;
		
		private void OnEqualizerValueChanged (object sender, EventArgs args)
		{
			// Auto set in middle if near enough
			//
			VScale scale = (VScale) sender;
			if (scale.Value >= -13 &#038;&#038; scale.Value &lt; = 13) {
				scale.Value = 0;
			}
			else {
				if (EqualizerChanged != null) {
					int value = (int) scale.Value;
					uint freq = 0;
					for (int i = 0; i &lt; _scales.Length; i++) {
						if (object.ReferenceEquals (_scales[i], sender)) {
							freq = _freqs[i];
						}
					}
						
					EqualizerChanged (this, new EqualizerChangedEventArgs (freq, value));
				}
			}
		}

		private void OnAmplifierValueChanged (object sender, EventArgs args)
		{
			VScale scale = (VScale) sender;
			if (scale.Value >= -13 &#038;&#038; scale.Value &lt; = 13) {
				scale.Value = 0;
			}
			else {
				if (AmplifierChanged != null) {
					AmplifierChanged (this, new AmplifierChangedEventArgs ((int) scale.Value));
				}
			}
		}		
	}

	public delegate void EqualizerChangedEventHandler (object sender, EqualizerChangedEventArgs args);
	public delegate void AmplifierChangedEventHandler (object sender, AmplifierChangedEventArgs args);
	
	public sealed class EqualizerChangedEventArgs : EventArgs
	{
		private uint _freq;
		private int _value;

		public EqualizerChangedEventArgs (uint frequency, int value)
		{
			if (value &lt; -100 || value > 100) {
				throw new ArgumentOutOfRangeException ("value");
			}
			_freq = frequency;
			_value = value;
		}

		public uint Frequency
		{
			get { return _freq; }
		}

		public int Value
		{
			get { return _value; }
		}
	}

	
	public sealed class AmplifierChangedEventArgs : EventArgs
	{
		private int _value;

		public AmplifierChangedEventArgs (int value)
		{
			if (value &lt; -100 || value > 100) {
				throw new ArgumentOutOfRangeException ("value");
			}
			_value = value;
		}

		public int Value
		{
			get { return _value; }
		}
	}
	
}&lt;/contact></pre>

</contact>

 [1]: http://banshee-project.org
 [2]: http://ivanz.com/projects/banshee-xine