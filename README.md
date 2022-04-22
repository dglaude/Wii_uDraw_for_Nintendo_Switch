# Wii_uDraw_for_Nintendo_Switch

This is a proof of concept to emulate mouse for the Switch with CircuitPython

The default Mouse emulation from CircuitPython use a composite USB-HID descriptor that contain three element: keyboard, mouse and multimedia control.
When using that, the switch was not detecting a "mouse" was connected.
My failure have been documented in the following NON WORKING repo: https://github.com/dglaude/CircuitPython_Switch_Mouse_Emulation

The trick and trigger was to create a custom USB_HID descriptor that describe a simple mouse not using a composite descriptor.
So I reused the code from this experiment to replicate an absolute mouse: https://github.com/dglaude/CircuitPython_udraw_Absolute_Mouse
But I changed the descriptor by one found in Nordic documentation: https://infocenter.nordicsemi.com/index.jsp?topic=%2Fcom.nordic.infocenter.sdk5.v13.0.0%2Fgroup__app__usbd__hid__mouse__desc.html

I am not an expert in USB-HID, so I am not sure at 100% of the descriptor, but it work as a mouse on the Switch and on a PC... it is good enough.

# Hardware

* second hand uDraw Wii GameTablet (it talk I2C and connect like a Nunchuck)
* Adafruit Wii Nunchuck Breakout Adapter - Qwiic / STEMMA QT => https://www.adafruit.com/product/4836
* Stemma QT cable
* Feather RP2040 (should work with many board that hav, but avoid M0 as this code need long int)

# Connection to the switch

There are two options to connect the Feather RP2040 to the Switch:
* Directly to the Switch with a USB-C to USB-C cable
* To the docking station of the Switch with a USB-A to USB-C cable

I am not sure yet how to connect another CircuitPython board that use a microUSB, this is untested yet.

# How to reproduce

Install the following libraries:
* Adafruit_CircuitPython_HID: https://github.com/adafruit/Adafruit_CircuitPython_HID (from Adafruit Bundle)
* wiichuck: https://github.com/jfurcean/CircuitPython_WiiChuck (from the Community Bundle: https://github.com/adafruit/CircuitPython_Community_Bundle/releases/)

Copy the files `boot.py` and `code.py` on your CIRCUITPY drive.
Reset the board (this is required to make sure `boot.py` executed and taken into account.

# Demo

I posted a video on Twitter with a demo of this code: https://twitter.com/DavidGlaude/status/1517493861282721799?s=20&t=qfPjnya-DcQ7UgT6MioTdA

# Credit

The uDraw specific part is my piece of code and contribution to CircuitPython_WiiChuck library.
Everything else has only been possible thanks to:
* @danh for all learn guide and the USB-HID code in the core of CircuitPython and in library
* @bitboy85 for providing working code for mouse_abs: https://gist.github.com/bitboy85/cdcd0e7e04082db414b5f1d23ab09005
* @jfurcean John Furcean for the WiiChuck library: https://github.com/jfurcean/CircuitPython_WiiChuck
* Nordic Semiconductor where I borrowed a standard Mouse USB-HID descriptor (there are a lot of identical very similar from Microsoft, Silabs, Microchip, ...) my first tentative was avec Nordic and it is working so far.
