arduino-tiny-841
============

A fork of shimniok's ( github.com/shimniok ) fork of arduino-tiny, which made an attempt to support the Tiny841. All work appears to have stopped on that core, and it was never in a state where sketches could be compiled (it looks like the initial work was never completed). 

This fork aims to finish what he started and add working support for the ATtiny841 on Arduino. 

If you installed this core prior to 3/14/2015, you must replace the previous entry in avrdude.conf with the new one. The efuse was not being read correctly. 

Status
===========

* Optiboot bootloader included
* Serial and Serial 1 work. 
* INPUT_PULLUP works
* millis and micros work
* analogRead() works
* PWM works on all 6 channels. 
* EEPROM works.
* tone is untested. 
* SPI is untested - but should work. Registers are identical to the mega328. 
* I2C/TWI hardware slave needs library support. It is not the same as the master/slave TWI on mega's, and the 841 does not have a USI, so the USI I2C libraries that exist for the tinyx4/x5 won't work either. I can't make SoftI2CMaster work.. 
* Pin change interrupts are untested (including INT0).
* USBAsp can't program it :-(


Hardware
============

For use with Optiboot, the following components and connections are required:
* Arduino pin 9/PA1/TXD0 to RXI of serial adapter
* Arduino pin 8/PA2/RXD0 to TXO of serial adapter
* Diode between Reset and Vcc (band towards Vcc)
* 0.1uf capacitor between Reset and DTR of serial adapter
* 10k resistor between reset and Vcc
* (optional) LED and series resistor from Arduino pin 2/PB2/physical pin 5 to ground (This is the pin optiboot flashes)

An example amenable to home etching can be found at http://drazzy.com/e/boards/boards.php

I'm planning to make a professionally fabbed board after verifying the current design, and will make that design public on OSHpark et. al. 

Installation
============

INSTALLATION

First ensure the Arduino software is correctly installed, and that the IDE is not running during the installation process. 


* Locate your Arduino Sketch folder.  This is the folder where the Arduino IDE
  stores Sketches, typically located in your Documents folder. 

* Ensure the "hardware" folder exists under the Arduino Sketch folder. If it is not there, create it. 



* Download Arduino-Tiny-841 from github as a ZIP file, and extract it into the 
  "hardware" folder, or simply clone the github repo into there.  For example,
  if the Arduino Sketch folder is...

      C:\Users\YourName\Documents\Arduino

  After extracting, the following files / folders should exist...

      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\LICENSE
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avrdude_conf.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\Boards.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\ChangeLog
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\README
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\README.md
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\platform.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\programmers.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\bootloaders\
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\cores\

  The following folder should contain the source files for the Arduino-Tiny
  core...

      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\cores\

* Locate avrdude.conf - typically in 
  C:\Program Files (x86)\Arduino\tools\avr\etc 

* If you are using Windows Vista or later, right-click avrdude.conf and
  choose the Security tab. Select "Users", and see if there is a checkmark 
  in the "Allow" column for "Full Control". If not, click Edit, select Users, 
  and click the checkbox to Allow Full Control. Apply.

* Open avrdude.conf using any text editor. Copy/paste the contents of 
  avrdude_conf.txt onto the end of avrdude.conf and save. 

*IF YOU ARE USING ARDUINO 1.6:*

* open the arduino-tiny-841 folder, and create a new folder named "avr". 
  Move the cores and bootloaders folders, as well as boards.txt, platform.txt, and programmers.txt into "avr"

* In this avr folder, create a "libraries" folder. Copy the EEPROM and SPI libraries into this folder from  
      (Arduino install location)/hardware/arduino/avr/libraries/

*IF YOU ARE USING ARDUINO 1.0.x, you must update the compiler toolchain* 
  
* Download and install Atmel Studio 6.2 (available from the Atmel website). 

* Locate the location of the Arduino toolchain, typically in:

  C:\Program Files (x86)\Arduino\tools\avr

* If using Windows Vista or later, right-click avr folder, Security tab. 
  Select "Users", and see if there is a checkmark in the "Allow" column for
  "Full Control". If not, click Edit, select Users, and click the checkbox
  to Allow Full Control. Apply.

* Copy the AVR toolchain from Atmel Studio over the old Arduino Toolchain, 
  replacing files when prompted. 

  C:\Program Files\Atmel\Atmel Toolchain\AVR8 GCC\Native\(version)\avr8-gnu-toolchain

  You must leave the old toolchain there, and copy these over it, because
  there are files needed by Arduino in addition to the toolchain that are
  located in the same directory. 
