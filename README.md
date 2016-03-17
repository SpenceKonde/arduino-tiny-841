ATTiny Modern - 441, 841, 828, 1634 for Arduino 1.6.x (1.6.3+ recommended)
THIS CORE IS BEING MERGED WITH https://github.com/SpenceKonde/ATTinyCore
============

A fork of shimniok's ( github.com/shimniok ) fork of arduino-tiny, which made an attempt to support the Tiny841. All work appears to have stopped on that core, and it was never in a state where sketches could be compiled (it looks like the initial work was never completed). 

This fork aims to finish what he started and add working support for the ATtiny841 on Arduino. 

Additionally, it brings in support for the ATTiny1634, brought in from rambo's 1.0.6 ATTiny1634 core, and the ATTiny828

*AS OF 7/9/2015 PLEASE RE-BURN BOOTLOADER TO ANY 8MHZ BOARDS*
I was too ambitious trying to make these work at 115200 baud upload, and it wound up being incredibly picky. Seems to work reliably at 57600. 

**When uploading sketches via ISP, due to limitations of the Arduino IDE, you must select a programmer marked ATTiny Classic or ATTiny Modern from the programmers menu (or any other programmer added by an installed third party core) in order to upload properly to most parts.**

Board Manager Installation
============

This core can be installed using the board manager. The board manager URL is:

`http://drazzy.com/package_drazzy.com_index.json`

1. File -> Preferences, enter the above URL in "Additional Board Manager URLs"
2. Tools -> Boards -> Board Manager...
  *If using 1.6.6, close board manager and re-open it (see below)
3. Select ATTinyCore (Modern) and click "Install". 

Due to [a bug](https://github.com/arduino/Arduino/issues/3795) in 1.6.6 of the Arduino IDE, new board manager entries are not visible the first time Board Manager is opened after adding a new board manager URL. 

Status
===========

* Optiboot bootloader included, and works on the 441/841 (7.37, 8, 9.216, 11.056, 12, 14.74, 16, 18.43, and 20mhz), and 1634 (7.37, 8, 9.216, 11.056, 12, 14.74 and 16mhz), 828 (8mhz)
* Board definitions for non-optiboot 441/841 @ 1, 4, 6, 7.37, 8, 9.216, 11.056, 12, 14.74, 16, 18.43, 20mhz, and the 1634 @1, 4, 6, 7.37, 8, 9.216, 11.056, 12, 14.74 and 16mhz, 828 @ 1, 8 mhz (it doesn't support a crystal)
* Tone is untested on all chips. Please report any problems.
* SPI (441/841/828), Serial (all), and Serial1 (441/841/1634) work. 
* I2C/TWI hardware slave on 441/841/828 supported by WireS library: https://github.com/orangkucing/WireS for 441/841/828
* I2C/TWI software master on 441/841/828 works: https://github.com/todbot/SoftI2CMaster - However, libraries must be adapted. See https://github.com/SpenceKonde/LiquidCrystal_I2C_Tiny for an example
* USI for 1634 can be used for I2C - use this library for I2C master: https://github.com/SpenceKonde/TinyWireM  - However, libraries must be adapted. See https://github.com/SpenceKonde/LiquidCrystal_I2C_Tiny for an example
* Optiboot without the LED blink (noLED) for 841 included; this saves 64 bytes of flash (not used by default - modify boards.txt if needed)
* Optiboot on serial 1 for 841, 1634 included, these are postfixed with "ser1". These must be flashed manually or modify boards.txt. 

Pin Mapping
============

### ATtiny 441/841
![x41 pin mapping](http://drazzy.com/e/img/Tiny841.jpg "Arduino Pin Mapping for ATTiny 841 and 441")
### ATtiny 1634
![1634 pin mapping](http://drazzy.com/e/img/Tiny1634.jpg "Arduino Pin Mapping for ATTiny 1634")

### ATtiny 828

```

ATtiny 828 pin mapping. All pin numbers match ADC and PCINT numbers

//             16*   26   24   14
//          17    27   25   15
//             PC0  PD2  PD0  PB6
//          PC1  PD3  PD1   PB7
//             _________________
// 18 RX  PC2 | *               | PB5   13
// 19 TX  PC3 |                 | PB4   12
// 20 *   PC4 |                 | PB3   11
//        VCC |                 | GND
//        GND |                 | PB2   10
// 21 *   PC5 |                 | PB1    9
// 22 *   PC6 |                 | AVCC
// 23     PC7 |_________________| PB0    8
//           PA0  PA2  PA4  PA6 
//              PA1  PA3  PA5  PA7
//            0     2    4    6
//               1     3    5    7

```


Hardware
============

For use with Optiboot, the following components and connections are required:
* Arduino pin 9/PA1/TXD0 to RXI of serial adapter (0/PB0 on 1634)
* Arduino pin 8/PA2/RXD0 to TXO of serial adapter (1/PA7 on 1634)
* Diode between Reset and Vcc (band towards Vcc)
* 0.1uf capacitor between Reset and DTR of serial adapter
* 10k resistor between reset and Vcc
* (optional) LED and series resistor from Arduino pin 2/PB2/physical pin 5 to ground (This is the pin optiboot flashes)

An example amenable to home etching can be found at http://drazzy.com/e/boards/boards.php

Suitable breakout boards can be purchased from my Tindie shop:

841: https://www.tindie.com/products/DrAzzy/attiny84184-breakout/ 

1634: https://www.tindie.com/products/DrAzzy/attiny1634-breakout-wserial-header-bare-board/

828: https://www.tindie.com/products/DrAzzy/atmega-x8attiny-x8828atmega-x8pb-breakout/

Caveats
============

* On the 1634 and 841, when using the Optiboot bootloader, the Watchdog Timer interrupt vector will always point to the start of the program, and cannot be used for other functionality. Because the 1634 and 841 do not have built-in bootloader support, this is achieved with "virtual boot" feature of Optiboot. This bootloader rewrites the reset and WDT interrupt vectors, pointing the WDT vector at the start of the program (where the reset vector would have pointed), and the reset vector to the bootloader (as there is no BOOTRST fuse). This does not effect the 828 (it has hardware bootloader support), nor does it effect the 1634 or 841 if they are programmed via ISP.
* Some people have problems programming it with USBAsp and TinyISP - but this is not readily reproducible ArduinoAsISP works reliably. In some cases, it has been found that connecting reset to ground while using the ISP programmer fixes things (particularly when using the USBAsp with eXtremeBurner AVR) - if doing this, you must release reset (at least momentarily) after each batch of programming operation. 
* At >4v, the speed of the internal oscillator on 1634R and 841 parts increases significantly - enough that neither serial (and hence the bootloader) does not work. It is recommended to run at 3.3v if using internal RC oscillator as a clock source. A future release may include an 8.1mhz internal RC @5v option, with it's own bootloader. 
* When using weird clock frequencies (ones with a frequency (in mhz) by which 64 cannot be divided evenly), micros() is 4-5 times slower (~110 clocks); it still reports the time at the point when it was called, not the end, however, and the time it gives is pretty close to reality (w/in 1% or so). This combination of performance and accuracy is the result of hand tuning for these clock speeds. For really weird clock speeds (ie, if you add your own), it will be slower still - hundreds of clock cycles - on the plus side, it still gives reasonably accurate numbers back even on exotic clock speeds, ("stock" micros() executes equally fast at all clock speeds, and just returns bogus values with anything that 64 doesn't divide evenly by) 


Manual Installation
============


### All Versions:

First ensure the Arduino software is correctly installed, and that the IDE is not running during the installation process. 

* Locate your Arduino Sketch folder.  This is the folder where the Arduino IDE
  stores Sketches, typically located in your Documents folder. 

* Ensure the "hardware" folder exists under the Arduino Sketch folder. If it is not there, create it. 

* Download Arduino-Tiny-841 from github as a ZIP file, and extract it into the 
  "hardware" folder. Alternately, you can clone the github repo to that location - this allows you to simply sync it to pick up the latest changes to the core.  For example,
  if the Arduino Sketch folder is...

      C:\Users\YourName\Documents\Arduino

  After extracting, the following files / folders should exist...

      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\LICENSE
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\avrdude.conf
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avrdude_conf16x.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avrdude_conf106.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\Boards106.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\Boards.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\ChangeLog
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\README
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\README.md
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\platform.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\programmers.txt
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\bootloaders\
      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\cores\

  The following folder should contain the source files for the Arduino-Tiny
  core...

      C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\cores\
      

You want it to look like this:

![core installation](http://drazzy.com/e/img/coreinstall.jpg "You want it to look like this")


* If YOU ARE USING ARDUINO VERSION 1.6.2 (not 1.6.3 or later, nor 1.6.1 or earlier), delete platform.txt and rename platform_162.txt to platform.txt. In this case, you must follow the steps below to modify avrdude.conf. I strongly recommend updating if you are still using 1.6.2, as this version has serious defects in the loading of third party hardware definitions.



### 1.0.x-specific additional steps
Modifying avrdude.conf should no longer be necessary, ever as of 8/22/2015 changes. 

* Windows

  * Locate avrdude.conf - typically in 
    C:\Program Files (x86)\Arduino\tools\avr\etc 

  * If you are using Windows Vista or later, right-click avrdude.conf and
    choose the Security tab. Select "Users", and see if there is a checkmark 
    in the "Allow" column for "Full Control". If not, click Edit, select Users, 
    and click the checkbox to Allow Full Control. Apply.

  * Open avrdude.conf using any text editor. At the end of the file, copy+paste the contents of avrdude_conf_16x or 106 (depending on which version of the IDE you are using)

  * If you are using Arduino 1.0.x, move the contents of  C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\avr\ to C:\Users\YourName\Documents\Arduino\hardware\arduino-tiny-841\ 

* OSX

  * Assuming you are using Arduino 1.6.x (*Atmel doesn't provide an AVR toolchain for Mac, so you couldn't update the compiler toolchain as instructed below for 1.0.x*), open terminal and run this command: (change to match your Arduino Sketch directory if it is something other than `~/Documents/Arduino`)

````
cat ~/Documents/Arduino/hardware/arduino-tiny-841/avrdude_conf_16x.txt | pbcopy && pbpaste >> /Applications/Arduino.app/Contents/Java/hardware/tools/avr/etc/avrdude.conf
````

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
