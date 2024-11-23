---
title: MuHack SAO 10th annyversary
excert: I made a little SAO pin for the 10th anniversary of the MuHack hackerspace
tags:
 - Anniversay
 - MuHack
 - Pin
 - SAO
 - LEDs
 - I2C
 - PCB
category:
 - project
toc: false
pin: true
---

# WIP - Expect some changes

> All the file for this project are available on the [Github Repo](https://github.com/MrMoDDoM/MuHack_SAO_10th/) 
{: .prompt-info }

For the 10th anniversary of the MuHack hackerspace, I made a little SAO that can also be pinned to your shirt or backpack, working off a button battery.

![MuTag v2](/assets/img/post/2024-10-23-MuHack-SAO-10th-all-colors.jpg)

## Features
- Two configuration: SAO and pin
- Two led mounted on a transparent parte of the PCB
- I2C interface for connecting a little SPI flash memory
- Gold plated for a neat design
- Clasp Butterfly Pin

# Getting Started

This PCB is designed to be used in two different configurations: as a pin or as a SAO.

The pin configuration is the simplest one, with only two LEDs and a switch to turn them on and off, everything powered by a CR1220 battery.

There is also a solderable zone for adding a Clasp Butterfly Pin to the PCB, so you can wear it on your shirt or backpack.

![MuTag v2](/assets/img/post/2024-10-23-MuHack-SAO-10th-back.jpg)


## BOM for PIN configuration
- 2x 0805 LEDs
- 1x CR1220 battery holder (like the Keystone 3002)
- 1x CR1220 battery
- 1x 0805 1k resistor
- 1x Switch for turning on/off the LEDs (like the C&K PCM12SMTR)

## BOM for SAO configuration
- 2x 0805 10k resistor
- 1x SPI SOIC-8 flash memory (like the Winbond W25Q32JVSSIM)
- (optional) Everything listed in the PIN configuration BOM except the battery and the holder

> Do not use the SAO configuration with the battery inserted, it could damage the circuit or the connected Badge!
{: .prompt-warning }