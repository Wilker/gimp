## Process this file with automake to produce Makefile.in

if PLATFORM_OSX
xobjective_c = "-xobjective-c"
framework_cocoa = -framework Cocoa
endif

if OS_WIN32
mwindows = -mwindows
WINSOCK_LIBS = -lws2_32
else
libm = -lm
endif

libgimpui = $(top_builddir)/libgimp/libgimpui-$(GIMP_API_VERSION).la
libgimpwidgets = $(top_builddir)/libgimpwidgets/libgimpwidgets-$(GIMP_API_VERSION).la
libgimp = $(top_builddir)/libgimp/libgimp-$(GIMP_API_VERSION).la
libgimpcolor = $(top_builddir)/libgimpcolor/libgimpcolor-$(GIMP_API_VERSION).la
libgimpbase = $(top_builddir)/libgimpbase/libgimpbase-$(GIMP_API_VERSION).la
libgimpconfig = $(top_builddir)/libgimpconfig/libgimpconfig-$(GIMP_API_VERSION).la
libgimpmath = $(top_builddir)/libgimpmath/libgimpmath-$(GIMP_API_VERSION).la $(libm)

libtinyscheme=tinyscheme/libtinyscheme.a $(libm)
libftx=ftx/libftx.a

if HAVE_WINDRES
include $(top_srcdir)/build/windows/gimprc-plug-ins.rule
script_fu_RC = script-fu.rc.o
endif

AM_CFLAGS = \
	-DSTANDALONE=0		\
	-DUSE_INTERFACE=1	\
	-DUSE_STRLWR=0

AM_CPPFLAGS = \
	-I$(top_srcdir)	\
	$(GTK_CFLAGS)	\
	$(GEGL_CFLAGS)	\
	-I$(includedir)	\
	$(xobjective_c)

AM_LDFLAGS = \
	$(mwindows)	\
	$(framework_cocoa)

SUBDIRS = tinyscheme ftx scripts


libexecdir = $(gimpplugindir)/plug-ins

libexec_PROGRAMS = script-fu

script_fu_SOURCES = \
	script-fu-types.h		\
	script-fu-enums.h		\
	\
	script-fu.c			\
	script-fu-console.c		\
	script-fu-console.h		\
	script-fu-eval.c		\
	script-fu-eval.h		\
	script-fu-interface.c		\
	script-fu-interface.h		\
	script-fu-text-console.h	\
	script-fu-text-console.c	\
	script-fu-intl.h		\
	script-fu-regex.c		\
	script-fu-regex.h		\
	script-fu-script.c		\
	script-fu-script.h		\
	script-fu-scripts.c		\
	script-fu-scripts.h		\
	script-fu-server.c		\
	script-fu-server.h		\
	script-fu-utils.c		\
	script-fu-utils.h		\
	scheme-wrapper.c		\
	scheme-wrapper.h

LDADD = \
	$(libgimpui)		\
	$(libgimpwidgets)	\
	$(libgimpconfig)	\
	$(libgimpmath)		\
	$(libgimp)		\
	$(libgimpcolor)		\
	$(libgimpbase)		\
	$(libtinyscheme)	\
	$(libftx)		\
	$(GTK_LIBS)		\
	$(SOCKET_LIBS)		\
	$(WINSOCK_LIBS)		\
	$(RT_LIBS)		\
	$(INTLLIBS)		\
	$(script_fu_RC)


# Perform static analysis on all *.scm files and look for usage of
# deprecated pdb procedures
check-for-deprecated-procedures-in-script-fu:
	@echo "Looking for deprecated procedures in *.scm files"
	@scm_files=`find $(top_srcdir)/plug-ins/script-fu -name "*.scm"`;       \
	deprecated_procs=`$(top_builddir)/app/gimp-$(GIMP_APP_VERSION)$(EXEEXT) --dump-pdb-procedures-deprecated`; \
	for scm_file in $$scm_files; do                                         \
	  for proc in $$deprecated_procs; do                                    \
	    if grep -qw $$proc $$scm_file; then                                 \
	      echo "$${scm_file} uses deprecated procedure '$${proc}'";         \
	    fi                                                                  \
	  done                                                                  \
	done
