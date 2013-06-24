###############################################################################
# Makefile for the project RetroAdapterV2
###############################################################################

## General Flags
PROJECT = RetroAdapterV2
MCU = atmega8
TARGET = RetroAdapterV2.elf
CC = avr-gcc

## Options common to compile, link and assembly rules
COMMON = -mmcu=$(MCU)
INCLUDES = -I./ -Iusbdrv/

## Compile options common for all C compilation units.
CFLAGS = $(COMMON)
CFLAGS += -Wall -std=gnu99 -DF_CPU=12000000UL -Os -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums -pipe
# Compiler fine-tuning for saving a few bytes.
CFLAGS += -fno-move-loop-invariants -fno-tree-scev-cprop

## Assembly specific flags
ASMFLAGS = $(COMMON)
ASMFLAGS += $(CFLAGS)
ASMFLAGS += -x assembler-with-cpp

## Linker flags
LDFLAGS = $(COMMON)
LDFLAGS +=  -Wl,-Map=RetroAdapterV2.map
# Replace CALL statements with RCALL where possible to save a few bytes.
LDFLAGS += -Wl,--relax

#LDFLAGS += $(GENTOO_LD_PATH_WORKAROUND)
#GENTOO_LD_PATH_WORKAROUND = -L/usr/i686-pc-linux-gnu/avr/lib
#GENTOO_LD_PATH_WORKAROUND = -L/usr/x86_64-pc-linux-gnu/avr/lib
#
# See:
# http://bugs.gentoo.org/show_bug.cgi?id=147155
# http://www.avrfreaks.net/index.php?name=PNphpBB2&file=viewtopic&p=336170
#
# ln -snf ../../lib/binutils/avr/2.*/ldscripts/ /usr/avr/lib/ldscripts


AVRDUDE = avrdude
AVRDUDE_PARAMS = -c usbasp
AVRDUDE_PARAMS += -p $(MCU)
AVRDUDE_PARAMS_FUSE = -U hfuse:w:0xC1:m -U lfuse:w:0x9F:m


## Intel Hex file production flags
HEX_FLASH_FLAGS = -R .eeprom

HEX_EEPROM_FLAGS = -j .eeprom
HEX_EEPROM_FLAGS += --set-section-flags=.eeprom="alloc,load"
HEX_EEPROM_FLAGS += --change-section-lma .eeprom=0 --no-change-warnings


## Objects that must be built in order to link
OBJECTS = usbdrvasm.o usbdrv.o 

## Build
all: $(TARGET) RetroAdapterV2.hex RetroAdapterV2.eep RetroAdapterV2.lss size

## Compile
usbdrvasm.o: usbdrv/usbdrvasm.S
	$(CC) $(INCLUDES) $(ASMFLAGS) -c -o $@ $<

usbdrv.o: usbdrv/usbdrv.c
	$(CC) $(INCLUDES) $(CFLAGS) -c -o $@ $<

amiga_m_asm.o: amiga_m_asm.S
	$(CC) $(INCLUDES) $(ASMFLAGS) -c -o $@ $<
dreamcast_asm.o: dreamcast_asm.S
	$(CC) $(INCLUDES) $(ASMFLAGS) -c -o $@ $<
n64gc_asm.o: n64gc_asm.S
	$(CC) $(INCLUDES) $(ASMFLAGS) -c -o $@ $<

##Compile and Link
# Compile all *.c files at once, allowing for better optimizations.
# Note: -combine has been removed in GCC 4.6, in favor of LTO
#       http://gcc.gnu.org/bugzilla/show_bug.cgi?id=29171#c7
#       http://gcc.gnu.org/wiki/LinkTimeOptimization
#
# Removed from the firmware because of compile error:
#   amiga_mouse.c analogue.c
# Removed from the firmware because I'm not interested in them:
#   3do.c atari_driving.c cd32.c pc-engine.c pc-fx.c sega_paddle.c
$(TARGET): $(OBJECTS) dreamcast_asm.o n64gc_asm.o RetroAdapterV2.c direct.c dreamcast.c dualdb9.c famicom.c n64gc.c psx.c RetroAdapterV2.c saturn.c timers.c
	 $(CC) $(CFLAGS) $(INCLUDES) $(LDFLAGS) \
		 -o $(TARGET) \
		 $^

%.hex: $(TARGET)
	avr-objcopy -O ihex $(HEX_FLASH_FLAGS)  $< $@

%.eep: $(TARGET)
	-avr-objcopy $(HEX_EEPROM_FLAGS) -O ihex $< $@ || exit 0

%.lss: $(TARGET)
	avr-objdump -h -S $< > $@

size: ${TARGET}
	avr-size --format=sysv --radix=10 $(TARGET)

size-detailed: ${TARGET}
	@avr-nm -f bsd -t d -A --size-sort $(TARGET) | \
		sed 's/^\([^:]\+\):\([0-9a-fA-F]\+\) \(.\) \(.\+\)$$/\2 \3 \4 [\1]/' | \
		sort -n


writeflash:
	$(AVRDUDE) $(AVRDUDE_PARAMS) \
		-U flash:w:RetroAdapterV2.hex:i

writefuse:
	$(AVRDUDE) $(AVRDUDE_PARAMS) \
		$(AVRDUDE_PARAMS_FUSE)

erase:
	$(AVRDUDE) $(AVRDUDE_PARAMS) \
		-e
dump:
	$(AVRDUDE) $(AVRDUDE_PARAMS) \
		-U flash:r:flash.dump:i \
		-U eeprom:r:eeprom.dump:i \
		-U hfuse:r:hfuse.dump:b \
		-U lfuse:r:lfuse.dump:b \
		-U lock:r:lock.dump:b \
		-U signature:r:signature.dump:h
# efuse is available on newer AVR microcontrollers
#		-U efuse:r:efuse.dump:b


## Clean target
.PHONY: all clean size writeflash writefuse erase dump
clean:
	-rm -rf $(OBJECTS) RetroAdapterV2.elf RetroAdapterV2.hex RetroAdapterV2.eep RetroAdapterV2.lss RetroAdapterV2.map
