---
title: MuHack Badge
excert: Official MuHack Badge
tags:
 - MuHack
 - Badge
 - LEDs
 - ESP32
 - RP2040
 - PCB
category:
 - project
toc: false
---

> All the file and code for this project are available on the [Github Repo](https://github.com/MrMoDDoM/MuHack-Badge) 
{: .prompt-info }

![Startup Sequence](/assets/img/post/2022-09-20-MuHack-Badge-startup.gif)

The board features an RP2040 as main microcontroller running micropython, and a ESP32 as a coprocessor.
The RP2040 is used to control the LEDs and to stream data from the BHI160B sensor hub.

## Features
- RP2040 MCU
- ESP32 MCU
- BHI160B sensor hub
- 14 WS2812B RGB LEDs
- USB, I2C and UART interfaces
- WiFi/BT connectivity
- Two buttons and one buzzer

# BHI Sensor Hub
The BHI160B is a 6-axis sensor hub with a 3-axis gyroscope and a 3-axis accelerometer. It automagically fuses the data from the two sensors and it provides a quaternion and a rotation matrix as output. The BHI160B is also able to detect some gestures and it can be used to detect the orientation of the board. The BHI160B is connected to the RP2040 through I2C and it is used to stream data from the gyroscope and accelerometer.

![Magnetometer](/assets/img/post/2022-09-20-MuHack-Badge-rotate.gif)

TODOs:
 - [ ] WRITE THE DOCUMENTATION
 - [x] Connect the interrupt line of BHI to the RP2040
 - [x] Invert TX/RX of UART between ESP32 and RP2040
 - [x] Change to a bigger footprint of ESP32 debug port
 - [ ] Maybe add two button for boot sel and reset for the ESP32
 - [ ] Change button battery footprint
 - [ ] Improve silkscreen text size
 - [ ] Add silkscreen label for many things (pinout, battery polarity, etc)
 - [ ] Expose BHI's internal I2C and interrupt line (for future sensors)
 - [ ] Move I2C pull-up resistor
 - [ ] Add led polarity
 - [ ] Remove power inductor (or change to an available one)
 - [ ] Maybe add a way to switch LEDs data line to the ESP32 instead of the RP2040
 - [ ] Docs and code comments
 - [ ] Remove the D16 Schottky Diode from the battery line
 - [ ] Meybe add some more cap to prevent brown-out when disconnecting USB power
 - [ ] Add a method within BHY library to stop every sensor
 - [ ] Add NFC tag (maybe)
  
 Special Thanks to:
 @gcammisa and Paolino

# License
This project is licensed under the AGPL-3.0 License

# Links
- [Github Repo](https://github.com/MrMoDDoM/MuHack-Badge)