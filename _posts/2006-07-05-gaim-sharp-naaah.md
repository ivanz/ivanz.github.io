---
title: 'Gaim# &#8211; Naaah&#8230;'
author: Ivan Zlatev
layout: post
permalink: /2006/07/05/gaim-sharp-naaah/
views:
  - 47
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 1464367
categories:
  - Coding
---
A few days ago I wanted to make a plugin for [Gaim][1] to control Banshee trough. E.g &#8220;/bnext&#8221; will change the track to the next one and &#8220;/bnp&#8221; will do a &#8220;/me is now playing Artist &#8211; Track Name&#8221;. Basically childish stuff. I hacked on Banshee and found that the design is really nice and it took me a couple of minutes to find [the RemotePlayer dbus interface][2]. (I am slow I know, I know. <img src="http://ivanz.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> ). Then I was nicely surprised by the fact that there \*seemed\* to exist mono bindings for gaim. Well actually there was a mono plugin loader, which is broken due to the fact it doesn&#8217;t set the ID&#8217;s of the plugins and then the unloader goes boom and segfaults. Then the actual mono bindings are missing&#8230; well there is something like&#8230;

<pre lang="csharp">Account account = Accounts.Find ("194073396", "prpl-oscar");

...
namespace Gaim
{
	public class Signal
	{
		[MethodImplAttribute(MethodImplOptions.InternalCall)]
		extern private static int _connect(IntPtr handle, object plugin, string signal, object evnt);
		
		public delegate void Handler(object[] args);
		
		public static int connect(IntPtr handle, object plugin, string signal, object evnt)
		{
			return _connect(handle, plugin, signal, evnt);
		}
	}
}</pre>

This scared the shit out of me. For more take a look [here][3]. I decided to hack on and make proper C# bindings. All fine, but then&#8230; the gaim api was exported by the gaim executable itself! I found out about libgaim-client, but then it wasn&#8217;t exporting the *\_get\_handle () functions. One needs the handle in order to connect to a signal for the specific subsystem (e.g accounts, conversations, etc). I guess this could be solved by an InternalCall implementation. Then I tried to code up a simple test like that:

<pre lang="csharp">namespace Gaim
{
	
	public class Accounts
	{
		....
		[DllImport("libgaim-client")]
		private static extern IntPtr gaim_accounts_find (string name, string protocolID);
		
		public static Account Find (string name, string protocolID)
		{
			IntPtr account = gaim_accounts_find (name, protocolID);
			
			if (account != IntPtr.Zero) {
				return new Account (account);
			}
			return null;
		}
	}

	public class Account
	{
		private IntPtr _raw = IntPtr.Zero;
		....
		internal Account (IntPtr raw)
		{
			_raw = raw;
		}
		...
	}

}</pre>

Account account was set to IntPtr.Zero and gaim debug window showed a complain about a dbus proxy&#8230; I checked if there is something dbus-ish in the gaim\_accounts\_find (&#8230;) and there wasn&#8217;t. The same thing written in a C plugin (but not linked agains libgaim-client I think) works and the account is retrieved properly&#8230;

This is when I said &#8220;Gaim# &#8211; Naaah&#8221;&#8230;

 [1]: http://gaim.sf.net
 [2]: http://cvs.gnome.org/viewcvs/*checkout*/banshee/src/RemotePlayer.cs?rev=1.9
 [3]: http://svn.sourceforge.net/viewcvs.cgi/gaim/trunk/plugins/mono