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

# Basic block diagram
We can divide the board in three main subsystems: the RP2040, the ESP32 and the BHI160B. The RP2040 is the main microcontroller of the board and it is used to control all the hardware stuff, like the LEDs, the buttons and the buzzer. It is also used to communicate with the BHI160B and the NFC.

The BHI160B takes care of the BMM150 and eventually optional sensors.

Optionally, it is possible to add an ESP32 and communicate with it through 5 GPIO pins (e.i. setting up an UART and interrupts lines).

Here a simplified block diagram of the board:

![Block Diagram](/assets/img/post/2022-09-20-MuHack-Badge-block_diagram.png)

# Getting started
One you have obtained a board, the first thing to do is to upload the MicroPython firmware.

Since it is used the same chip of the Raspberry Pi Pico, you can use the official firmware from the MicroPython website and follow the official guide to upload it.

https://micropython.org/download/RPI_PICO/

Once you have uploaded the firmware, you can connect to the board using a serial terminal (e.g. screen, minicom, etc) and start playing with it: now you have a fully functional MicroPython board!

The next step is to upload the BOSS system: the BOSS system is a small firmware written in MicroPython that runs on the RP2040 and it is used to setup the BHI160B and stream data from it. It also provides a simple CLI system to use the badge.

To upload the BOSS system, simply copy all the file within the ```Software/BOSS/``` folder to the board and reboot it. Don't forget to copy the ```BHY``` folder too! It contains the BHI160B driver and comes from [another repository](https://github.com/MrMoDDoM/BHY-Micropython-Driver).

You should now have a fully functional badge! You can use the serial console to interact with the BOSS system or you can use the headless mode to run some simple example applications.

![Badge rotate](/assets/img/post/2022-09-20-MuHack-Badge-rotate.gif)

## Chagelog v2

 - Added USB connector for the ESP32
 - Added NFC subsystem
 - Completely reworked the power system
 - Added more LEDs for battery status and esp32
 - Added a button for the ESP32
 - WIP: firmware update for better BHI support

## Chagelog v1
 - Initial release

TODOs:
 - [ ] WRITE THE DOCUMENTATION
 - [x] Connect the interrupt line of BHI to the RP2040
 - [x] Invert TX/RX of UART between ESP32 and RP2040
 - [x] Change to a bigger footprint of ESP32 debug port
 - [x] Maybe add two button for boot sel and reset for the ESP32
 - [ ] ~~Change button battery footprint~~ The button battery was removed
 - [x] Improve silkscreen text size
 - [x] Add silkscreen label for many things (pinout, battery polarity, etc)
 - [x] Expose BHI's internal I2C and interrupt line (for future sensors)
 - [x] Move I2C pull-up resistor
 - [x] Add led polarity
 - [x] Remove power inductor (or change to an available one)
 - [ ] ~~Maybe add a way to switch LEDs data line to the ESP32 instead of the RP2040~~ Changed my mind
 - [ ] Docs and code comments
 - [x] Remove the D16 Schottky Diode from the battery line
 - [x] Meybe add some more cap to prevent brown-out when disconnecting USB power
 - [ ] Add a method within BHY library to stop every sensor
 - [x] Add NFC tag (maybe)
  
 Special Thanks to:
 @gcammisa and Paolino

# License
This project is licensed under the AGPL-3.0 License

# Links
- [Github Repo](https://github.com/MrMoDDoM/MuHack-Badge)