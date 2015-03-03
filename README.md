arduino-tiny-841
============

A fork of shimniok's ( github.com/shimniok ) fork of arduino-tiny, which made an attempt to support the Tiny841. All work appears to have stopped on that core, and it was never in a state where sketches could be compiled (it looks like the initial work was never completed). 

This fork aims to finish what he started and add working support for the ATtiny841 on Arduino. 

Status
===========

* Serial and Serial 1 work. 
* INPUT_PULLUP works
* millis and micros work
* analogRead() works
* PWM works on all 6 channels. 
* EEPROM works.
* tone is untested. 
* SPI is untested - but should work. Registers are identical to the mega328. 
* I2C/TWI hardware slave needs library support. It is not the same as the master/slave TWI on mega's, and the 841 does not have a USI, so the USI I2C libraries that exist for the tinyx4/x5 won't work either. I can't make SoftI2CMaster work, but that may be due to interrupt issues. 
* Pin change interrupts are untested (including INT0).
* Something is wrong with the upload process using ArduinoAsISP - it's SLOOOOOOW using Arduino-as-ISP. There seems to be a very long period of time at the start of upload where it engages in some sort of repetitive action, before eventually going ahead and uploading the compiled sketch. 
* After uploading, the IDE complains that pagel and bs2 need to be defined in avrdude.conf. I can find no documentation that explains what those two settings mean or how to determine their correct values.


Requirements
============

You must update the arduino compiler toolchain. 

I use Arduino 1.0.6 as a base. 

Download and install Atmel Studio 6.2, and copy the toolchain from that ontop of your existing Arduino toolchain (note - on recent versions of windows, you may have to jump through hoops to get windows to allow you to modify those files - Since Vista, windows UAC prevents modification of the contents of Program Files folders by default. Information on how to deal with this is readily available online and is beyond the scope of this document). 

You must add an entry to avrdude.conf (see avrdude_conf.txt). 

Then drop the core into your hardware folder. 
