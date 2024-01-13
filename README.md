RetroAdapter
============

Originally written by Paul Qureshi (for the ATmega168 microcontroller).

Modifications in this repository by Denilson Sá (adapted to the ATmega8 microcontroller).

Overview
--------

This repository contains my changes on top of RetroAdapter (originally for
ATmega168) to make it work with the ATmega8 microcontroller that I already had.
This is basically the RetroAdapter version 2.1a with these changes:

* some tweaks in the Makefile,
* an updated usbdrv,
* many modules disabled (because I'll never use them, and to reduce the size of the firmware),
* a few conditionals hardcoded (because my hardware only supports one kind of controller),
* optional bootloader changed (or updated) from bootloadHID to USBaspLoader.

The firmware in this repository worked on the ATmega8 microcontroller.

Feel free to use this repository as a starting point for your own project.

Background
----------

For a long while I wanted to build a Sega Mega Drive controller adapter to USB.
Fortunately, Paul Qureshi wrote have created RetroAdapter and published all the
source and schematics on his website <http://denki.world3.net/retro_v2.html>.
This adapter uses an ATmega 8-bit microcontroller with almost no extra
components (only a few resistors, a few capacitors, a couple of Zener diodes, a
crystal).

He used ATmega168, which is more powerful than the ATmega8 that I already have.
In order to make it compile and make it fit in 8KB (or 6KB + bootloader), I had
to make small changes to the RetroAdapter code. These changes are saved in this
repository.

The Makefile changes mostly come from my previous project:
[atmega8-magnetometer-usb-mouse](https://github.com/denilsonsa/atmega8-magnetometer-usb-mouse/blob/master/firmware/Makefile)

My blog post about this project:
<http://denilson.sa.nom.br/blog/2013-07-15/i-built-a-retroadapter>

How to get started
------------------

If you're not familiar with AVR ATmega microcontrollers, please read my
multi-part series:
[First contact with ATmega8 microcontroller](https://denilson.sa.nom.br/blog/2007-10-25/first-contact-with-atmega8-microcontroller-part-1).
It starts from the very basics and goes step by step on building a simple
project using such microcontrollers.

Once you're familiar with AVR ATmega microcontrollers, and you have the
necessary software tools, you can dig into this repository:

* [fuses.md](fuses.md) - Lists what are the expected Fuse values to be
  programmed into the microcontroller.
* [hardware/](hardware/) - Schematics of the original RetroAdapter and also for
  the ATmega8 version.
* [source/](source/) - The entire source-code for the firmware and for the
  optional bootloader. Includes a [Makefile](source/Makefile) which can be used
  to build the firmware, write it to the microcontroller, and also write the
  fuse bytes.
* [more_docs/](more_docs/) - Further documentation on the RetroAdapter, saved
  from the original website before it went offline.
* [legacy_from_RetroAdapter_2.1a/](legacy_from_RetroAdapter_2.1a/) - Other
  files originally from RetroAdapter 2.1a, before the changes from this
  repository. You probably don't need these.
