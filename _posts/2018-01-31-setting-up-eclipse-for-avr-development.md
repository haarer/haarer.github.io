# How i set up eclipse to develop for my AVR embedded development

## Which eclipse version to use
I used neon for quite some time, but now switched to [oxygen][1]

## Install the avr eclipse plugin

The archive (version 2.4.2) can be [downloaded here](https://sourceforge.net/projects/avr-eclipse/files/avr-eclipse%20stable%20release/)
It is installed by adding the archive as an update site in eclipse (select "archive").

There is also an avr-plugin available from the eclipse marketplace, but it is rather old (version 2.3.4).
I did not try it.
![the old plugin](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-01-31-avr-plugin-eclipse-marketplace.png)

## Install and configure a toolchain
I use my [build script][2] to compile a toolchain with the current gcc release 7.2

The tool chain is set up in the preferences dialog of eclipse
* MSYS is installed to c:\tools\msys64
* the toolchain is generated to /opt/crosschain inside MSYS
* i use make from the [unxutils](http://unxutils.sourceforge.net/) package

![prefs dialog](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-01-31-avr-eclipse-preferences.png)

I copied the avr dude executeable and some dlls to a separate directory to avoid setting search path to the dlls.
![prefs dialog](https://raw.github.com/haarer/haarer.github.io/master/_posts/2018-01-31-avrdude2.png)

## Tools for modeling the system architecture

Papyrus together with the SysML 1.4 plugin: [eclipse update site][3]



  [1]: https://www.eclipse.org/downloads/eclipse-packages/
  [2]: https://github.com/haarer/toolchain68k
  [3]: http://download.eclipse.org/modeling/mdt/papyrus/components/sysml14/
