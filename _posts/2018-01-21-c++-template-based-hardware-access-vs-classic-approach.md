---
layout: post
title: HAL programming - Comparison of the classic approach to control an io pin to a C++ template based solution
categories: c++ avr templates i/o
---

This Article is about HALs (Hardware Abstraction Layers), what they do, and how C++ can help implementing them.
I want to showcase how C++ templates can be used, and compare this to the "classic" approach.

But first, lets think about the problems progamming for a microcontroller.

# Low Level Programming Issues
When developing code for a microcontroller, one of the jobs is to access the registers in order control the state of output pins, read the state of input pins, and set the direction of i/o pins.
There are several ways how microcontroller vendors can design the hardware, but the programming model is usually memory based. This means there are several memory locations mapped to the chips i/o lines - writing to the locations changes the lines state, reading from locations retrieves the lines state. There are also memory locations for configuration of the port lines - as input, as output, input with pullup - each chip has its own way how to set or clear a line.

Some microcontrollers even have special instructions for port access (older ones, e.g. Z80), or special commands useful for single bit manipulations, like the AVR chips used in most Arduinos.

When it comes to programming, one has to learn the pecularities of the specific chip. This takes quite some time.

If one wants to program a variety of chips this time is multiplied. 

The solution for this problem is, that an expert creates a library which encapsulates the gory details of port manipulation.

# better call HAL 
This library is sometimes called a HAL (Hardware Abstraction Layer).
A HAL presents an uniform programming interface which works always the same regardless how the hardware is accessed.

 The people who developed the Arduino platform have done this, and this is one of the reasons for the great success of the Arduinos.
They are quite easy to program, one can create a program which blinks a LED within 10 minutes, including the installation of the development environment.

The code for an AVR based Arduino or an Expressif ESP32 is exactly the same, the HAL hides the different hardware architectures. 

# HAL costs
Unfortunatly there is a price, and it may be too high. This depends on on the implementation of the HAL
* the total size of the code may increase, because the HAL contains functions or implementations which are not used in the application
* the latency of i/o manipulation may be worse to "bare metal code" (this is platform specific code)
* there may be errors in the HAL which are hard to find 
* the API design of the HAL may not match the applications needs

# me and the Arduino HAL
In one of my hobby development projects i thougt: Hey, why not use an Arduino for it ? 
* they're cheap
* i wanted to use an AVR processor anyway
* they look great for prototyping - dont need to make a pcb and so on.. 
* there is a library for nearly anything that might be connected to an Arduino

I was a bit sceptical about the development environment, but i tried.

After some time i hit limits of the built in Arduino libraries. The timer i wanted to use was already used, the library hat high latency seting pin states, and so on.
I had also problems adding own header files.

I went back to my trusty Atmel Studio, but used avrdude with the "wiring" programmer type to upload my code to the Arduino board.
Because like readable and managable code i also wrote a HAL using c++ template programming.

# Templates to the rescue

There are many programmers thinking: Nooo Way, C++ , templates on a microcontroller - this is a "no go". 

I believe the opposite is true. Modern C++ allows to solve many of the problems in HAL programming by moving decisions from runtime into compile time. 

This is a tiny program with a trivial functionality.
It waits until a bit 6 of port b goes high (pin 12 on an arduino mega). Then it goes into a loop, toggling bit 7 of port b(this is pin 13 on an arduino mega, which is connected to the onboard led)
The program directly accesses the ports. No HAL at all.

```c++
#include <avr/io.h>
#include <util/delay.h>


int main(void)
{
        DDRB |= (1<<7);

        while(  (  ( PORTB & (1<<6) ) !=(1<<6) ) )
                ;

        while(1)
        {
                _delay_ms(100);
                PORTB |= (1<<7);
                _delay_ms(100);
                PORTB &= ~(1<<7);
        }
}
```

The second program uses a class template which is parametrized by an uint8 - the pin number. 
'note that the program sets the output direction always to out. this is to make it comparable to the classic version.'
```c++
#include <avr/io.h>
#include <util/delay.h>

typedef enum {in,out} PinDir_T;

template<uint8_t PinNo>
class IOPin
{
        public:
        IOPin(){};
        void SetDir(PinDir_T dir)
        {
                switch(PinNo)
                {
                        case  2: DDRE |= (1<<4); break; // pin  2 is PE 4 on arduino mega
                        case 12: DDRB |= (1<<6); break; // pin 12 is PB 6 on arduino mega
                        case 13: DDRB |= (1<<7); break; // pin 13 is PB 7 on arduino mega
                }
        }
        void SetHigh(void)
        {
                switch(PinNo)
                {
                        case  2: PORTE |= (1<<4); break;
                        case 12: PORTB |= (1<<6); break;
                        case 13: PORTB |= (1<<7); break;
                }
        }
        void SetLow(void)
        {
                switch(PinNo)
                {
                        case  2: PORTE &= ~(1<<4); break;
                        case 12: PORTB &= ~(1<<6); break;
                        case 13: PORTB &= ~(1<<7); break;
                }
        }
        bool State(void)
        {
                switch(PinNo)
                {
                        case  2: return (PORTE & (1<<4) ) == (1<<4); break;
                        case 12: return (PORTB & (1<<6) ) == (1<<6); break;
                        case 13: return (PORTB & (1<<7) ) == (1<<7); break;
                }
        }
};



int main(void)
{
        IOPin<13> ledpin;
        IOPin<12> testpin;
        ledpin.SetDir(out);

        while(0 == testpin.State())
                ;

        while(1)
        {
                _delay_ms(100);
                ledpin.SetHigh();
                _delay_ms(100);
                ledpin.SetLow();
        }
}

```

The main program is much more readable, and changing the pins requires only the change of the pin number (provided, that the IOPin class has definitions for that pin number).
It is also immediately clear, that the code in main() has no AVR specific aspects. They are all encapsulated in the IOPin class

*But..  uuuuuhhhhh - the code bloat.. and oooooohhhh - it will be slow.* 

No, its not bloated, and no its not slow.
Lets compare the code sizes !

*Classic version*
```
~/toolchain68k/examples/avr-cpp-example$ make size
avr-size main.o  avr-test.elf
   text	   data	    bss	    dec	    hex	filename
     48	      0	      0	     48	     30	main.o
    308	      0	      0	    308	    134	avr-test.elf

avr-size -Ax avr-test.elf
avr-test.elf  :
section                      size       addr
.data                         0x0   0x800200
.text                       0x134        0x0
.stab                       0x5dc        0x0
.stabstr                    0xe80        0x0
.comment                     0x11        0x0
.note.gnu.avr.deviceinfo     0x40        0x0
.debug_info                 0xbbc        0x0
.debug_abbrev               0xb1a        0x0
.debug_line                  0x1d        0x0
.debug_str                  0x3e6        0x0
Total                      0x30ba
```

*Template version*
```
~/toolchain68k/examples/avr-cpp-example$ make size
avr-size main.o  avr-test.elf
   text	   data	    bss	    dec	    hex	filename
     48	      0	      0	     48	     30	main.o
    308	      0	      0	    308	    134	avr-test.elf

avr-size -Ax avr-test.elf
avr-test.elf  :
section                      size       addr
.data                         0x0   0x800200
.text                       0x134        0x0
.stab                       0x60c        0x0
.stabstr                   0x11d2        0x0
.comment                     0x11        0x0
.note.gnu.avr.deviceinfo     0x40        0x0
.debug_info                 0xbbc        0x0
.debug_abbrev               0xb1a        0x0
.debug_line                  0x1d        0x0
.debug_str                  0x3e6        0x0
Total                      0x343c

```

What's this ? The sizes are the same ? Is this a copy and paste error ? :) 

No. The total size of the ELF files differs ($30ba vs $343c) but diffing the hex files reveals that the generated code is exactly the same in the classic and in the template version. 

I'll try to explain why:
* Templates are evaluated at compile time. That means, the constant pin number value passed to the template results in a compile time constant switch expression
* The compile time constant switch expression is simpified by the compiler to the content of the selected case.
* The compiler inlines the code in the called function of the IOPin class.

Basically, the compiler throws away nearly everything from the HAL class that isnt needed, and leaves only the code that one would have written by hand. 

A look into the lss file of the template version shows the efficiency of this optimization - it generates two assembler statements
for the `while(0== testpin.State())` loop:

*Template version*
```
00000100 <main>:
        {
                switch(PinNo)
                {
                        case  2: DDRE |= (1<<4); break; // pin  2 is PE 4 on arduino mega
                        case 12: DDRB |= (1<<6); break; // pin 12 is PB 6 on arduino mega 
                        case 13: DDRB |= (1<<7); break; // pin 13 is PB 7 on arduino mega 
 100:   27 9a           sbi     0x04, 7 ; 4
#ifdef TEMPLATEBASED
        IOPin<13> ledpin;
        IOPin<12> testpin;
        ledpin.SetDir(out);

        while(0 == testpin.State())
 102:   2e 9b           sbis    0x05, 6 ; 5
 104:   fe cf           rjmp    .-4             ; 0x102 <main+0x2>
``` 

The classic implementation isn't any bit better:
*Classic version*
``` 
00000100 <main>:


int main(void)
{
#ifdef CLASSIC
        DDRB |= (1<<7);
 100:   27 9a           sbi     0x04, 7 ; 4

        while(  (  ( PORTB & (1<<6) ) !=(1<<6) ) )
 102:   2e 9b           sbis    0x05, 6 ; 5
 104:   fe cf           rjmp    .-4             ; 0x102 <main+0x2>
 
``` 

The code and a Makefile is in the examples/avr-cpp-example subdirectory of my [toolchain project](http://github.com/haarer/toolchain68k).
 


