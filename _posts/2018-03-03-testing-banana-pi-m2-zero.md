---
layout: post
title: "Testing Banana PI M2 Zero"
categories: linux embedded arm allwinner "banana pi zero"
---

Somedays ago, i ordered an Banana PI M2 Zero from [Reichelt][1]. Today i had time, and decided to try it. 
It was a nice cold sunny day, the sun was shining into my lab -so what could possibly go wrong ?

Be warned  - This is another sad story about poor quality of software, documentation and support. 

**Beware of Mini HDMI Cables**

The Banana PI M2 Zero, just as the Raspberry PI Zero both have a Mini HDMI connector. I was pretty sure i had an adapter for that around but the adapter had a different opinion about its location. After approx an hour of searching i gave up and went shopping. 

This was an epic fail - the only Mini HDMI Cables were i found were of the Vivanco Brand - with a price Tag of 26€! 
It seems the cables are made by naked virgins in the full moon in some ancient temple or so. 

Well, i definitly could not buy a cable for this price and decided to buy [this adapter][2] or [this cable][8] later.

**Images for the Banana PI Zero**

There are quite some images available for the Banana PI Zero, but there seems to be no "mainstream support" by any distribution.

Their [download site][4]  seems to be some melting pot for various images from all kinds of sources.

The [armbian site][5] does list some H2+ based boards, but not the banana-pi-zero. [It seems the reason is the poor support by the manufacturer][6] - at least some of the posts in the armbian forum indicate that the manufacturer is either not willing or competent to provide correct information about their products. 

I downloaded this [armbian image][3] and wrote it to an SD Card using Win32DiskImager.

**Serial Console**

For now i want to use the serial console to get the board up and running.

The Banana PI Zero has a 3 pin connector for a serial interface - an advantage to the raspberry pi zero. 

An other advantage is, that the [armbian image][3] i used already supports the serial console, in the raspberry image i had to change the config of the serial port first to enable the console.

The Console runs on 115200, and the ambian image with default password 1234 begins with setting up the root password and creation of a new user. This is nice.

**Setting up WIFI from console**

WIFI is set up most easily using the nmtui tool from the console. I just started the tool, added the adapter and set up ssid and password - perfect !
The board connected to my network and i could continue using ssh.


**Updating the Image, bricking and debricking**

After the wifi was set up, i updated the system using apt. There are preconfigured package sources and the update found quite some updates. 

But, unfortunately after rebooting, the boot process halted. The message: 

    ** File not found /boot/dtb/sun8i-h2-plus-bananapi-m2-zero.dtb **
    
Indeed, the file wasn't there (revealed by the following commmand)
   
    ls mmc 0 /boot/dtb 

What can i do about this ? Look into the original image. First analyse the image file:

    fdisk -l 2017-12-04-Armbian_5.36_Bananapim2zero_Ubuntu_xenial_next_4.14.3_desktop_preview_build_by_bpi.img.img
    Disk 2017-12-04-Armbian_5.36_Bananapim2zero_Ubuntu_xenial_next_4.14.3_desktop_preview_build_by_bpi.img.img: 2.9 GiB, 3045064704 bytes, 5947392 sectors
    Units: sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disklabel type: dos
    Disk identifier: 0xac323dfe

    Device                                                                                                      Boot Start   End Sectors  Size Id Type
    2017-12-04-Armbian_5.36_Bananapim2zero_Ubuntu_xenial_next_4.14.3_desktop_preview_build_by_bpi.img.img       8192 5947391 5939200      2.9G 83 Linux

The offest is blocksize * the partition start : 512*8192 = 4194304. This is the offset to mount it.

    mount -o loop,offset=4194304 2017-12-04-Armbian_5.36_Bananapim2zero_Ubuntu_xenial_next_4.14.3_desktop_preview_build_by_bpi.img.img  /mnt

Okay, image is mounted, lets check for the missing file..
    
    ls /mnt/boot/dtb/*banana*
    /mnt/boot/dtb/sun7i-a20-bananapi.dtb
    /mnt/boot/dtb/sun8i-a83t-bananapi-m3.dtb
    /mnt/boot/dtb/sun8i-r16-bananapi-m2m.dtb
    /mnt/boot/dtb/sun7i-a20-bananapi-m1-plus.dtb
    /mnt/boot/dtb/sun8i-h2-plus-bananapi-m2-zero.dtb
    /mnt/boot/dtb/sun7i-a20-bananapro.dtb
    /mnt/boot/dtb/sun8i-h3-bananapi-m2-plus.dtb
    
There we have our missing banana ;-)
At this point i removed the SD Card from the banana and put into a card reader to copy the missing file from the image.
I also diffed some of the boot files: 

 *  armbianEnv.txt had added **usbstoragequirks=0x2537:0x1066:u,0x2537:0x1068:u**
 * the Kernel config exposed various differences - but it seems this has no effects.

After unmounting the card and reinserting it into the banana-pi-zero, it booted again, and i could login using wifi again. 

Clearly, this is not the fault of the armbian people. they just don't support the board for good reasons. 

** Testing GPIO**

Now it is time to test GPIO. 

I hooked up a LED from Ground to pin 11 (this is GPIO17 and Pin 0 from wiring libraries perspective)

I built the BPI Wiring Library and GPIO Tools from the [Sinovoip Github repo][7]
Unfortunately the GPIO tool based on their own library didnt work at all, it did complain that 
this wasnt a genuine Raspberry .. 

    Unable to determine hardware version. I see: Hardware   : Allwinner sun8i Family
    ,
    - expecting BCM2708, BCM2709 or BCM2835.
    If this is a genuine Raspberry Pi then please report this
    to projects@drogon.net. If this is not a Raspberry Pi then you
    are on your own as wiringPi is designed to support the
    Raspberry Pi ONLY.

After digging in the code of the Sinovoip Wiring library, i found out, that they detect the board type by 
checking a file board.sh in /var/lib/bananapi

    if ((bpiFd = fopen("/var/lib/bananapi/board.sh", "r")) == NULL) {
        return -1;
    }
    
needless to say.. neither the file nor the directory wasn't there. 

I provided a file with only one line

    BOARD=bpi-m2z

After this trick, the GPIO Library recognized the Board and i could toggle my LED

    gpio toggle 0
    
    
**Conclusion**

I definitely do not recommend the Banana PI M2 Zero for beginners. 
 * The lack of support by the manufacturer makes it hard to get it running.
 * There is no continously updated distribution for it.
 * The Documentation is poor, and not maintained
 * it is hard to find information because most information applies to the genuine raspberries and does not apply to the banana pi zero
 * the CSI is incompatible to the Raspberry, you need to buy a different camera module
 
The board has some features i like
 * it has an antenna connector
 * it comes with a wlan antenna
 * it has the possibility to connect a RJ45 for LAN 
 * it has an extra header for a serial console




  [1]: https://www.reichelt.de/Einplatinen-Computer/BANANA-PI-ZERO/3/index.html?ACTION=3&GROUPID=8242&ARTICLE=218297
  [2]: https://www.reichelt.de/A-V-Adapter-HDMI-/AD-HDMI-MINI/3/index.html?ACTION=3&GROUPID=5385&ARTICLE=103082
  [3]:  http://forum.banana-pi.org/t/bpi-m2-zero-new-image-2017-12-04-armbian-5-36-m2-zero-ubuntu-xenial-next-4-14-3-desktop-preview-buildbybpi/4325
  [4]: http://www.banana-pi.org/downloadall.html
  [5]: https://www.armbian.com/download/
  [6]: https://forum.armbian.com/topic/4801-banana-pi-zero/
  [7]: https://github.com/BPI-SINOVOIP/BPI-WiringPi2
  [8]: https://www.reichelt.de/A-V-Kabel-HDMI-/AK-HDMI-100AC/3/index.html?ACTION=3&GROUPID=3615&ARTICLE=132921
