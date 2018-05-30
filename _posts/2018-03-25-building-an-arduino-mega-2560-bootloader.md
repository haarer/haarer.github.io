---
layout: post
title: "Building an Arduino Mega2560 Bootloader"
categories: arduino,bootloader
---

I made some experiments with a [cryptographic library for avr processors][1].
During my tests i had the problem, that i could not flash the examples into my Arduino Mega2560 (a sainsmart clone). I double checked the avrdude parameters, but they were ok, and i could flash a different program with the same parameters. I had similar problems before, and i suspected the bootloader, which contains a also a monitor program. I read somewhere, that the monitor might interfere with the upload, because it is activated by a sequence of "!!!" in the data. 

Since i dont need the monitor, and dont want to have a monitor that is able to dump the memory together with key material, i searched for the source code. 
It seems the origin is [here](http://www.avr-developers.com/bootloaderdocs/index.html)

On the Arduino git site is an [other version](https://github.com/arduino/Arduino-stk500v2-bootloader)

It is not clear which is the latest and maintained source for the bootloader code - i decided to try the Arduino repo

There is also a bootloader called [optiboot](https://github.com/Optiboot/optiboot) having a quite [actively developed fork](https://github.com/sleemanj/optiboot), but it has no support for the mega2560.


**Building a Bootloader**

The build process is straigth forward - there is a Makefile. 
Unfortunatly they seem to build the Bootloader with an ancient compiler. I had to make some changes to get it compiled with gcc 7.3.

There is also no means to disable the Monitor, but this is easily added. 

The result source code is [my fork][2]. The bootloader is built by this command:

	make mega2560 -e DISABLE_MONITOR=1

**Flashing the Bootloader**

For flashing, a programmer must be connected to the ISP header. I have an avr-dragon, which is a good programmer for a low price.

The following commands unlock the bootloader, erase the chip and then flash the new code , and lock the bootloader again

	avrdude -C/opt/crosschain/etc/avrdude.conf -patmega2560 -cdragon_isp -Ulock:w:0x3F:m -Uefuse:w:0xFD:m -Uhfuse:w:0xD8:m -Ulfuse:w:0xFF:m  -v -e

	avrdude -C/opt/crosschain/etc/avrdude.conf -patmega2560 -cdragon_isp -v -Uflash:w:stk500boot_v2_mega2560.hex:i -Ulock:w:0x0f:m -v

**Success**

After i had flashed the monitor-less bootloader, the upload problems disappeared.



  [1]: https://wiki.das-labor.org/w/AVR-Crypto-Lib
  [2]: https://github.com/haarer/Arduino-stk500v2-bootloader
  
