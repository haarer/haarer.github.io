---
layout: post
title: Connect a LVDS LCD panel to a cubieboard2
categories: lcd cubieboard
---


I wanted to connect a LVDS LCD Panel directly to my cubieboard - not using an HDMI cable.

# Hardware

* cubieboard 2
* old toshiba 12" LCD panel (800x600)

# useful links

* http://linux-sunxi.org/Display
* http://linux-sunxi.org/Cubieboard/LVDS
* https://linux-sunxi.org/Fex_Guide#disp_init_configuration
* http://tinyvga.com/vga-timing/800x600@60Hz

# How i did it

First i changed the fex configuration

* mount the boot partition:
`mount /dev/nanda /mnt/`

* the binary device configuration must be converted to human readable form `bin2fex /mnt/script.bin /root/1.fex`

* edit fex file (see content below)

Content to add to fex file:

    [disp_init]
    disp_init_enable = 1
    disp_mode = 0
    screen0_output_type = 1
    screen0_output_mode = 5
    
    [lcd0_para]
    lcd_used = 1
    lcd_x = 800
    lcd_y = 600
    lcd_dclk_freq = 51
    lcd_pwm_not_used = 0
    lcd_pwm_ch = 0
    lcd_pwm_freq = 10000
    lcd_pwm_pol = 1
    lcd_max_bright = 240
    lcd_min_bright = 64
    lcd_if = 3
    lcd_hbp = 158
    lcd_ht = 1056
    lcd_vbp = 23
    lcd_vt = 1256
    lcd_vspw = 3
    lcd_hspw = 20
    lcd_hv_if = 0
    lcd_hv_smode = 0
    lcd_hv_s888_if = 0
    lcd_hv_syuv_if = 0
    lcd_lvds_ch = 0
    lcd_lvds_mode = 0
    lcd_lvds_bitwidth = 1
    lcd_lvds_io_cross = 0
    lcd_cpu_if = 0
    lcd_frm = 1
    lcd_io_cfg0 = 0
    lcd_gamma_correction_en = 0
    lcd_gamma_tbl_0 = 0x0
    lcd_gamma_tbl_1 = 0x10101
    lcd_gamma_tbl_255 = 0xffffff
    lcd_bl_en_used = 0
    lcd_bl_en = port:PH07<1><0><default><1>
    lcd_power_used = 0
    lcd_power = port:PH08<1><0><default><1>
    lcd_pwm_used = 0

* convert the fex file back to binary `fex2bin /root/1.fex /mnt/script.bin`

* unmount boot partition `umount /mnt/`

* change uEnv.txt as follows, to make the cubieboard use the other output.

Content of uEnv.txt

    console=tty0
    extraargs=console=ttyS0,115200 disp.screen0_output_mode=800x600  rootwait panic$
    nand_root=/dev/nandb

* wire the panel

Panel Connections

The following table shows the connection of panel signals to the cubieboard. Additionally, there are connections for power supply of the panel and the panels backlight.

cubietruck2 pin |name                |lcd pin   |cable color
----------------|--------------------|----------|-----------
09              |Ground              |GND       |black
07              |PD6 (LCDD6/LVDS0PC) |06 (RXC+) |brown
10              |PD7 (LCDD7/LVDS0NC) |07 (RXC-) |brown-white
05              |PD4 (LCDD4/LNVS0P2) |09 (RX2+) |blue
08              |PD5 (LCDD5/LVDS0N2) |10 (RX2-) |blue-white
03              |PD2 (LCDD2/LVDS0P1) |12 (RX1+) |orange
06              |PD3 (LCDD3/LVDS0N1) |13 (RX1-) |orange-white
02              |Ground              |GND       |black-white
01              |PD0 (LCDD0/LVDSP0)  |15 (RX0+) |green
04              |PD1 (LCDD1/LVDS0N0) |16 (RX0-) |green-white
 

# Yess !
The result on my bench.
![it works](https://raw.github.com/haarer/haarer.github.io/master/_posts/20151129_011426.jpg)

