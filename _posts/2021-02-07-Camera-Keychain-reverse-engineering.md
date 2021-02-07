---
title: Camera Keychain reverse engineering (ONGOING)
author: Guille
tags: reverse_engineering camera embedded
---

Some days ago, I found a curious gadget long forgotten at home. It's a keychain, but holds a camera inside! There's a microSD card slot and a microphone too.

Unfortunately, the case was damaged and looks like some electronic components were lost. I've not tried to power it on, anyway.

Looking on the net I found it is known as one of the variants of the 808 Car Keys Micro Camera. There's more information on this site about the model I have: [Link](https://www.chucklohr.com/808/C10/index.html)

I disassembled it, analyzed and took some pictures:

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/IMG_20210201_005020.jpg "Top")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/IMG_20210201_005208.jpg "Top_alt")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/IMG_20210201_005035.jpg "Bottom")

If you look closely, it can be seen that the main chip is labeled as SQ907F-L. I could not find any particular information about it apart from being QFP-128 so, it's a black box. Actually, looks like the manufacturer is a company from Taiwan. The actual website gives no information about their products:
[SQ website](http://www.sq.com.tw/)

However, it's equivalent to the SQ907B and there's a backup of their website in the Web Archive which gives a bit of info about the chip's capabilities:
[SQ907B in old SQ website](http://web.archive.org/web/20111021050700/http://www.sq.com.tw/english/product/dsc100dw.htm)

Also, there's a SPI flash chip labeled as 25Q40T. Looking for it on the net reveals it's a 4Mbit SPI flash. I couldn't find the exact datasheet, but looks like the Winbond W25Q40BW is a compatible one. Here's the datasheet:
[W25Q40BW datasheet](https://www.winbond.com/resource-files/w25q40bw%20revf%20101113.pdf)

Let's make a memory dump! I'm not going to show how to do it in this article, maybe later in a separate one. I'll just say I used a CH341A module with the clip add-on.

TODO
