
HOSTNAME = $(shell hostname)

ifeq ($(HOSTNAME),vipe.technion.ac.il)
	VIPE = 1
endif

ifdef BERLIOS
	ifdef VIPE
		D = $(HOME)/progs/fcs/berlios-site
	else
		D = /var/www/html/shlomi/usr-bin-perl
	endif
	WML_FLAGS = -DBERLIOS=BERLIOS
else
	ifdef VIPE
		D = $(HOME)/public_html/freecell-solver/
	else
		D = /var/www/html/shlomi/usr-bin-perl
	endif
	WML_FLAGS = 
endif
#D = /home/httpd/html/ip-noise

IMAGES_PRE1 = $(shell cd src && (ls *.tar.gz *.zip *.patch *.css *.png))
IMAGES = $(addprefix $(D)/,$(IMAGES_PRE1))

# WML_FLAGS = -DBERLIOS=BERLIOS

WML_FLAGS += --passoption=2,-X

HTMLS = $(D)/index.html $(D)/download.html $(D)/links.html $(D)/characters.html

ARC_DOCS = $(D)/README $(D)/USAGE $(D)/ChangeLog $(D)/TODO $(D)/AUTHORS

INCLUDES_PROTO = std/logo.wml
INCLUDES = $(addprefix src/,$(INCLUDES_PROTO))

SUBDIRS_WITH_INDEXES = $(D)/win32_build $(D)/win32_build/bootstrap $(D)/win32_build/dynamic $(D)/win32_build/static

SUBDIRS = $(SUBDIRS_WITH_INDEXES)

INDEXES = $(addsuffix /index.html,$(SUBDIRS_WITH_INDEXES))

IMAGES += $(addprefix $(D)/win32_build/,bootstrap/curl.exe bootstrap/build.bat static/zip.exe static/unzip.exe dynamic/fcs.zip)

all : $(SUBDIRS) $(HTMLS) $(IMAGES) $(RAW_SUBDIRS) $(ARC_DOCS) $(INDEXES)

$(SUBDIRS) :: % : 
	@if [ ! -e $@ ] ; then \
		mkdir $@ ; \
	fi

$(HTMLS) :: $(D)/% : src/%.wml src/.wmlrc template.wml $(INCLUDES)
	cd src && wml $(WML_FLAGS) -DFILENAME="$(notdir $@)" $(notdir $<) > $@

$(IMAGES) :: $(D)/% : src/%
	cp -f $< $@

$(RAW_SUBDIRS) :: $(D)/% : src/%
	rm -fr $@
	cp -r $< $@

# Build index.html pages for the appropriate sub-directories.
$(INDEXES) :: $(D)/%/index.html : %
	MYPATH=`pwd` (cd $< && perl "$$MYPATH"/gen_index.pl $@)


