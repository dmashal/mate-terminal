NULL =

if ENABLE_SKEY
SUBDIRS = skey
endif

bin_PROGRAMS = mate-terminal

BUILT_SOURCES = \
	terminal-marshal.c \
	terminal-marshal.h \
	terminal-type-builtins.c \
	terminal-type-builtins.h \
	$(NULL)

mate_terminal_SOURCES= \
	eggshell.c \
	eggshell.h \
	profile-editor.c \
	profile-editor.h \
	terminal.c \
	terminal-accels.c \
	terminal-accels.h \
	terminal-app.c \
	terminal-app.h \
	terminal-dconf.c \
	terminal-dconf.h \
	terminal-debug.c \
	terminal-debug.h \
	terminal-encoding.c \
	terminal-encoding.h \
	terminal-gsettings.c \
	terminal-gsettings.h \
	terminal-info-bar.c \
	terminal-info-bar.h \
	terminal-intl.h \
	terminal-options.c \
	terminal-options.h \
	terminal-profile.c \
	terminal-profile.h \
	terminal-screen.c \
	terminal-screen.h \
	terminal-screen-container.c \
	terminal-screen-container.h \
	terminal-search-dialog.c \
	terminal-search-dialog.h \
	terminal-tab-label.c \
	terminal-tab-label.h \
	terminal-tabs-menu.c \
	terminal-tabs-menu.h \
	terminal-util.c \
	terminal-util.h \
	terminal-version.h \
	terminal-window.c \
	terminal-window.h \
	$(NULL)

if ENABLE_SKEY
mate_terminal_SOURCES += \
	skey-popup.c \
	skey-popup.h \
	$(NULL)
endif

nodist_mate_terminal_SOURCES= $(BUILT_SOURCES)

mate_terminal_CPPFLAGS = \
	-DTERMINAL_COMPILATION \
	-DEXECUTABLE_NAME=\"mate-terminal\" \
	-DTERM_DATADIR="\"$(datadir)\"" \
	-DTERM_LOCALEDIR="\"$(datadir)/locale\"" \
	-DTERM_PKGDATADIR="\"$(pkgdatadir)\"" \
	-DTERM_HELPDIR="\"$(HELP_DIR)\"" \
	-DSN_API_NOT_YET_FROZEN \
	-DGDK_MULTIHEAD_SAFE \
	-DG_DISABLE_SINGLE_INCLUDES \
	-DPANGO_DISABLE_SINGLE_INCLUDES \
	-DATK_DISABLE_SINGLE_INCLUDES \
	-DGDK_DISABLE_SINGLE_INCLUDES \
	-DGDK_PIXBUF_DISABLE_SINGLE_INCLUDES \
	-DGTK_DISABLE_SINGLE_INCLUDES \
	$(DISABLE_DEPRECATED) \
	$(DCONF_CFLAGS) \
	$(AM_CPPFLAGS)

mate_terminal_CFLAGS = \
	$(TERM_CFLAGS) \
	$(WARN_CFLAGS) \
	$(DCONF_CFLAGS) \
	$(AM_CFLAGS)

mate_terminal_LDFLAGS = -lICE

mate_terminal_LDADD = \
	skey/libskey.la \
	$(DCONF_LIBS) \
	$(TERM_LIBS)

if WITH_SMCLIENT
mate_terminal_SOURCES += \
	eggsmclient.c \
	eggsmclient.h \
	eggsmclient-private.h \
	$(NULL)
mate_terminal_CFLAGS += $(SMCLIENT_CFLAGS)
mate_terminal_LDADD += $(SMCLIENT_LIBS)

if WITH_SMCLIENT_XSMP
mate_terminal_SOURCES += \
	eggdesktopfile.c \
	eggdesktopfile.h \
	eggsmclient-xsmp.c \
	$(NULL)
mate_terminal_CPPFLAGS += -DEGG_SM_CLIENT_BACKEND_XSMP
endif
if WITH_SMCLIENT_WIN32
mate_terminal_SOURCES += eggsmclient-win32.c
endif
if WITH_SMCLIENT_QUARTZ
mate_terminal_SOURCES += eggsmclient-osx.c
endif
endif

TYPES_H_FILES = \
	terminal-profile.h \
	$(NULL)

terminal-type-builtins.h: stamp-terminal-type-builtins.h
	@true

stamp-terminal-type-builtins.h: terminal-type-builtins.h.template $(TYPES_H_FILES)
	$(AM_V_GEN) $(GLIB_MKENUMS) --template $< $(filter-out $<,$^) > xgen-ttbh \
	&& (cmp -s xgen-ttbh terminal-type-builtins.h || cp xgen-ttbh terminal-type-builtins.h ) \
	&& rm -f xgen-ttbh \
	&& echo timestamp > $(@F)

terminal-type-builtins.c: terminal-type-builtins.c.template $(TYPES_H_FILES)
	$(AM_V_GEN) $(GLIB_MKENUMS) --template $< $(filter-out $<,$^) > xgen-ttbc \
	&& (cmp -s xgen-ttbc terminal-type-builtins.c || cp xgen-ttbc terminal-type-builtins.c ) \
	&& rm -f xgen-ttbc

terminal-marshal.h: $(srcdir)/terminal-marshal.list
	$(AM_V_GEN) ( $(GLIB_GENMARSHAL) --prefix=_terminal_marshal $(srcdir)/terminal-marshal.list \
			--header \
			--internal > terminal-marshal.h.tmp \
	&& mv terminal-marshal.h.tmp terminal-marshal.h ) \
	|| ( rm -f terminal-marshal.h.tmp && exit 1 )

terminal-marshal.c: $(srcdir)/terminal-marshal.list
	$(AM_V_GEN) ( $(GLIB_GENMARSHAL) --prefix=_terminal_marshal $(srcdir)/terminal-marshal.list \
				--header \
				--body \
				--internal > terminal-marshal.c.tmp \
		&& mv terminal-marshal.c.tmp terminal-marshal.c ) \
	|| ( rm -f terminal-marshal.c.tmp && exit 1 )

gsettingsschema_in_files = org.mate.terminal.gschema.xml.in
gsettings_SCHEMAS = $(gsettingsschema_in_files:.xml.in=.xml)
.PRECIOUS: $(gsettings_SCHEMAS)
@INTLTOOL_XML_NOMERGE_RULE@

aboutdir = $(pkgdatadir)
about_DATA = \
	terminal.about \
	$(NULL)

uimanagerdir = $(pkgdatadir)
uimanager_DATA = \
	terminal.xml \
	$(NULL)

builder_in_files = \
	encodings-dialog.glade \
	find-dialog.glade \
	keybinding-editor.glade \
	profile-manager.glade \
	profile-new-dialog.glade \
	profile-preferences.glade \
	skey-challenge.glade \
	$(NULL)

builderdir = $(pkgdatadir)
builder_DATA =  $(builder_in_files:.glade=.ui)

CLEANFILES = \
	stamp-terminal-type-builtins.h \
	mate-terminal.schemas \
	$(gsettings_SCHEMAS) \
	stamp-terminal-type-builtins.h \
	$(builder_DATA) \
	$(BUILT_SOURCES)

EXTRA_DIST = \
	terminal-marshal.list \
	terminal-type-builtins.c.template \
	terminal-type-builtins.h.template \
	extra-strings.c \
	$(about_DATA) \
	$(schema_in_files) \
	$(gsettingsschema_in_files) \
	$(uimanager_DATA) \
	$(builder_in_files) \
	$(NULL)

@GSETTINGS_RULES@

@INTLTOOL_SCHEMAS_RULE@

%.ui: %.glade
	$(AM_V_GEN) $(GTK_BUILDER_CONVERT) $< $@

-include $(top_srcdir)/git.mk
