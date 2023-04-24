---
title: BHY Micropython Driver
excert: Driver written in Micropython for the BHY Sensor
tags:
 - MuHack
 - Badge
 - BHY
 - Micropython
 - Sensor
 - PCB
category:
 - project
toc: false
---

# BHY-Micropython-Driver
Micropython porting of the BOSH BHY driver 

This module was created for the MuHack Badge, tested only on a BHI160B chip.
The included RAM patch are only for the BHI160B.
Even if a decent amount of debugging and testing have been done, expect bugs and crashes.

# Know bug:
- The FIFO reading procedure is slow and does not avoid large data buffer, causing the system to crash for RAM exaustion. Try to be as fast as possible to work on that data, expecially with high sensor frequency (below 50Hz should be fine)
- The flush FIFO request does not work properly, causing the system to crash for RAM exaustion. Try to be as fast as possible to work on that data, expecially with high sensor frequency (below 50Hz should be fine)

# License
This project is licensed under the AGPL-3.0 License

# Links
- [Github](https://github.com/MrMoDDoM/BHY-Micropython-Driver)
- [BHI160B Datasheet](https://www.bosch-sensortec.com/media/boschsensortec/downloads/datasheets/bst-bhi160b-ds000.pdf)
- [BHI160B Application Note](https://www.bosch-sensortec.com/media/boschsensortec/downloads/application_notes_1/bst-bhi160b-an000.pdf)
