PREFIX=/home/david/Projects/iphone/toolchain/toolchain/pre/bin/arm-apple-darwin9-
#BASE_DEV=/home/david/Projects/iphone/toolchain/sdks/iPhoneOS3.1.2.sdk
BASE_DEV=/home/david/Projects/iphone/toolchain/sdks/iPhoneOS2.0.sdk

TARGET = mame

# set this the operating system you're building for
# (actually you'll probably need your own main makefile anyways)
# MAMEOS = msdos
MAMEOS = iOS

# extension for executables
# EXE = .exe
EXE =

# CPU core include paths
VPATH=src $(wildcard src/cpu/*)

# compiler, linker and utilities
# compiler, linker and utilities
AR = /home/david/Projects/iphone/toolchain/toolchain/pre/bin/arm-apple-darwin9-ar
CC = /home/david/Projects/iphone/toolchain/toolchain/pre/bin/arm-apple-darwin9-gcc
CPP =/home/david/Projects/iphone/toolchain/toolchain/pre/bin/arm-apple-darwin9-gcc
LD =  /home/david/Projects/iphone/toolchain/toolchain/pre/bin/arm-apple-darwin9-gcc
ASM = /home/david/Projects/iphone/toolchain/toolchain/pre/bin/arm-apple-darwin9-gcc
MD = @mkdir
RM = @rm -f


#DEVLIBS = -LC:/DEVKITGP2X/sysroot/usr/lib/

EMULATOR = $(TARGET)$(EXE)

//DEFS = -DLSB_FIRST -DALIGN_INTS -DALIGN_SHORTS  -DINLINE="static __inline" 

DEFS = -DGP2X -DLSB_FIRST -DALIGN_INTS -DALIGN_SHORTS -DINLINE="static __inline" -Dasm="__asm__ __volatile__" -DMAME_UNDERCLOCK -DMAME_FASTSOUND -DENABLE_AUTOFIRE -DBIGCASE
# -DMAME_MEMINLINE

CFLAGS =
CFLAGS += -fsigned-char $(DEVLIBS) \
	-Isrc -Isrc/$(MAMEOS) -Isrc/zlib \
	-O3 -msoft-float -ffast-math -fomit-frame-pointer -fstrict-aliasing \
	-mstructure-size-boundary=32 -fexpensive-optimizations \
	-fweb -frename-registers -falign-functions=16 -falign-loops -falign-labels -falign-jumps \
	-finline -finline-functions -fno-common -fno-builtin -fsingle-precision-constant \
	-Wall -Wno-sign-compare -Wunused -Wpointer-arith -Wcast-align -Waggregate-return -Wshadow


CFLAGS += -Isrc -Isrc/$(MAMEOS) -Isrc/zlib -IClasses/btstack
CFLAGS += -O3 -ffast-math 
CFLAGS += -I/home/david/Projects/iphone/toolchain/sdks/iPhoneOS3.1.2.sdk/usr/include
	
#CFLAGS	= -I./ -I./Classes/  \
          -I/home/david/Projects/iphone/toolchain/sdks/iPhoneOS3.1.2.sdk/usr/include \
          -march=armv6 -DARM_ARCH -DGP2X_BUILD  -O3 -ffast-math -ftemplate-depth-36 -mstructure-size-boundary=32 -falign-functions=32 \
          -falign-loops -falign-labels -falign-jumps -finline -finline-functions -fno-builtin -fno-common -fomit-frame-pointer \
          -Isrc/ -Isrc/$(MAMEOS) -Isrc/zlib
          	
CFLAGS += -I./Classes/
CFLAGS += -F/home/david/Projects/iphone/toolchain/sdks/iPhoneOS3.1.2.sdk/System/Library/PrivateFrameworks 

LDFLAGS +=  $(CFLAGS) -static -s
#LDFLAGS = $(CFLAGS) -s -static
LDFLAGS = -lobjc -lpthread -bind_at_load 

LDFLAGS += -L/home/david/Projects/iphone/toolchain/apps/mame4iphone/lib
LDFLAGS += -lBTstack

LDFLAGS += -framework Foundation -framework CoreFoundation -framework CoreSurface -framework UIKit -framework QuartzCore -framework CoreGraphics -framework AudioToolbox    
LDFLAGS += -lm -lc  -lz 
LDFLAGS += -F$(BASE_DEV)/System/Library/Frameworks
LDFLAGS += -F$(BASE_DEV)/System/Library/PrivateFrameworks
LDFLAGS += -L$(BASE_DEV)/usr/lib 

LIBS = -lm -lpthread 

OBJ = obj_$(TARGET)_$(MAMEOS)
OBJDIRS = $(OBJ) $(OBJ)/cpu $(OBJ)/sound $(OBJ)/$(MAMEOS) \
	$(OBJ)/drivers $(OBJ)/machine $(OBJ)/vidhrdw $(OBJ)/sndhrdw \
	$(OBJ)/zlib $(OBJ)/Classes

all:	maketree $(EMULATOR)

# include the various .mak files
include src/core.mak
include src/$(TARGET).mak
include src/rules.mak
include src/sound.mak
include src/$(MAMEOS)/$(MAMEOS).mak
include Classes/objc.mak

# combine the various definitions to one
CDEFS = $(DEFS) $(COREDEFS) $(CPUDEFS) $(SOUNDDEFS)

$(EMULATOR): $(OBJC) $(COREOBJS) $(OSOBJS) $(DRVOBJS)
	$(LD) $(LDFLAGS) $(OBJC) $(COREOBJS) $(OSOBJS) $(LIBS) $(DRVOBJS) -o $@

$(OBJ)/%.o: src/%.c
	@echo Compiling $<...
	$(CC) $(CDEFS) $(CFLAGS) -c $< -o $@

$(OBJ)/%.o: src/%.cpp
	@echo Compiling $<...
	$(CPP) $(CDEFS) $(CFLAGS) -fno-rtti -c $< -o $@
	
$(OBJ)/%.o: %.m
	@echo Compiling $<...
	$(CC) $(CDEFS) $(CFLAGS) -c $< -o $@	

$(OBJ)/%.o: src/%.s
	@echo Compiling $<...
	$(CPP) $(CDEFS) $(CFLAGS) -c $< -o $@

$(OBJ)/%.o: src/%.S
	@echo Compiling $<...
	$(CPP) $(CDEFS) $(CFLAGS) -c $< -o $@

$(sort $(OBJDIRS)):
	$(MD) $@

maketree: $(sort $(OBJDIRS))

clean:
	$(RM) -r $(OBJ)
	$(RM) $(EMULATOR)
