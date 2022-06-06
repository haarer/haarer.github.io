---
layout: post
title: "More on LED Failures"
categories: LED Lighting"
---

In 2018 i wrote about [failures on LEDs i use for lighting](https://haarer.github.io/led/lighting%22/2018/09/27/led-fail-too-early.html) . Over the Years i too often had to exchange them - much earlier than the typically claimed lifetime of 15000+ hours suggests. 

Unfortunatly, some of the LEDs were defect within a year, and we're not talking about no-name el-cheapo stuff but top brands as Osram.

All are GU10 Socket type (230 V AC) used in metal case outdoor fixtures in a bone dry location high at the wall directly under the roof.

The outside lights are on from dusk till dawn - that means an on time of 4200 hours per year. They should last more than 3 years.

**The bad bunch**
Today i took the time to crack open the six LEDs that failed this year.
![2022 LED fail collection](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_163206906.jpg)

They brands are 
 * 2 Osram ACO 1534 4.3W / 350lm
 * 2 Megos IN106027 5.5W / 300lm
 * 1 VTAC VT-2096 6 W / 500lm
 * 1 Hornbach #6251295 4.8W / 345lm 

**Fun Fact #1**: The one with the longest run time of this years faulty LEDs was the VTAC-2096 which lived 5 years since 2017. Another one of this type did last only one year untim 2018.

**Fun Fact #2**: No product data is available on the web for any of the LEDs - except for the Hornbach type which still can be [shopped](https://www.hornbach.de/shop/3x-LED-Reflektorlampe-PAR16-GU10-4-8W50W-345-lm-2700-K-warmweiss-3-Stueck/6251295/artikel.html).

**Possible Reasons for Failure**
There are some possible reasons for failure with increasing probability
 * bad components 
 * moisture and subsequent short circuit
 * overvoltage (transient)
 * overtemperature

*I believe the major reason is overtemperature.*
Why ? Because the poor thermal design observed in most LEDs i cracked open.
The LEDs are made cheap and they try max out the lumen in a tiny case which must be insulated agains mains voltag because those LEDs are often a direct replacement for exposed halogen lights which could be touched.

All the short lived ( lasting a year) LEDs of this batch had a glass case. Glass is a good thermal insulator.

The 5 years living VTAC had a plastic coated aluminum case with the LED board thermoglued to the case. By far the best thermal design of the six.

But lets start with the worst:
**OSRAM**
![OSRAM](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_165520730.jpg)
![OSRAM2](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_165548390.jpg)
The tiny "heat sink" is just sticked to the PCB with some specks of thermal glue - there are holes for screws, but no screws. In both LEDs the heat sink was loose and had no contact to the PCB anymore. In one case the LED chip desoldered itself and fell off. It even left burn marks on the lens.
The circuit is a primary switchmode design.

**MEGOS** 
![MEGOS](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_164026383.jpg)
The very thin PCB is thermoglued to an aluminum can which is glued to the glass case. The circuit probably is a primary switcher driving 6 LED chips - i didnt notice a coil though.

**Hornbach**
![Hornbach ](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_170241574.jpg)
![Hornbach pcb ](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_170411004.jpg)
A primary switcher driving 5 chips on a thick aluminum core board.

**VTAC VT-2096**
![VTAC](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_170625662_MP.jpg)
Plastic coated aluminum case with the LED board thermoglued to the case. The heat dissipation is superior to the others, the circuit design is not. It is a capacitor divider / rectifier type driving 12 chips. The capacitor is the weak spot and failed (after 5 years).

**Lessons learned**
* dont buy glass cased LEDs, go for metal case if possible
* top brands are not worth the price
* the cheaper the better - they will fail anyway
* if you want it really long lasting, desing your own
  (i did this to upgrade a GU 9 based halogen wall light, used COB LEDs with a *proper* heat sink driven by a meanwell brand constant current source. I has to fail yet, since ~ 8 years )