---
layout: post
title: "Failure modes of LED Lighting"
categories: LED Lighting"
---

Years ago i have exchanged all lights im my house to LEDs. Vendors claim that they live much longer than older (halogen or CFL) types.

But i am not entirely happy with them. They fail much earlier than the claimed ten thousands of operating hours.

Instead of throwing them away, i open them and try to analyse the reason for the failures.

**About the claimed life time**

Having a light on 24/7, this would mean 8760 operating hours per year.

If the claim is 20000 operating hours, most should live more than 2 years.

A claim of x operating hours life time means that most of the specimen in a sample live that long. This is not a MTBF. The life time is specified to make a statement about the minimum brightness after this time - but black is 0%..

This is an article with some more facts about LED life time:
[Link](https://www.edn.com/electronics-blogs/led-zone/4441442/How-long-do-LEDs-really-last-)


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
 