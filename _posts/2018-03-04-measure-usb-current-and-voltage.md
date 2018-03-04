---
layout: post
title: "Measure USB Voltage and Current - the BananoMeter"
categories: usb,voltage,current,"banana pi zero"
---

I wanted to measure Voltage and current on the USB (for my experiments with banana pi zero).
Instead of buying a cheap insert "instrument", or hooking a cable to my bench supply i cut an USB extension cable and inserted a China Ammeter.

By doing this, i can use this cable for any USB Measurements.

**Cutting the Cable**

I removed the outer cable hull and cut the screen. Then i cut the power supply wires.
They were red and black in my cable, but i double checked that with a multimeter.

The data wires were green and white - i left them alone.

**The Volt & Amperemeter**

I got [the "Instrument"][1] from ebay. It is pretty cheap.

It has an embedded shunt, beefy current wires, can be powered from the measured voltage, 
and has separate displays for current and voltage. It also has a decent measurement and supply range and a sufficiently good accuracy:

    High precision, four digital display
    Working voltage:3.5-30V DC
    Working current:<20mA
    Display: 0.28" Two color blue and red
    Measuring range:DC 0-100V 0-10A (NO NEED shunt)
    Refresh rate: about 1S / 3times

    Measure accuracy:
    Voltage:Range x 0.08%+Two decimal point
    Current: Range x 0.08%+Two decimal point

    Operating temperature: -10 to 65° c
    Working pressure: 80 to 106 kPa

    size :48×29×20 mm

    About wiring:
        Red line thin: power supply+
        Red line (thick): IN+, current input
        Yellow line (thick):PW+, measuring terminal voltage input positive
        Black line thick: COM, common measuring

    Package Contents
        100% Brand New
        1pcs 100v 10a meter with cable
        
They specify wiring diagrams :)

The following Diagram shows the meter powered by external source (this is needed if one wants to measure outside the power supply range)        

![cut cable picture](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-03-04-china-ammeter-wiring.png)

This shows the meter powered by measured voltage (this is possible only if the voltage in the supply range and the additional 20mA for the meter are not a problem)

![cut cable picture](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-03-04-china-ammeter-wiring-sourcepowered.jpg)

       
**Wiring the instrument**

For my USB meter i've chosen the source powered variant. 

 * The thin yellow and red cables go to usb + which is also connected to usb load and usb source.
 * the thin black cable is not needed. I isolated it, but it could be cut at the instrument as well.
 * the thick red wire goes to usb - of the load
 * the thick black wire goes to usb - of the source

I isolated the solder joints by heat shrink.

![cut cable picture](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-03-04-bananometer-cable-detail.jpg)

**Measuring idle current of Banana PI M2 Zero**

The picture shows the Meter measuring the Banana Pi Zero Idle current.
It works pretty well.

![Measurement](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-03-04-bananometer.jpg)



  [1]: http://www.ebay.de/itm/100V-10A-DC-Digital-LED-Voltmeter-Ammeter-Amp-Volt-Meter-Built-in-shunts-12v-24v-/271731598862?hash=item3f4477260e
  
