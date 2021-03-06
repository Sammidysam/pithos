ACLOCAL_AMFLAGS = -I m4
DISTCHECK_CONFIGURE_FLAGS = --with-docs

# ----------- pithos --------------

pithosdir = $(pythondir)/pithos/
pithos_PYTHON = $(wildcard pithos/*.py)

pandoradir = $(pithosdir)/pandora
pandora_PYTHON = $(wildcard pithos/pandora/*.py)

pluginsdir = $(pithosdir)/plugins
plugins_PYTHON = $(wildcard pithos/plugins/*.py)

bin_SCRIPTS = bin/pithos
bin/pithos: bin/pithos.in
	@$(MKDIR_P) bin
	$(AM_V_GEN)sed                              \
		-e s!\@prefix\@!$(prefix)!          \
		-e s!\@datadir\@!$(datadir)!        \
		-e s!\@pkgdatadir\@!$(pkgdatadir)!  \
		-e s!\@pkglibdir\@!$(pkglibdir)!    \
		-e s!\@localedir\@!$(localedir)!    \
		-e s!\@pythondir\@!$(pythondir)!    \
		-e s!\@pyexecdir\@!$(pyexecdir)!    \
		-e s!\@VERSION\@!$(VERSION)!        \
		< $< > $@
	@chmod a+x $@

# ------------ data ----------------

gsettings_SCHEMAS = data/pithos.gschema.xml
@GSETTINGS_RULES@

pkgdata_DATA = pithos.gresource
noinst_DATA = $(shell $(GLIB_COMPILE_RESOURCES) --sourcedir=data --generate-dependencies data/pithos.gresource.xml)
pithos.gresource: data/pithos.gresource.xml $(noinst_DATA)
	$(AM_V_GEN) $(GLIB_COMPILE_RESOURCES) --target=$@ --sourcedir=$(top_srcdir)/data --generate $<

desktop_in_files = data/pithos.desktop.in
desktop_FILES = $(desktop_in_files:.desktop.in=.desktop)
@INTLTOOL_DESKTOP_RULE@
@DESKTOP_FILE_RULES@

appstream_in_files = data/pithos.appdata.xml.in
appstream_XML = $(appstream_in_files:.xml.in=.xml)
@INTLTOOL_XML_RULE@
@APPSTREAM_XML_RULES@

hicolor_scalabledir = $(datadir)/icons/hicolor/scalable/apps
hicolor_scalable_DATA = data/icons/hicolor/scalable/apps/pithos.svg \
                        data/icons/hicolor/scalable/apps/pithos-tray-icon.svg

hicolordir = $(datadir)/icons/hicolor/48x48/apps
hicolor_DATA = data/icons/hicolor/48x48/apps/pithos-tray-icon.png

ubuntu_lightdir = $(datadir)/icons/ubuntu-mono-light/16x16/apps
ubuntu_light_DATA = data/icons/ubuntu-mono-light/16x16/apps/pithos-tray-icon.svg

ubuntu_darkdir = $(datadir)/icons/ubuntu-mono-dark/16x16/apps
ubuntu_dark_DATA = data/icons/ubuntu-mono-dark/16x16/apps/pithos-tray-icon.svg

UPDATE_ICON_CACHE = gtk-update-icon-cache -f -t $(datadir)/icons/hicolor || :

install-data-hook:
	$(UPDATE_ICON_CACHE);

uninstall-hook:
	$(UPDATE_ICON_CACHE);

BUILT_SOURCES = pithos.gresource
SUBDIRS = po
CLEANFILES = $(pkgdata_DATA) $(bin_SCRIPTS) $(desktop_FILES) $(appstream_XML)
EXTRA_DIST = license README.md autogen.sh requirements-osx.txt bin/pithos.in data/pithos.gresource.xml \
	$(appstream_in_files) $(desktop_in_files) $(noinst_DATA) $(gsettings_SCHEMAS) $(hicolor_DATA) \
	$(hicolor_scalable_DATA) $(ubuntu_light_DATA) $(ubuntu_dark_DATA)

# -------------- docs ---------------

if WITH_DOCS

sphinx_verbose = $(sphinx_verbose_$(V))
sphinx_verbose_ = $(sphinx_verbose_$(AM_DEFAULT_VERBOSITY))
sphinx_verbose_0 = @echo "  SPHINX  " $@; $(SPHINX_BUILD) $< $@ > /dev/null;
sphinx_verbose_1 = $(SPHINX_BUILD) $< $@;

docs/html: docs docs/conf.py docs/index.rst
	$(sphinx_verbose)

all-local: docs/html

install-data-local:
	$(INSTALL) -d $(htmldir)
	cp -r docs/html $(htmldir)

uninstall-local:
	rm -rf $(htmldir)/html

clean-local:
	rm -rf docs/html

EXTRA_DIST += docs/conf.py docs/index.rst

endif
