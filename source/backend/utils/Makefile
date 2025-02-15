#!/usr/bin/make -f
# Makefile for carla-plugin #
# ------------------------- #
# Created by falkTX
#

CWD=../..
include ../Makefile.mk

# ---------------------------------------------------------------------------------------------------------------------

BUILD_CXX_FLAGS += $(FLUIDSYNTH_FLAGS)
BUILD_CXX_FLAGS += $(MAGIC_LIBS)
BUILD_CXX_FLAGS += $(YSFX_FLAGS)

# ---------------------------------------------------------------------------------------------------------------------

OBJS = \
	$(OBJDIR)/CachedPlugins.cpp.o \
	$(OBJDIR)/CarlaUtils.cpp.o \
	$(OBJDIR)/Information.cpp.o \
	$(OBJDIR)/JUCE.cpp.o \
	$(OBJDIR)/PipeClient.cpp.o \
	$(OBJDIR)/PluginDiscovery.cpp.o \
	$(OBJDIR)/System.cpp.o \
	$(OBJDIR)/Windows.cpp.o

TARGETS = $(BINDIR)/libcarla_utils$(LIB_EXT)

# ---------------------------------------------------------------------------------------------------------------------

LIBS  = $(MODULEDIR)/jackbridge.a
LIBS += $(MODULEDIR)/lilv.a
LIBS += $(MODULEDIR)/water.files.a

ifeq ($(HAVE_YSFX),true)
LIBS += $(MODULEDIR)/ysfx.a
endif

ifeq ($(USING_JUCE),true)
LIBS += $(MODULEDIR)/carla_juce.a
LIBS += $(MODULEDIR)/juce_audio_basics.a
LIBS += $(MODULEDIR)/juce_audio_processors.a
LIBS += $(MODULEDIR)/juce_core.a
LIBS += $(MODULEDIR)/juce_data_structures.a
LIBS += $(MODULEDIR)/juce_events.a
LIBS += $(MODULEDIR)/juce_graphics.a
LIBS += $(MODULEDIR)/juce_gui_basics.a
LIBS += $(MODULEDIR)/juce_gui_extra.a
endif

LINK_FLAGS += $(JACKBRIDGE_LIBS)
LINK_FLAGS += $(LILV_LIBS)
LINK_FLAGS += $(WATER_LIBS)

ifeq ($(HAVE_X11),true)
LINK_FLAGS += $(X11_LIBS)
endif

ifeq ($(HAVE_YSFX),true)
LINK_FLAGS += $(YSFX_GRAPHICS_LIBS)
endif

ifneq ($(HAIKU),true)
ifneq ($(WASM),true)
LINK_FLAGS += -pthread
endif
endif

ifeq ($(USING_JUCE),true)
LINK_FLAGS += $(JUCE_AUDIO_BASICS_LIBS)
LINK_FLAGS += $(JUCE_AUDIO_PROCESSORS_LIBS)
LINK_FLAGS += $(JUCE_CORE_LIBS)
LINK_FLAGS += $(JUCE_DATA_STRUCTURES_LIBS)
LINK_FLAGS += $(JUCE_EVENTS_LIBS)
LINK_FLAGS += $(JUCE_GRAPHICS_LIBS)
LINK_FLAGS += $(JUCE_GUI_BASICS_LIBS)
LINK_FLAGS += $(JUCE_GUI_EXTRA_LIBS)
endif

# ---------------------------------------------------------------------------------------------------------------------

all: $(TARGETS)

# ---------------------------------------------------------------------------------------------------------------------

clean:
	rm -f $(OBJS) $(TARGETS)

debug:
	$(MAKE) DEBUG=true

# ---------------------------------------------------------------------------------------------------------------------

$(BINDIR)/libcarla_utils$(LIB_EXT): $(OBJS) $(LIBS)
	-@mkdir -p $(BINDIR)
	@echo "Linking libcarla_utils$(LIB_EXT)"
	$(SILENT)$(CXX) $(OBJS) $(BUILD_CXX_FLAGS) $(LIBS_START) $(LIBS) $(LIBS_END) $(LINK_FLAGS) $(LINK_FLAGS) $(SHARED) -o $@

# ---------------------------------------------------------------------------------------------------------------------

ifeq ($(MACOS),true)
ifeq ($(USING_JUCE),true)
$(OBJDIR)/CachedPlugins.cpp.o: BUILD_CXX_FLAGS += -std=gnu++14
endif
$(OBJDIR)/CarlaUtils.cpp.o: BUILD_CXX_FLAGS += -ObjC++

$(OBJDIR)/Windows.cpp.o: BUILD_CXX_FLAGS += -ObjC++
endif

$(OBJDIR)/%.cpp.o: %.cpp
	-@mkdir -p $(OBJDIR)
	@echo "Compiling $<"
	$(SILENT)$(CXX) $< $(BUILD_CXX_FLAGS) -c -o $@

-include $(OBJS:%.o=%.d)

# ---------------------------------------------------------------------------------------------------------------------
