SUBDIRS =				\
	libstatus-notifier-watcher	\
	status-notifier			\
	system-tray

noinst_LTLIBRARIES = libtray.la
noinst_PROGRAMS = testtray

AM_CPPFLAGS =							\
	$(NOTIFICATION_AREA_CFLAGS)				\
	$(LIBMATE_PANEL_APPLET_CFLAGS)				\
	-I$(srcdir)						\
	-I$(srcdir)/../../libmate-panel-applet			\
	-I$(top_builddir)/libmate-panel-applet			\
	-DMATELOCALEDIR=\""$(datadir)/locale"\"		\
	-DG_LOG_DOMAIN=\""notification-area-applet"\"		\
	-DNOTIFICATION_AREA_MENU_UI_DIR=\""$(uidir)"\"		\
	-DPROVIDE_WATCHER_SERVICE=1				\
	$(DISABLE_DEPRECATED_CFLAGS)

AM_CFLAGS = $(WARN_CFLAGS)

libtray_la_SOURCES =				\
	na-box.c				\
	na-box.h				\
	na-host.c				\
	na-host.h				\
	na-item.c				\
	na-item.h

libtray_la_LIBADD =							\
	libstatus-notifier-watcher/libstatus-notifier-watcher.la	\
	status-notifier/libstatus-notifier.la				\
	system-tray/libsystem-tray.la

NOTIFICATION_AREA_SOURCES = main.c main.h

NOTIFICATION_AREA_LDADD =				\
	../../libmate-panel-applet/libmate-panel-applet-4.la	\
	libtray.la \
	$(NOTIFICATION_AREA_LIBS)			\
	$(LIBMATE_PANEL_APPLET_LIBS)


testtray_SOURCES = testtray.c
testtray_LDADD =			\
	libtray.la \
	$(NOTIFICATION_AREA_LIBS)

if NOTIFICATION_AREA_INPROCESS
APPLET_IN_PROCESS = true
APPLET_LOCATION   = $(pkglibdir)/libnotification-area-applet.so

notification_area_appletlibdir = $(pkglibdir)
notification_area_appletlib_LTLIBRARIES = libnotification-area-applet.la
libnotification_area_applet_la_SOURCES = $(NOTIFICATION_AREA_SOURCES)
libnotification_area_applet_la_LIBADD = $(NOTIFICATION_AREA_LDADD)
libnotification_area_applet_la_LDFLAGS = -module -avoid-version
libnotification_area_applet_la_CFLAGS = $(AM_CFLAGS)
else
APPLET_IN_PROCESS = false
APPLET_LOCATION   = $(libexecdir)/notification-area-applet

libexec_PROGRAMS = notification-area-applet
notification_area_applet_SOURCES = $(NOTIFICATION_AREA_SOURCES)
notification_area_applet_LDADD = $(NOTIFICATION_AREA_LDADD)
notification_area_applet_CFLAGS = $(AM_CFLAGS)
endif

appletdir       = $(datadir)/mate-panel/applets
applet_in_files = org.mate.panel.NotificationAreaApplet.mate-panel-applet.in
applet_DATA     = $(applet_in_files:.mate-panel-applet.in=.mate-panel-applet)

$(applet_in_files): $(applet_in_files).in Makefile
	$(AM_V_GEN)sed \
		-e "s|\@LOCATION\@|$(APPLET_LOCATION)|" \
		-e "s|\@IN_PROCESS\@|$(APPLET_IN_PROCESS)|" \
		-e "s|\@VERSION\@|$(PACKAGE_VERSION)|" \
		$< > $@

@PANEL_INTLTOOL_MATE_PANEL_APPLET_RULE@

if !NOTIFICATION_AREA_INPROCESS
servicedir       = $(datadir)/dbus-1/services
service_in_files = org.mate.panel.applet.NotificationAreaAppletFactory.service.in
service_DATA     = $(service_in_files:.service.in=.service)

org.mate.panel.applet.NotificationAreaAppletFactory.service: $(service_in_files)
	$(AM_V_GEN)sed \
		-e "s|\@LOCATION\@|$(APPLET_LOCATION)|" \
		$< > $@
endif

uidir = $(datadir)/mate-panel/ui
ui_DATA = notification-area-menu.xml

EXTRA_DIST =								\
	org.mate.panel.NotificationAreaApplet.mate-panel-applet.in.in	\
	$(ui_DATA)							\
	$(service_in_files)

CLEANFILES =			\
	$(applet_DATA) 		\
	$(applet_DATA).in	\
	$(service_DATA)

-include $(top_srcdir)/git.mk
