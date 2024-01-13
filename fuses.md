NOTE: If you are just updating the firmware, you don't need to worry about any of this.

Fuse settings for the ATmega8
-----------------------------

| Fuse byte | Value |                      |
|:----------|-------|----------------------|
| Low       |  0x9f |                      |
| High      |  0xc1 | (without bootloader) |
| High      |  0xc0 | (with bootloader)    |

To program with avrdude use one of the following command lines:

    avrdude -c <your programmer> -p atmega8 -U lfuse:w:0x9f:m -U hfuse:w:0xc1:m
    avrdude -c <your programmer> -p atmega8 -U lfuse:w:0x9f:m -U hfuse:w:0xc0:m

Fuse settings for the ATmega168
-------------------------------

(These settings are from the original RetroAdapter 2.1a, before the modifications done in this repository.)

| Fuse byte | Value |
|:----------|-------|
| Low       |  0xef |
| High      |  0xdf |
| Extended  |  0xf8 |

To program with avrdude use the following command line:

    avrdude -c <your programmer> -p atmega168 -U lfuse:w:0xef:m -U hfuse:w:0xdf:m -U efuse:w:0xf8:m

Ignore warnings about the efuse not being set properly, it is a limitation of avrdude.

Calculating the Fuse bytes for your microcontroller
---------------------------------------------------

If you have a different microcontroller, please read the datasheet specific for your model.

You can also use a third-party tool like <https://www.engbedded.com/fusecalc/> to help understanding and calculating the values for each fuse byte.
