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
* PWM does not work (which sort of defeats the whole point of the part)
* tone is untested. 
* SPI is untested.
* I2C is untested.
* pin change interrupts almost certainly don't work. 


Requirements
============

You must update the arduino compiler toolchain. Download and install Atmel Studio 6.2, and gank the toolchain from that, and replace the existing Arduino toolchain (note - on recent versions of windows, you may have to jump through hoops to get windows to allow you to modify those files - Since Vista, windows UAC prevents modification of the contents of Program Files folders by default. Information on how to deal with this is readily available online and is beyond the scope of this document. 

You must add an entry to avrdude.conf (see the avrdude section). 

Then drop the core into your hardware folder.
