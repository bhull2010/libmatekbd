if BUILD_TESTS
TEST = test
endif

SUBDIRS = libmatekbd $(TEST) po

ACLOCAL_AMFLAGS = -I m4

DISTCLEANFILES = \
  intltool-extract \
  intltool-merge \
  intltool-update
 
EXTRA_DIST = \
        libmatekbd.pc.in \
        libmatekbdui.pc.in \
        intltool-extract.in \
        intltool-merge.in \
        intltool-update.in

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libmatekbd.pc libmatekbdui.pc
