PRG            = ReTest
MCU_TARGET     = atmega328p
OPTIMIZE       = -O2
DEFS           = -D F_CPU=16000000
LIBS           =

CC             = avr-gcc
OBJCOPY        = avr-objcopy
OBJDUMP        = avr-objdump

# Override is only needed by avr-lib build system.
override CFLAGS        = -g -Wall $(OPTIMIZE) -mmcu=$(MCU_TARGET) $(DEFS)
override LDFLAGS       = -Wl,-Map,$(PRG).map
all: $(PRG).elf lst text eeprom
$(PRG).elf: $(PRG).o spi.o lcd.o rotaryenc.o
	$(CC) $(CFLAGS) $(LDFLAGS) -o $@ $^ $(LIBS)

# dependencies:
$(PRG).o: $(PRG).c
spi.o: spi.c spi.h
lcd.o: lcd.c lcd.h
rotaryenc.o: rotaryenc.c rotaryenc.h

clean:
	rm -rf *.o *.elf *.eps *.png *.pdf *.bak 
	rm -rf *.lst *.map *.hex *.bin *.srec *~

lst:  $(PRG).lst

%.lst: %.elf
	$(OBJDUMP) -h -S $< > $@

# Rules for building the .text rom images
text: hex bin srec
hex:  $(PRG).hex
bin:  $(PRG).bin
srec: $(PRG).srec

%.hex: %.elf
	$(OBJCOPY) -j .text -j .data -O ihex $< $@

%.srec: %.elf
	$(OBJCOPY) -j .text -j .data -O srec $< $@

%.bin: %.elf
	$(OBJCOPY) -j .text -j .data -O binary $< $@

# Rules for building the .eeprom rom images
eeprom: ehex ebin esrec
ehex:  $(PRG)_eeprom.hex
ebin:  $(PRG)_eeprom.bin
esrec: $(PRG)_eeprom.srec

%_eeprom.hex: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O ihex $< $@ 

%_eeprom.srec: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O srec $< $@

%_eeprom.bin: %.elf
	$(OBJCOPY) -j .eeprom --change-section-lma .eeprom=0 -O binary $< $@ 
