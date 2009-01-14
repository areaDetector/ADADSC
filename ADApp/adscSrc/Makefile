TOP=../..
include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS AFTER THIS LINE
#=============================

# Set to YES or NO to use or not use simadsc simulation library instead of
# real ADSC detcon_lib_th library

USE_SIMADSC = NO

# simadsc location (only used if USE_SIMADSC is YES)

SIMADSC_HOME = /opt/simadsc

# ADSC control software location

ADSC_HOME = /opt/adscdc

ifeq (YES, $(USE_SIMADSC))
  USR_CXXFLAGS += -DUSE_SIMADSC
endif

# The required ADSC header files are detcon_entry.h, detcon_par.h, and
# detcon_state.h.  USR_INCLUDES should include the directories containing
# these header files.

USR_INCLUDES += -I$(ADSC_HOME)/src_std/incl
USR_INCLUDES += -I$(ADSC_HOME)/src_std/detcon_lib_th

ifeq (YES, $(USE_SIMADSC))
  USR_INCLUDES += -I$(SIMADSC_HOME)/include
endif

# The required ADSC library files are libauxlib.a and libdetcon_th.a.  A
# corresponding <name>_DIR definition is required for each of these and should
# be set to the directory containing the library.

ifeq (YES, $(USE_SIMADSC))
  simadsc_DIR = $(SIMADSC_HOME)/lib
else
  ifeq (cygwin32, $(OS_CLASS))
    auxlib_DIR    = $(ADSC_HOME)/lib/cygwin
    detcon_th_DIR = $(ADSC_HOME)/lib/cygwin
  endif
  ifeq (Darwin, $(OS_CLASS))
    auxlib_DIR    = $(ADSC_HOME)/lib/darwin
    detcon_th_DIR = $(ADSC_HOME)/lib/darwin
  endif
  ifeq (Linux, $(OS_CLASS))
    auxlib_DIR    = $(ADSC_HOME)/lib/linux
    detcon_th_DIR = $(ADSC_HOME)/lib/linux
  endif
  ifeq (solaris, $(OS_CLASS))
    auxlib_DIR    = $(ADSC_HOME)/lib/sun
    detcon_th_DIR = $(ADSC_HOME)/lib/sun
  endif
endif

LIBRARY_IOC_cygwin32 += adsc
LIBRARY_IOC_Darwin   += adsc
LIBRARY_IOC_Linux    += adsc
LIBRARY_IOC_solaris  += adsc

LIB_SRCS += adsc.cpp
LIB_SRCS += drvAdscEpics.c
LIB_LIBS += NDPlugin ADBase
ifeq (YES, $(USE_SIMADSC))
  LIB_LIBS += simadsc
else
  LIB_LIBS += detcon_th auxlib
endif
LIB_LIBS += sscan autosave
LIB_LIBS += asyn
LIB_LIBS += $(EPICS_BASE_IOC_LIBS)

#==================================
PROD_IOC_cygwin32 += adscApp
PROD_IOC_Darwin   += adscApp
PROD_IOC_Linux    += adscApp
PROD_IOC_solaris  += adscApp
# <name>.dbd will be created from <name>Include.dbd
DBD += adscApp.dbd
DBD += adscSupport.dbd

# <name>_registerRecordDeviceDriver.cpp will be created from <name>.dbd
adscApp_SRCS_DEFAULT += adscApp_registerRecordDeviceDriver.cpp adscAppMain.cpp
adscApp_SRCS_vxWorks += adscApp_registerRecordDeviceDriver.cpp

# Add locally compiled object code
adscApp_LIBS += adsc
adscApp_LIBS += NDPlugin ADBase
ifeq (YES, $(USE_SIMADSC))
  adscApp_LIBS += simadsc
else
  adscApp_LIBS += detcon_th auxlib
endif
adscApp_LIBS += sscan autosave 
adscApp_LIBS += asyn
adscApp_LIBS += $(EPICS_BASE_IOC_LIBS)

#=============================

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

