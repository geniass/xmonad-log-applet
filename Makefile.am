plugindir = $(PLUGIN_DIR)
plugin_PROGRAMS = xmonad-log-applet

xmonad_log_applet_SOURCES = main.c

xmonad_log_applet_CPPFLAGS = \
			     $(GLIB_CFLAGS) \
			     $(DBUS_GLIB_CFLAGS) \
			     $(LIBPANEL_CFLAGS) \
			     $(LIBMATEPANEL_CFLAGS) \
			     @GTK_CFLAGS@

xmonad_log_applet_LDADD = \
			  $(GLIB_LIBS) \
			  $(DBUS_GLIB_LIBS) \
			  $(LIBPANEL_LIBS) \
			  $(LIBMATEPANEL_LIBS) \
			  @GTK_LIBS@

appletdir = $(LIBPANEL_APPLET_DIR)
if PANEL_XFCE4
applet_files = xmonad-log-applet.desktop
$(applet_files): $(applet_files:.desktop=.desktop.in)
	$(SED) -e "s|\@PLUGIN_DIR\@|$(PLUGIN_DIR)|" $< > $@
else
if PANEL_MATE
applet_files = org.mate.panel.XmonadLogApplet.mate-panel-applet
$(applet_files): $(applet_files:.mate-panel-applet=.mate-panel-applet.in)
	$(SED) -e "s|\@PLUGIN_DIR\@|$(PLUGIN_DIR)|" $< > $@
else
applet_files = org.gnome.panel.XmonadLogApplet.panel-applet
$(applet_files): $(applet_files:.panel-applet=.panel-applet.in)
	$(SED) -e "s|\@PLUGIN_DIR\@|$(PLUGIN_DIR)|" $< > $@
endif
endif

applet_DATA = $(applet_files)

if !PANEL_XFCE4
if !PANEL_MATE
servicedir = $(SESSION_BUS_SERVICES_DIR)
service_in_files = org.gnome.panel.applet.XmonadLogAppletFactory.service.in
service_DATA = $(service_in_files:.service.in=.service)
org.gnome.panel.applet.XmonadLogAppletFactory.service: $(service_in_files)
   	 $(SED) -e "s|\@PLUGIN_DIR\@|$(PLUGIN_DIR)|" $< > $@
else
servicedir = $(SESSION_BUS_SERVICES_DIR)
service_in_files = org.mate.panel.applet.XmonadLogAppletFactory.service.in
service_DATA = $(service_in_files:.service.in=.service)
org.mate.panel.applet.XmonadLogAppletFactory.service: $(service_in_files)
	$(SED) -e "s|\@PLUGIN_DIR\@|$(PLUGIN_DIR)|" $< > $@
endif
endif

DISTCLEANFILES = \
		 $(applet_DATA) \
		 $(service_DATA)

EXTRA_DIST = \
	     AUTHORS.md \
	     LICENSE \
	     README.md \
	     xmonad.hs \
	     xmonad-log-applet48.png \
	     org.gnome.panel.XmonadLogApplet.panel-applet.in \
	     org.mate.panel.XmonadLogApplet.panel-applet.in \
	     xmonad-log-applet.desktop.in \
	     $(service_in_files)

gtk_update_icon_cache = gtk-update-icon-cache -f -t

update-icon-cache:
	@-if test -z "$(DESTDIR)"; then \
	    echo "Updating Gtk icon cache."; \
	    $(gtk_update_icon_cache) $(datadir)/icons/hicolor; \
	    else \
	    echo "*** Icon cache not updated.  After (un)install, run this:"; \
	    echo "*** $(gtk_update_icon_cache) $(datadir)/icons/hicolor"; \
	    fi

install-icons:
	mkdir -p $(DESTDIR)$(datadir)/icons/hicolor/48x48/apps; \
	    $(INSTALL_DATA) $(srcdir)/xmonad-log-applet48.png $(DESTDIR)$(datadir)/icons/hicolor/48x48/apps/xmonad-log-applet.png;

uninstall-icons:
	rm -f $(DESTDIR)$(datadir)/icons/hicolor/48x48/apps/xmonad-log-applet.png;

install-data-local: install-icons update-icon-cache

uninstall-local: uninstall-icons update-icon-cache
