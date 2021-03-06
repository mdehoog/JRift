#############################################################################
#
# Filename    : Makefile
# Content     : Makefile for building linux version of: libovr
# Created     : 2013
# Authors     : Simon Hallam and Peter Giokaris
# Copyright   : Copyright 2013 OculusVR, Inc. All Rights Reserved
# Instruction : The g++ compiler and stdndard lib packages need to be 
#               installed on the system.  Navigate in a shell to the 
#               directory where this Makefile is located and enter:
#
#               make                builds the release version for the 
#                                   current architechture
#               make clean          delete intermediate release object files 
#                                   and the library file
#               make DEBUG=1        builds the debug version for the current
#                                   architechture
#               make clean DEBUG=1  deletes intermediate debug object files 
#                                   and the library file
#
# Output      : Relative to the directory this Makefile lives in, libraries
#               are built at the following locations depending upon the
#               architechture of the system you are running:
#
#               ./Lib/Linux/Debug/i386/libovr.a
#               ./Lib/Linux/Debug/x86_64/libovr.a
#               ./Lib/Linux/Release/i386/libovr.a
#               ./Lib/Linux/Release/x86_64/libovr.a
#
#############################################################################

UNAME:= $(shell uname)

ifeq ($(shell uname), Darwin)

LIBDIR:=OSX

ADDOBJS :=  \
		$(OBJPATH)/OVR_OSX_HIDDevice.o \
		$(OBJPATH)/OVR_OSX_SensorDevice.o \
		$(OBJPATH)/OVR_OSX_DeviceManager.o \
		$(OBJPATH)/OVR_OSX_HMDDevice.o
else

LIBDIR=Linux
ADDOBJS :=  \
		$(OBJPATH)/OVR_Linux_HIDDevice.o \
		$(OBJPATH)/OVR_Linux_SensorDevice.o \
		$(OBJPATH)/OVR_Linux_DeviceManager.o \
		$(OBJPATH)/OVR_Linux_HMDDevice.o
endif

####### Compiler, tools and options

CXX           = g++
LINK          = ar rvs
DELETEFILE    = rm -f

####### Detect debug or release

DEBUG         = 0
ifeq ($(DEBUG), 1)
	CXXFLAGS      = -pipe -DDEBUG -g -fPIC
	RELEASETYPE   = Debug
else
	CXXFLAGS      = -pipe -O2 -fPIC
	RELEASETYPE   = Release
endif

####### Detect system architecture

SYSARCH       = x86_64
ifeq ($(shell uname -m),i686)
SYSARCH       = i386
CXXFLAGS += -m32
endif
#SYSARCH       = i386
#CXXFLAGS += -m32
####### Paths

LIBOVRPATH    = .
3RDPARTYPATH  = ../3rdParty
INCPATH       = -I. -I.. -I$(LIBOVRPATH)/Include -I$(LIBOVRPATH)/Src
OBJPATH       = ./Obj/$(LIBDIR)/$(RELEASETYPE)/$(SYSARCH)

####### Files

TARGET        = ./Lib/$(LIBDIR)/$(RELEASETYPE)/$(SYSARCH)/libovr.a

OBJECTS       = $(OBJPATH)/OVR_DeviceHandle.o \
		$(OBJPATH)/OVR_DeviceImpl.o \
		$(OBJPATH)/OVR_JSON.o \
		$(OBJPATH)/OVR_LatencyTestImpl.o \
		$(OBJPATH)/OVR_Profile.o \
		$(OBJPATH)/OVR_SensorFilter.o\
		$(OBJPATH)/OVR_SensorFusion.o\
		$(OBJPATH)/OVR_SensorImpl.o \
		$(OBJPATH)/OVR_ThreadCommandQueue.o \
		$(OBJPATH)/OVR_Alg.o \
		$(OBJPATH)/OVR_Allocator.o \
		$(OBJPATH)/OVR_Atomic.o \
		$(OBJPATH)/OVR_File.o \
		$(OBJPATH)/OVR_FileFILE.o \
		$(OBJPATH)/OVR_Log.o \
		$(OBJPATH)/OVR_Math.o \
		$(OBJPATH)/OVR_RefCount.o \
		$(OBJPATH)/OVR_Std.o \
		$(OBJPATH)/OVR_String.o \
		$(OBJPATH)/OVR_String_FormatUtil.o \
		$(OBJPATH)/OVR_String_PathUtil.o \
		$(OBJPATH)/OVR_SysFile.o \
		$(OBJPATH)/OVR_System.o \
		$(OBJPATH)/OVR_Timer.o \
		$(OBJPATH)/OVR_UTF8Util.o \
		$(OBJPATH)/Util_LatencyTest.o \
		$(OBJPATH)/Util_MagCalibration.o \
		$(OBJPATH)/Util_Render_Stereo.o \
		$(OBJPATH)/OVR_ThreadsPthread.o \
		$(OBJPATH)/tinyxml2.o

ifeq ($(shell uname), Darwin)

LIBDIR:=OSX

OBJECTS+=  \
		$(OBJPATH)/OVR_OSX_HIDDevice.o \
		$(OBJPATH)/OVR_OSX_SensorDevice.o \
		$(OBJPATH)/OVR_OSX_DeviceManager.o \
		$(OBJPATH)/OVR_OSX_HMDDevice.o
else

LIBDIR=Linux
OBJECTS+=  \
		$(OBJPATH)/OVR_Linux_HIDDevice.o \
		$(OBJPATH)/OVR_Linux_SensorDevice.o \
		$(OBJPATH)/OVR_Linux_DeviceManager.o \
		$(OBJPATH)/OVR_Linux_HMDDevice.o
endif
####### Rules

all: $(OBJPATH)   $(TARGET)

$(TARGET):  $(OBJECTS)
	mkdir -p $(dir $(TARGET))
	$(LINK) $(TARGET) $(OBJECTS)

$(OBJPATH):
	mkdir -p $(OBJPATH)

$(OBJPATH)/%.o: $(LIBOVRPATH)/Src/Util/%.cpp 
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<
$(OBJPATH)/%.o: $(LIBOVRPATH)/Src/Kernel/%.cpp 
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<
$(OBJPATH)/%.o: $(LIBOVRPATH)/Src/%.cpp 
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<

$(OBJPATH)/tinyxml2.o: $(3RDPARTYPATH)/TinyXml/tinyxml2.cpp 
	$(CXX) -c $(CXXFLAGS) $(INCPATH) -o $@ $<

clean:
	-$(DELETEFILE) $(OBJECTS)
	-$(DELETEFILE) $(TARGET)

