# Make file for Intel compiler on Linux or compatible OS
# The license.html file describes the conditions under which this software may be distributed.
#

# Before running "make" the COMPILER environment variables must be set.
# To set the environment variables "source" the compiler environment script:
#     source /opt/intel/bin/compilervars.sh  [ ia32 | intel64 ]
# Refer to the install instructions for details.
# If the environment variables are set, INTEL_LICENSE_FILE will be defined.
ifndef INTEL_LICENSE_FILE
    $(error The compiler environment variables are not set)
endif

# list of source files
SRC = astyle_main.cpp \
      ASBeautifier.cpp \
      ASFormatter.cpp \
      ASEnhancer.cpp \
      ASLocalizer.cpp \
      ASResource.cpp

# source directories
vpath %.cpp ../../src
vpath %.h   ../../src

# NOTE for java compiles the environment variable $JAVA_HOME must be set
# example: export JAVA_HOME=/usr/lib/jvm/java-6-sun-1.6.0.00
ifndef JAVA_HOME
    JAVA_HOME = /usr/lib/jvm/default-java
endif

# set prefix if not defined on the command line
ifndef prefix
    prefix=/usr
endif
SYSCONF_PATH=$(prefix)/share/doc/astyle
# the path was changed in release 2.01
# SYSCONF_PATH_OLD may be removed at the appropriate time
SYSCONF_PATH_OLD=$(prefix)/share/astyle

# define macros
bindir = bin
objdir = obj
ipath=$(prefix)/bin

CBASEFLAGS = -w2 -Wall -fno-rtti -fno-exceptions
JAVAINCS   = -I$(JAVA_HOME)/include
CXX = icpc
INSTALL=install -o $(USER) -g $(USER)

##################################################

# define compile options for each build
ifdef CFLAGS
    CFLAGSr   = -DNDEBUG $(CBASEFLAGS) $(CFLAGS)
    CFLAGSd   = -g $(CBASEFLAGS) $(CFLAGS)
else
    CFLAGSr   = -DNDEBUG -O3 $(CBASEFLAGS)
    CFLAGSd   = -g $(CBASEFLAGS)
endif
CFLAGSs   = -DASTYLE_LIB -fpic -fvisibility=hidden $(CFLAGSr)
CFLAGSsd  = -DASTYLE_LIB -fpic -fvisibility=hidden $(CFLAGSd)
CFLAGSa   = -DASTYLE_LIB $(CFLAGSr)
CFLAGSad  = -DASTYLE_LIB $(CFLAGSd)
CFLAGSsj  = -DASTYLE_JNI -fpic -fvisibility=hidden $(CFLAGSr) $(JAVAINCS)
CFLAGSsjd = -DASTYLE_JNI -fpic -fvisibility=hidden $(CFLAGSd) $(JAVAINCS)

# define link options
ifdef LDFLAGS
    LDFLAGSr   = $(LDFLAGS)
    LDFLAGSd   = $(LDFLAGS)
else
    LDFLAGSr   = -s -static-intel
    LDFLAGSd   = -static-intel
endif

# object files are built from the source list $(SRC)
# a suffix is added for each build
OBJ   = $(patsubst %.cpp,$(objdir)/%.o,$(SRC))
OBJd  = $(patsubst %.cpp,$(objdir)/%_d.o,$(SRC))
OBJs  = $(patsubst %.cpp,$(objdir)/%_s.o,$(SRC))
OBJsd = $(patsubst %.cpp,$(objdir)/%_sd.o,$(SRC))
OBJa  = $(patsubst %.cpp,$(objdir)/%_a.o,$(SRC))
OBJad = $(patsubst %.cpp,$(objdir)/%_ad.o,$(SRC))
OBJsj  = $(patsubst %.cpp,$(objdir)/%_sj.o,$(SRC))
OBJsjd = $(patsubst %.cpp,$(objdir)/%_sjd.o,$(SRC))

# define object file rule (with the suffix) for each build

# OBJ
$(objdir)/%.o:  %.cpp  astyle.h  astyle_main.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSr) -c -o $@ $<

# OBJd
$(objdir)/%_d.o:  %.cpp  astyle.h  astyle_main.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSd) -c -o $@ $<

# OBJs
$(objdir)/%_s.o:  %.cpp  astyle.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSs) -c -o $@ $<

# OBJsd
$(objdir)/%_sd.o:  %.cpp  astyle.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSsd) -c -o $@ $<

# OBJa
$(objdir)/%_a.o:  %.cpp  astyle.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSa) -c -o $@ $<

# OBJad
$(objdir)/%_ad.o:  %.cpp  astyle.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSad) -c -o $@ $<

# OBJsj
$(objdir)/%_sj.o:  %.cpp  astyle.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSsj) -c -o $@ $<

# OBJsjd
$(objdir)/%_sjd.o:  %.cpp  astyle.h
	@ mkdir -p $(objdir)
	$(CXX) $(CFLAGSsjd) -c -o $@ $<

##################################################
# define build dependencies for each command

release:  astyle
astyle:  $(OBJ)
	@ mkdir -p $(bindir)
	$(CXX) $(LDFLAGSr) -o $(bindir)/$@ $^
	@ echo

debug:  astyled
astyled:  $(OBJd)
	@ mkdir -p $(bindir)
	$(CXX) $(LDFLAGSd) -o $(bindir)/$@ $^
	@ echo

shared:  libastyle.so
libastyle.so:  $(OBJs)
	@ mkdir -p $(bindir)
	$(CXX) -shared $(LDFLAGSr) -o $(bindir)/$@ $^
	@ echo

shareddebug:  libastyled.so
libastyled.so:  $(OBJsd)
	@ mkdir -p $(bindir)
	$(CXX) -shared $(LDFLAGSd) -o $(bindir)/$@ $^
	@ echo

static:  libastyle.a
libastyle.a:  $(OBJa)
	@ mkdir -p $(bindir)
	ar crs $(bindir)/$@ $^
	@ echo

staticdebug:  libastyled.a
libastyled.a:  $(OBJad)
	@ mkdir -p $(bindir)
	ar crs $(bindir)/$@ $^
	@ echo

java:  libastylej.so
libastylej.so:  $(OBJsj)
	@ mkdir -p $(bindir)
	$(CXX) -shared $(LDFLAGSr) -o $(bindir)/$@ $^
	@ echo

javadebug:  libastylejd.so
libastylejd.so:  $(OBJsjd)
	@ mkdir -p $(bindir)
	$(CXX) -shared $(LDFLAGSd) -o $(bindir)/$@ $^
	@ echo

all:  release debug shared shareddebug static staticdebug

javaall:  java javadebug

clean:
	rm -f $(objdir)/*.o $(bindir)/*astyle*

cleanobj:
	rm -f $(objdir)/*.o

install:
	$(INSTALL) -m 755 -d $(ipath)
	@$(INSTALL) -m 755 $(bindir)/astyle  $(ipath)

	$(INSTALL) -m 755 -d $(SYSCONF_PATH)
	@mkdir -p $(SYSCONF_PATH)/html;
	@for files in ../../doc/*.html  ../../doc/*.css; \
	do \
		$(INSTALL)  -m 644  $$files  $(SYSCONF_PATH)/html; \
	done
	@if [ -d $(SYSCONF_PATH_OLD) ];  then \
		rm -rf $(SYSCONF_PATH_OLD); \
	fi

uninstall:
	rm -f $(ipath)/astyle
	rm -rf $(SYSCONF_PATH)
	@if [ -d $(SYSCONF_PATH_OLD) ];  then \
		rm -rf $(SYSCONF_PATH_OLD); \
	fi
