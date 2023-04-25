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

> All the file and code for this project are available on the [Github Repo](https://github.com/MrMoDDoM/MuKeyboard) 
{: .prompt-info }

**With this library is possible to emulate an HID keyboard on every Arduino board, even without the HID interface.
It's completly compatible with every sketch written for the standard Keyboard.h library**

It's necessary to use a small circuit to interface with the USB cable:

![Schematics](/assets/img/post/2017-05-20-MuKeyboard-schematic.jpg)
_Image courtesy of http://www.practicalarduino.com_

# Installation
Simply ``` git clone https://github.com/MrMoDDoM/MuKeyboard.git ``` directly into your Arduino's library folder & compile!

> This project is still under beta, please expect a lot of changes.
{: .prompt-warning }

# Disclaimer
In this project is present code from the Arduino INC and from Objective Development Software GmbH, covered with different type of license: probably I have made some mistakes using and re-covering code with GNU/GPL v3, **my intentions were not to steal work from anyone**

# License
Released under GNU/GPL v3 by Daniele Barattieri di San Pietro

# Links
- [Github Repo](https://github.com/MrMoDDoM/MuKeyboard)
- [V-Bus](https://www.obdev.at/products/vusb/index.html)
- [Arduino](https://www.arduino.cc/)
- [Keyboard.h](https://www.arduino.cc/en/Reference/Keyboard)
- [Arduino HID](https://www.arduino.cc/en/Reference/HID)
 