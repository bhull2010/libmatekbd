lib_LTLIBRARIES = libmatekbd.la libmatekbdui.la

uidir = $(pkgdatadir)/ui
ui_DATA = show-layout.ui

common_CFLAGS = \
	-I$(top_srcdir) -Wall 			\
	$(GDK_CFLAGS)					\
	$(GIO_CFLAGS)					\
	$(LIBXKLAVIER_CFLAGS)				\
	$(XLIB_CFLAGS)					\
	-I$(top_srcdir)/intl				\
	-DG_LOG_DOMAIN=\"MateKbdIndicator\"		\
	-DDATADIR=\"$(datadir)\"			\
	-DSYSCONFDIR=\"$(sysconfdir)\"			\
	-DLIBDIR=\"$(libdir)\"				

libmatekbd_la_CFLAGS = $(common_CFLAGS)

libmatekbdui_la_CFLAGS = $(common_CFLAGS)		\
	-DUIDIR=\"$(uidir)\"				\
	$(GTK_CFLAGS)

common_LDFLAGS = -version-info @VERSION_INFO@ -no-undefined

common_LIBADD = \
		$(GDK_LIBS) \
		$(GIO_LIBS) \
		$(LIBXKLAVIER_LIBS) \
		$(XLIB_LIBS)

libmatekbd_la_LDFLAGS = $(common_LDFLAGS)
libmatekbd_la_LIBADD =  $(common_LIBADD)

libmatekbdui_la_LDFLAGS = $(common_LDFLAGS)
libmatekbdui_la_LIBADD = libmatekbd.la \
                           $(common_LIBADD) \
                           $(GLIB_LIBS) \
                           $(GMODULE_LIBS) \
                           $(GTK_LIBS)

libmatekbd_la_SOURCES = \
                         matekbd-desktop-config.c \
                         matekbd-keyboard-config.c \
                         matekbd-util.c

libmatekbdui_la_SOURCES = \
                         matekbd-indicator-config.c \
                         matekbd-indicator.c \
                         matekbd-status.c \
                         matekbd-indicator-marshal.c \
                         matekbd-keyboard-drawing-marshal.c \
                         matekbd-keyboard-drawing.c

BUILT_SOURCES = matekbd-indicator-marshal.c \
	matekbd-indicator-marshal.h         \
        matekbd-keyboard-drawing-marshal.c            \
        matekbd-keyboard-drawing-marshal.h

CLEANFILES = $(BUILT_SOURCES)

matekbdincdir = $(includedir)/libmatekbd
matekbdinc_HEADERS = \
                      matekbd-desktop-config.h \
                      matekbd-keyboard-config.h \
                      matekbd-indicator.h \
                      matekbd-status.h \
                      matekbd-indicator-config.h \
                      matekbd-keyboard-drawing.h \
                      matekbd-util.h

@INTLTOOL_DESKTOP_RULE@

noinst_HEADERS = $(extra_nih) \
                 matekbd-config-private.h

@INTLTOOL_XML_NOMERGE_RULE@
gsettings_SCHEMAS = org.mate.peripherals-keyboard-xkb.gschema.xml
@GSETTINGS_RULES@

%.gschema.xml.in: %.gschema.xml.in.in Makefile
	$(AM_V_GEN) $(SED) -e 's^\@datadir\@^$(datadir)^g' -e 's^\@GETTEXT_PACKAGE\@^$(GETTEXT_PACKAGE)^g' < $< > $@

convertdir = $(datadir)/MateConf/gsettings
convert_DATA = matekbd.convert

EXTRA_DIST = matekbd-indicator-marshal.list \
             matekbd-keyboard-drawing-marshal.list \
             $(gsettings_SCHEMAS) $(convert_DATA) $(ui_DATA)

GLIB_GENMARSHAL = $(shell pkg-config --variable=glib_genmarshal glib-2.0)

matekbd-indicator-marshal.h: matekbd-indicator-marshal.list
	$(AM_V_GEN)$(GLIB_GENMARSHAL) --prefix=matekbd_indicator $(srcdir)/matekbd-indicator-marshal.list --header > $@

matekbd-indicator-marshal.c: matekbd-indicator-marshal.h
	$(AM_V_GEN)$(GLIB_GENMARSHAL) --prefix=matekbd_indicator $(srcdir)/matekbd-indicator-marshal.list --body > $@

matekbd-keyboard-drawing-marshal.h: matekbd-keyboard-drawing-marshal.list
	$(AM_V_GEN)( $(GLIB_GENMARSHAL) --prefix=matekbd_keyboard_drawing $(srcdir)/matekbd-keyboard-drawing-marshal.list \
                            --header > matekbd-keyboard-drawing-marshal.tmp \
          && mv matekbd-keyboard-drawing-marshal.tmp matekbd-keyboard-drawing-marshal.h ) \
        || ( rm -f matekbd-keyboard-drawing-marshal.tmp && exit 1 )
 
matekbd-keyboard-drawing-marshal.c: matekbd-keyboard-drawing-marshal.h
	$(AM_V_GEN)( $(GLIB_GENMARSHAL) --prefix=matekbd_keyboard_drawing $(srcdir)/matekbd-keyboard-drawing-marshal.list \
                            --body > matekbd-keyboard-drawing-marshal.tmp \
          && mv matekbd-keyboard-drawing-marshal.tmp matekbd-keyboard-drawing-marshal.c ) \
        || ( rm -f matekbd-keyboard-drawing-marshal.tmp && exit 1 )
