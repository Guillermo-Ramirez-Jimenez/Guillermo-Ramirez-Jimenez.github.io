---
title: Camera Keychain reverse engineering (ONGOING)
author: Guille
tags: reverse_engineering camera embedded
---

Some days ago, I found a curious gadget long forgotten at home. It's a keychain, but holds a camera inside! There's a microSD card slot and a microphone too.

Unfortunately, the case was damaged and looks like some electronic components were lost. I've not tried to power it on, anyway.

Looking on the net I found it is known as one of the variants of the 808 Car Keys Micro Camera. There's more information on this site about the model I have: [Link](https://www.chucklohr.com/808/C10/index.html)

I disassembled it, analyzed and took some pictures:

TODO

If you look closely, there is a SPI flash chip labeled as 25Q40T. Looking for it on the net reveals it's a 4Mbit SPI flash. I could not find the exact datasheet, but looks like the Winbond W25Q40BW is a compatible one. Here is the datasheet: [W25Q40BW datasheet](https://www.winbond.com/resource-files/w25q40bw%20revf%20101113.pdf)

Let's make a memory dump!

TODO
