DEVICE=atmega328p
PROGRAMMER=arduino
PORT=/dev/tty.usbmodem1101
BAUD=115200

CC=avr-gcc -Wall -D F_CPU=16000000UL -Os -mmcu=$(DEVICE)

all: temperature morsecode

helpers.o: Common/helpers.c
	$(CC) -c Common/helpers.c -o helpers.o

temperature: temperature.o helpers.o
	$(CC) -o temperature.elf temperature.o helpers.o
	avr-objcopy -O ihex -R .eeprom temperature.elf temperature.hex
	avr-size --format=avr --mcu=$(DEVICE) temperature.elf

temperature.o: temperature.c
	$(CC) -c temperature.c -o temperature.o

installtemp:
	avrdude -V -p $(DEVICE) -c $(PROGRAMMER) -P $(PORT) -b $(BAUD) -U flash:w:temperature.hex:i

morsecode: morsecode.o helpers.o
	$(CC) -o morsecode.elf morsecode.o helpers.o
	avr-objcopy -O ihex -R .eeprom morsecode.elf morsecode.hex
	avr-size --format=avr --mcu=$(DEVICE) morsecode.elf

morsecode.o: morsecode.c
	$(CC) -c morsecode.c -o morsecode.o

installmc:
	avrdude -V -p $(DEVICE) -c $(PROGRAMMER) -P $(PORT) -b $(BAUD) -U flash:w:morsecode.hex:i

serialmonitor:
	screen $(PORT) 9600

clean:
	rm *.o *.elf *.hex