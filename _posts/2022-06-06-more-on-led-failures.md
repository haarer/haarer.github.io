---
layout: post
title: "More on LED Failures"
categories: LED Lighting"
---

In 2018 i wrote an [Article]() about failures on LEDs i use for lighting. Over the Years i often had to exchange them - much earlier than the claimed lifetime of 15000+ hours suggests. 

The outside lights are on from dusk till dawn - that means an on time of 4200 hours per year.
They should last 3 years at least.

All are GU10 Socket type (230 V AC) used in metal case outdoor fixtures in a bone dry location high at the wall directly under the roof.

Unfortunatly, some of the LEDs were defect within a year, and we're not talking about no-name el-cheapo stuff but top brands as Osram.

Today i took the time to crack open the six LEDs that failed this year.
![2022 LED fail collection](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_163206906.jpg)

They are 
 * 2 Osram
 * 2 Megos
 * 1 VTAC VT2096
 * 1 Hornbach

Fun Fact: The one with the longest run time was the VTAC-2096 which lived 5 years since 2017 - another one of this type did last only one year.

**What are possible Reasons for Failure**
There are some possible reasons for failure with increasing probability
 * bad components 
 * moisture and subsequent short circuit
 * overvoltage (transient)
 * overtemperature

The most probable reason is overtemperature. This is caused by poor thermal design and the need to max out the lumen in a tiny case. 

All the short lived LEDs ( failed after a year)had a glass case. Glass is a good thermal insulator.

The 5 years living VTAC had a plastic coated aluminum case with the LED board thermoglued to the case. By far the best thermal design of the six.

But lets start with the worst:
**OSRAM**
The tiny "heat sink" is just sticked to the PCB with some specks of thermal glue - there are holes for screws, but no screws. In both LEDs the heat sink was loose and had no contact to the PCB anymore. In one case the LED chip desoldered itself and fell off. It even left burn marks on the lens.
The circuit is a primary switchmode design.

**MEGOS** 
The very thin PCB is thermoglued to an aluminum plate. The circuit probably is a primary switcher but i didnt notice a coil driving 6 LED chips
![2022 LED fail collection](https://raw.github.com/haarer/haarer.github.io/master/_posts/IMG_20220606_164026383.jpg)


**Hornbach**

**VTAC VT-2096**






**LED technical variants**

I use two totally different types: 

 * GU10 Socket (230 V AC, lots of failures)
 * GU5.3 (12 V AC, not a single failure)
 
 For the 12 V types which i use in certain lights mostly for safety reasons, i did not have a single fail, after several years of operation.
 The 230 V types fail much more often, in various rooms and outside lights.
  
 **230V Designs**
 
 The 230 V types use following different designs
 
 * capacitive voltage divider (lots of failures)
 * directly driven high voltage led chain  (few failures)
 * switchmode converter (very few failures)

 **12V Designs**
 
 The 12 V types use the following designs
 
 * switchmode converters
 * series resistors (rare)

 
 
 **VTAC VT-2096 failure**
 
 This is a 230V GU10 6W 500LM with a claimed life time of 20000 hours and 15000 on/off cycles
 
 [Link to VTAC page](https://v-tac.eu/led-lights/lights1/led-spotlights-gu10/led-spotlight---6w-gu10-smd-white-plastic-milky-cover-warm-white-detail.html)
 
 It did last less than one year, in a sealed outside light, automatically switched on over night.
 
 The electrical design is a capacitive voltage divider. The primary failure reason was capacity loss of the lower cap. In turn the voltage over the led chain rises and one led chip failed.
 
 The cap is a 105 °C rated chinese brand cap. The caps can has bulged.
 
 The thermal design has the LED chips on an aluminium backed pcb, and a plastic case with a thin aluminium can inside. There is no thermal contact betwenn the aluminium led carrier and the aluminium can. 
 
 ![VT-2096 dismantled](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-09-27-vtac-led-fail.jpg)
 
 
 **Melitec L81-2 failure**
 
 This is a 230V GU10 6.5W 345LM with a claimed life time of  25000hrs, and 20000 on off cycles
 
 [Link to a page with an image of product, it is no longer available](https://www.leuchtenservice-shop.de/LED-Leuchtmittel-L81)
 
 It was tested in the german magazin "test" of "Stifung Warentest" with a "good" result in march 2016.
 
[Link to test page](https://www.testberichte.de/p/melitec-tests/led-leuchtmittel-l81-testbericht.html)
 
 The electrical design is a switchmode converter with galvanic separation, driving 6 LED chips on an aluminium backed pcb. Three of the LED chips are connected in series, and those two chains are parallel. The led chips operate at approx. 9V.
 
 The thermal design has the led carrier screwed to a massive aluminium heat sink, which is also the case. There is  thermal grease applied.
 
 Despite the good thermal design, both LED chains have a failed LED.
 
 ![L81-2 dismantled](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-09-27-melitec-led-fail.jpg)
 