---
title: MuKeyboard
excert: Modified version of Keyboard.h for Arduino, based on V-Bus framework.
tags:
 - MuKeyboard
 - Arduino
 - V-Bus
 - Rubber Ducky
 - Hacking
 - HID
 - Keyboard
 - USB
category:
 - project
toc: false

---

# MuKeyboard
## Modified version of Keyboard.h for Arduino, based on V-Bus framework.

   @author: MrMoDDoM
   @copyright: 2017<br>
   @version: 0.5

  ----------------------------------------------------------
  This project is hosted and supported by the hackersapece MuHack,<br>
  with love from Brescia, Italy ( www.muhack.org ) ;)<br>
  ----------------------------------------------------------

**With this library is possible to emulate an HID keyboard on every Arduino board, even without the HID interface.
It's completly compatible with every sketch written for the standard Keyboard.h library**

It's necessary to use a small circuit to interface with the USB cable:

![Circuit](https://github.com/MrMoDDoM/MuKeyboard/blob/master/schematic.jpg)

_Thank to http://www.practicalarduino.com for the image and concept_

### Refer to the example sketch to understand the usage

**This project is still under beta phase, where will be a lot of change**

# Installation
Simply ``` git clone https://github.com/MrMoDDoM/MuKeyboard.git ``` directly into your Arduino's library folder & compile!

# Disclaimer
In this project is present code for the Arduino INC and from Objective Development Software GmbH, covered with different type of licens: probably I have made some mistakes using and re-covering code with GNU/GPL v3, so please write me before take legal actions against my project.
**My intentions were not to steal work from anyone**

# Copyright

    This file is part of MuKeyboard.
    Copyright (C) 2017  Daniele Barattieri di San Pietro

    MuHi is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    Foobar is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with MuHi.  If not, see <http://www.gnu.org/licenses/> or 
    <http://www.muhack.org/> for futher datails.


# Links
- [Github](https://github.com/MrMoDDoM/MuKeyboard)
- [V-Bus](https://www.obdev.at/products/vusb/index.html)
- [Arduino](https://www.arduino.cc/)
- [Keyboard.h](https://www.arduino.cc/en/Reference/Keyboard)
- [Arduino HID](https://www.arduino.cc/en/Reference/HID)
 