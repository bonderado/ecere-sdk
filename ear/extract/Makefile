ifneq ($(V),1)
.SILENT:
endif

.PHONY: all objdir cleantarget clean realclean wipeclean distclean

# CORE VARIABLES

MODULE := extract
CONFIG := release
ifndef COMPILER
COMPILER := default
endif

TARGET_TYPE = executable

# FLAGS

ECFLAGS =
ifndef DEBIAN_PACKAGE
CFLAGS =
LDFLAGS =
endif
PRJ_CFLAGS =
CECFLAGS =
OFLAGS =
LIBS =

ifdef DEBUG
NOSTRIP := y
endif

CONSOLE = -mwindows

# INCLUDES

_CF_DIR = ../../
USE_BOOTSTRAP := defined

include $(_CF_DIR)crossplatform.mk
include $(_CF_DIR)default.cf

# POST-INCLUDES VARIABLES

OBJ = obj/$(CONFIG).$(PLATFORM)$(COMPILER_SUFFIX)$(DEBUG_SUFFIX)/

RES = 

TARGET = obj/$(CONFIG).$(PLATFORM)$(COMPILER_SUFFIX)$(DEBUG_SUFFIX)/extract$(E)

_ECSOURCES = \
	extract.ec

ECSOURCES = $(call shwspace,$(_ECSOURCES))

COBJECTS = $(call shwspace,$(addprefix $(OBJ),$(patsubst %.ec,%$(C),$(notdir $(_ECSOURCES)))))

SYMBOLS = $(call shwspace,$(addprefix $(OBJ),$(patsubst %.ec,%$(S),$(notdir $(_ECSOURCES)))))

IMPORTS = $(call shwspace,$(addprefix $(OBJ),$(patsubst %.ec,%$(I),$(notdir $(_ECSOURCES)))))

ECOBJECTS = $(call shwspace,$(addprefix $(OBJ),$(patsubst %.ec,%$(O),$(notdir $(_ECSOURCES)))))

BOWLS = $(call shwspace,$(addprefix $(OBJ),$(patsubst %.ec,%$(B),$(notdir $(_ECSOURCES)))))

OBJECTS = $(ECOBJECTS) $(OBJ)$(MODULE).main$(O)

SOURCES = $(ECSOURCES)

RESOURCES = $(RESOURCES1) $(RESOURCES2)
RESOURCES1 = \
	../../ecere/res/vanilla/ecere/actions/folderNew.png \
	../../ecere/res/vanilla/ecere/actions/goUp.png \
	../../ecere/res/vanilla/ecere/devices/computer.png \
	../../ecere/res/vanilla/ecere/devices/driveHardDisk.png \
	../../ecere/res/vanilla/ecere/devices/driveRemovableMedia.png \
	../../ecere/res/vanilla/ecere/devices/mediaFloppy.png \
	../../ecere/res/vanilla/ecere/devices/mediaOptical.png \
	../../ecere/res/vanilla/ecere/elements/areaClose.png \
	../../ecere/res/vanilla/ecere/elements/areaMaximize.png \
	../../ecere/res/vanilla/ecere/elements/areaMinimize.png \
	../../ecere/res/vanilla/ecere/elements/areaRestore.png \
	../../ecere/res/vanilla/ecere/elements/arrowDown.png \
	../../ecere/res/vanilla/ecere/elements/arrowLeft.png \
	../../ecere/res/vanilla/ecere/elements/arrowRight.png \
	../../ecere/res/vanilla/ecere/elements/arrowUp.png \
	../../ecere/res/vanilla/ecere/elements/optionBoxDisabledSelected.png \
	../../ecere/res/vanilla/ecere/elements/optionBoxDown.png \
	../../ecere/res/vanilla/ecere/elements/optionBoxSelected.png \
	../../ecere/res/vanilla/ecere/elements/optionBoxSelectedDown.png \
	../../ecere/res/vanilla/ecere/elements/optionBoxUp.png \
	../../ecere/res/vanilla/ecere/places/driveRemote.png \
	../../ecere/res/vanilla/ecere/places/folder.png \
	../../ecere/res/vanilla/ecere/places/folderRemote.png \
	../../ecere/res/vanilla/ecere/places/networkServer.png \
	../../ecere/res/vanilla/ecere/places/networkWorkgroup.png \
	../../ecere/res/vanilla/ecere/status/folderOpen.png \
	../../ecere/locale/es.mo
RESOURCES2 = \
	../../ecere/locale/hu.mo \
	../../ecere/locale/mr.mo \
	../../ecere/locale/nl.mo \
	../../ecere/locale/pt_BR.mo \
	../../ecere/locale/ru.mo \
	../../ecere/locale/zh_CN.mo \
	locale/es.mo \
	locale/he.mo \
	locale/ru.mo \
	locale/zh_CN.mo

LIBS += $(SHAREDLIB) $(EXECUTABLE) $(LINKOPT)

ifndef STATIC_LIBRARY_TARGET
LIBS += \
	$(call _L,ecereVanilla) \
	$(call _L,z)
endif

PRJ_CFLAGS += \
	 $(if $(DEBUG), -g, -Os) $(FPIC) -w \
			 -DECERE_STATIC

ECFLAGS += -module $(MODULE)
ECFLAGS += \
	 -nolinenumbers

# PLATFORM-SPECIFIC OPTIONS

ifdef WINDOWS_TARGET

ifndef STATIC_LIBRARY_TARGET
LIBS += \
	$(call _L,mpr) \
	$(call _L,winmm) \
	$(call _L,imm32) \
	$(call _L,gdi32)
endif

else
ifdef LINUX_TARGET

ifndef STATIC_LIBRARY_TARGET
OFLAGS += \
	 -L/usr/X11R6/lib
LIBS += \
	$(call _L,fontconfig) \
	$(call _L,freetype) \
	$(call _L,Xrender) \
	$(call _L,Xext) \
	$(call _L,ncurses) \
	$(call _L,m) \
	$(call _L,dl) \
	$(call _L,pthread) \
	$(call _L,X11)
endif

else
ifdef OSX_TARGET

ifndef STATIC_LIBRARY_TARGET
OFLAGS += \
	 -L/usr/X11R6/lib
LIBS += \
	$(call _L,fontconfig) \
	$(call _L,freetype) \
	$(call _L,Xrender) \
	$(call _L,Xext) \
	$(call _L,ncurses) \
	$(call _L,m) \
	$(call _L,dl) \
	$(call _L,pthread) \
	$(call _L,X11)
endif

endif
endif
endif

CECFLAGS += -cpp $(_CPP)

ifndef STATIC_LIBRARY_TARGET
OFLAGS += \
	 -L../../ecere/obj/vanilla.$(PLATFORM)$(COMPILER_SUFFIX)$(DEBUG_SUFFIX) \
	 -L../../deps/zlib-1.2.8/obj/release.$(PLATFORM)$(COMPILER_SUFFIX)
endif

# TARGETS

all: objdir $(TARGET)

objdir:
	$(call mkdir,$(OBJ))

$(OBJ)$(MODULE).main.ec: $(SYMBOLS) $(COBJECTS)
	$(ECS) $(ARCH_FLAGS) $(ECSLIBOPT) $(SYMBOLS) $(IMPORTS) -symbols obj/$(CONFIG).$(PLATFORM)$(COMPILER_SUFFIX)$(DEBUG_SUFFIX) -o $(OBJ)$(MODULE).main.ec

$(OBJ)$(MODULE).main.c: $(OBJ)$(MODULE).main.ec
	$(ECP) $(CFLAGS) $(CECFLAGS) $(ECFLAGS) $(PRJ_CFLAGS) -c $(OBJ)$(MODULE).main.ec -o $(OBJ)$(MODULE).main.sym -symbols $(OBJ)
	$(ECC) $(CFLAGS) $(CECFLAGS) $(ECFLAGS) $(PRJ_CFLAGS) $(FVISIBILITY) -c $(OBJ)$(MODULE).main.ec -o $(OBJ)$(MODULE).main.c -symbols $(OBJ)

$(SYMBOLS): | objdir
$(OBJECTS): | objdir
$(TARGET): $(SOURCES) $(RESOURCES) $(SYMBOLS) $(OBJECTS) | objdir
ifndef STATIC_LIBRARY_TARGET
	$(CC) $(OFLAGS) $(OBJECTS) $(LIBS) -o $(TARGET) $(INSTALLNAME)
ifndef NOSTRIP
	$(STRIP) $(STRIPOPT) $(TARGET)
endif
ifndef WINDOWS_TARGET
ifdef EXECUTABLE_TARGET
  ifndef DISABLE_EXEC_COMPRESSION
	@-$(call sys_path,$(UPX) $(UPXFLAGS) $(TARGET)) || $(call echo,upx not installed; not compressing.)
  endif
endif
else
  ifndef DISABLE_EXEC_COMPRESSION
	@-$(call sys_path,$(UPX) $(UPXFLAGS) $(TARGET)) || $(call echo,upx not installed; not compressing.)
  endif
endif
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/res/vanilla/ecere/actions/folderNew.png ../../ecere/res/vanilla/ecere/actions/goUp.png "ecere/actions"
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/res/vanilla/ecere/devices/computer.png ../../ecere/res/vanilla/ecere/devices/driveHardDisk.png ../../ecere/res/vanilla/ecere/devices/driveRemovableMedia.png ../../ecere/res/vanilla/ecere/devices/mediaFloppy.png ../../ecere/res/vanilla/ecere/devices/mediaOptical.png "ecere/devices"
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/res/vanilla/ecere/elements/areaClose.png ../../ecere/res/vanilla/ecere/elements/areaMaximize.png ../../ecere/res/vanilla/ecere/elements/areaMinimize.png ../../ecere/res/vanilla/ecere/elements/areaRestore.png ../../ecere/res/vanilla/ecere/elements/arrowDown.png ../../ecere/res/vanilla/ecere/elements/arrowLeft.png ../../ecere/res/vanilla/ecere/elements/arrowRight.png ../../ecere/res/vanilla/ecere/elements/arrowUp.png ../../ecere/res/vanilla/ecere/elements/optionBoxDisabledSelected.png ../../ecere/res/vanilla/ecere/elements/optionBoxDown.png "ecere/elements"
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/res/vanilla/ecere/elements/optionBoxSelected.png ../../ecere/res/vanilla/ecere/elements/optionBoxSelectedDown.png ../../ecere/res/vanilla/ecere/elements/optionBoxUp.png "ecere/elements"
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/res/vanilla/ecere/places/driveRemote.png ../../ecere/res/vanilla/ecere/places/folder.png ../../ecere/res/vanilla/ecere/places/folderRemote.png ../../ecere/res/vanilla/ecere/places/networkServer.png ../../ecere/res/vanilla/ecere/places/networkWorkgroup.png "ecere/places"
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/res/vanilla/ecere/status/folderOpen.png "ecere/status"
	$(EAR) aw$(EARFLAGS) $(TARGET) ../../ecere/locale/es.mo ../../ecere/locale/hu.mo ../../ecere/locale/mr.mo ../../ecere/locale/nl.mo ../../ecere/locale/pt_BR.mo ../../ecere/locale/ru.mo ../../ecere/locale/zh_CN.mo "ecere/locale"
	$(EAR) aw$(EARFLAGS) $(TARGET) locale/es.mo locale/he.mo locale/ru.mo locale/zh_CN.mo "locale"
else
	$(AR) rcs $(TARGET) $(OBJECTS) $(LIBS)
endif
ifdef DEBIAN_PACKAGE
	touch -r extract.ec $(OBJ)extract.c $(OBJ)$(MODULE).main.ec $(OBJ)$(MODULE).main.c $(OBJECTS) $(SYMBOLS) $(TARGET)
endif

# SYMBOL RULES

$(OBJ)extract.sym: extract.ec
	$(ECP) $(CFLAGS) $(CECFLAGS) $(ECFLAGS) $(PRJ_CFLAGS) -c extract.ec -o $(OBJ)extract.sym

# C OBJECT RULES

$(OBJ)extract.c: extract.ec $(OBJ)extract.sym | $(SYMBOLS)
	$(ECC) $(CFLAGS) $(CECFLAGS) $(ECFLAGS) $(PRJ_CFLAGS) $(FVISIBILITY) -c extract.ec -o $(OBJ)extract.c -symbols $(OBJ)

# OBJECT RULES

$(OBJ)extract.o: $(OBJ)extract.c
	$(CC) $(CFLAGS) $(PRJ_CFLAGS) $(FVISIBILITY) -c $(OBJ)extract.c -o $(OBJ)extract.o

$(OBJ)$(MODULE).main$(O): $(OBJ)$(MODULE).main.c
	$(CC) $(CFLAGS) $(PRJ_CFLAGS) $(FVISIBILITY) -c $(OBJ)$(MODULE).main.c -o $(OBJ)$(MODULE).main$(O)

cleantarget:
	$(call rm,$(OBJ)$(MODULE).main.o $(OBJ)$(MODULE).main.c $(OBJ)$(MODULE).main.ec $(OBJ)$(MODULE).main$(I) $(OBJ)$(MODULE).main$(S))
	$(call rm,$(OBJ)symbols.lst)
	$(call rm,$(OBJ)objects.lst)
	$(call rm,$(TARGET))
ifdef SHARED_LIBRARY_TARGET
ifdef LINUX_TARGET
ifdef LINUX_HOST
	$(call rm,$(OBJ)$(LP)$(MODULE)$(SO)$(basename $(VER)))
	$(call rm,$(OBJ)$(LP)$(MODULE)$(SO))
endif
endif
endif

clean: cleantarget
	$(call rm,$(_OBJECTS))
	$(call rm,$(ECOBJECTS))
	$(call rm,$(COBJECTS))
	$(call rm,$(BOWLS))
	$(call rm,$(IMPORTS))
	$(call rm,$(SYMBOLS))

wipeclean:
	$(call rmr,obj/)

realclean: cleantarget
	$(call rmr,$(OBJ))

distclean:
	$(_MAKE) -f $(_CF_DIR)Cleanfile distclean distclean_all_subdirs

$(MAKEFILE_LIST): ;
$(SOURCES): ;
$(RESOURCES): ;
