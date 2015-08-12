---
title: 'GQB &#8211; Implementation Notes'
author: Ivan Zlatev
layout: post
permalink: /2006/05/12/gqb-implementation-notes/
views:
  - 27
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
dsq_thread_id:
  - 3166125
categories:
  - Coding
---
Recently I coded a tool called [GTK+ Quick Browser][1] &#8211; those are my implementation notes.

The code layout is as follows:  
* gqb.c &#8211; GQB code  
* utils.c|h &#8211; directory content retrieval + sorting + deallocation  
* pref-dialog.c|h &#8211; configuration frontend  
* conf.c|h &#8211; configuration backend  
* eggtrayicon.c|h &#8211; GTK+ tray container for Linux/X11

The configuration backend is based on [GKeyFile][2] with individual \*\_get and \*\_set wrapper functions for each option. If there is no configuration set for an option (or the key files is missing for example), then the default value for it is returned. The configuration file is ~/.gqb.conf .  
For the configuration front and for the GUI in general libglade is used. The dialogs are designed with Glade.  
For the file/directory handling [GLib][3] is used. Strings are being converted from GLib filename encoding to UTF-8 for use with [GTK+][4]. Talking about strings the current code is a huge mish mash &#8211; I should drop GString at a point I think.

The GTK+ GtkMenu is a container (GTK\_MENU\_SHELL), which contains GtkMenuItems. A GtkMenuItem is again just a container, containing a GtkLabel. When one adds a submenu to a menu item he actually attaches it to the menu item, so there is no child< ->parent relation between the menu items (again the child of the menu item is the label and the parent is the conatiner menu). Assuming one knows that, it is easy to retrieve a path from the menu item clicked by the user:

<pre lang="C">static GString* menu_path_from_menu_item (GtkWidget *item)
{
	GtkWidget *container_menu, *temp_item;
	gchar *label;
	GString *path;

	gtk_label_get (GTK_LABEL (GTK_BIN (item)-&gt;child), &label);
	path = g_string_new (label);
	g_string_prepend (path, G_DIR_SEPARATOR_S);	

	container_menu = gtk_widget_get_parent (item);
	temp_item = gtk_menu_get_attach_widget (GTK_MENU (container_menu));

	/* something from the main menu */
	if (temp_item == NULL) {
		return path;
	}

	while (temp_item != NULL) {
		gtk_label_get (GTK_LABEL (GTK_BIN(temp_item)-&gt;child), &label);
		g_string_prepend (path, label);
		g_string_prepend (path, G_DIR_SEPARATOR_S);

		container_menu = gtk_widget_get_parent (temp_item);
		if (GTK_IS_MENU (container_menu) == FALSE) {
			break;
		}
		temp_item = gtk_menu_get_attach_widget (GTK_MENU (container_menu));
	}

	return path;
}</pre>

I get the text of the label (menuitem->child) and prepend it to the path (in the beginning an empty string), then get the menu where the menu is, then get the menuitem to which the menu is attached, then loop until there is n. Apparently the text of the menuitem holding the submenu is the parent directory for a file/directory.

The other tricky thing with the menus is that GTK+ does not like creating submenus on the fly. What I do is to use the principle of appending to already created menus. When adding directory content I attach a blank submenu for each directory in the currently added one.

**Execution Flow**

*Initialization*

    main [main.c]
    -> gqb_run [gqb.c|h]
    -> create tray icon with on click handler
    	-> eggtrayicon.c|h

*Browsing*  
Every time a menu item is deselected the memory is freed. Select/Deselect is in the context of highlighted, not clicked.

    menuitem_select (item)
    -> if not directory do nothing
    -> menu_path_from_menu_item (item)
    -> menu_append_dir_listing (path)
    	-> menu_prepend_header
    	-> conf_show_files_get [conf.c|h]
    		-> read ~/.gqb.conf
    	-> directory_get_content(dir and/or files) [utils.c|h]
    		-> sort by name [utils.c|h]
    	-> create click/select/deselect handlers

*File Execution/MenuItem on click handler*

    menuitem_click
    -> proceed if not directory
    -> menu_path_from_menu_item
    -> conf_profile_get (shell handler) [conf.c|h]
    	-> read ~/.gqb.conf
    -> create command "shell-handler path"
    -> g_filename_from_utf8 (command)
    -> g_spawn_command_line_async (command)

 [1]: http://ivanz.com/projects/gtk-quick-browser/
 [2]: http://developer.gnome.org/doc/API/2.0/glib/glib-Key-value-file-parser.html
 [3]: http://developer.gnome.org/doc/API/2.0/glib/index.html
 [4]: http://developer.gnome.org/doc/API/2.0/gtk/index.html