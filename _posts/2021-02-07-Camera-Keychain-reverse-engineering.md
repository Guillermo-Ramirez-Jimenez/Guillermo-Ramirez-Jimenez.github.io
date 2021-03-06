---
title: Camera Keychain reverse engineering
author: Guille
tags: reverse_engineering camera embedded Binwalk
---

Some days ago, I found a curious gadget long forgotten at home. It's a keychain, but holds a camera inside! There's a microSD card slot and a microphone too.

Unfortunately, the case was damaged and looks like some electronic components were lost. It doesn't power on, anyway.

I think it can be an interesting project to try to figure out as much as we can about how this little device works.

Looking on the net I found it is known as one of the variants of the 808 Car Keys Micro Camera. There's more information on this site about the model I have: [Link](https://www.chucklohr.com/808/C10/index.html)

I disassembled it, analyzed and took some pictures:

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/IMG_20210201_005020.jpg "Top")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/IMG_20210201_005208.jpg "Top_alt")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/IMG_20210201_005035.jpg "Bottom")

The first thing to note is the camera. It's a 24 pin CMOS camera. I haven't found any extra info about it for now.

If you look closely, it can be seen that the main chip is labeled as SQ907F-L. I could not find any particular information about it apart from being QFP-128 package so, it's a black box, but it's the CPU for sure. Actually, looks like the manufacturer is a company from Taiwan. The actual website gives no information about their products:
[SQ website](http://www.sq.com.tw/)

However, it's equivalent to the SQ907B and there's a backup of their website in the Web Archive which gives a bit of info about the chip's capabilities:
[SQ907B in old SQ website](http://web.archive.org/web/20111021050700/http://www.sq.com.tw/english/product/dsc100dw.htm)

There's a section about the development kits offered by the company for other chips. According to it, they're x86 chips programmed in C++ running an RTOS. We may be lucky if this chip is similar in some way.

Additionally, there's a chip labeled as Hynix 401A HY57V161610FTP- 8. This chip is a 1Mx16bit SDRAM. As it's a RAM, there's no info inside it.

Also, there's a SPI flash chip labeled as 25Q40T. These files usually store the device firmware. The firmware is the code that makes the CPU work as we want. Looking for it on the net reveals it's a 4Mbit SPI flash. I couldn't find the exact datasheet, but looks like the Winbond W25Q40BW is a compatible one. Here's the datasheet:
[W25Q40BW datasheet](https://www.winbond.com/resource-files/w25q40bw%20revf%20101113.pdf)

Let's make a memory dump! I'm not going to show how to do it in this article, maybe later in a separate one. I'll just say I used a CH341A module with the clip add-on.

The result is a 512KB file. Let's start having a look at it in a hex editor (BTW, I'm using HxD). I'll do a wild guess and suppose it uses U-Boot as a bootloader. The bootloader is the first piece of code that is loaded in memory and its purpose is to initialize and facilitate the start of the following programs. U-Boot is an open source bootloader that's been broadly adopted for embedded projects because of its high portability.

Let's look for a "boot" string in the file.

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/BootStringScreenshot.png "Boot String")

Not what I expected. However, there are many "SQBOOT" matches. Pay attention to the offsets, the space between matches is 0x10000, which corresponds to 64KB. Going back to the SPI flash datasheet, it can be seen that it's divided in 64KB blocks. It doesn't look like a coincidence.

I wrote a simple Python script which divides the file according to the offsets specified in a separate file. The result is 4 files of 64KB and 1 of 256KB. The last file is the remaining memory, which is blank. Now, it's important to note that a blank memory is full of ones, which means all the values are 0xFF.

Let's have a look at the other files in Binwalk. Binwalk is an open source program designed to identify relevant parts of binary firmware files. The initial results are, in order:

```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
42620         0xA67C          JPEG image data, EXIF standard
42635         0xA68B          TIFF image data, big-endian, offset of first image directory: 136
```
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
42620         0xA67C          JPEG image data, EXIF standard
42635         0xA68B          TIFF image data, big-endian, offset of first image directory: 136
```
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
42620         0xA67C          JPEG image data, EXIF standard
42635         0xA68B          TIFF image data, big-endian, offset of first image directory: 136
```
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
6795          0x1A8B          JPEG image data, JFIF standard 1.02
6825          0x1AA9          TIFF image data, little-endian offset of first image directory: 8
7127          0x1BD7          JPEG image data, JFIF standard 1.02
11700         0x2DB4          JPEG image data, JFIF standard 1.02
21821         0x553D          Copyright string: "Copyright (c) 1998 Hewlett-Packard Company"
42620         0xA67C          JPEG image data, EXIF standard
42635         0xA68B          TIFF image data, big-endian, offset of first image directory: 136
```

So, the first 3 parts look really similar, while the last one is a bit more particular. Binwalk says that one contains a copyright string. Having a closer look reveals more text actually, but let's take it easy and check something else before.

Let's have a look at the entropy. The entropy measures the randomness of a file. It's important because it can be helpful to notice if a file presents some type of encryption. When a file is encrypted, the intention is to make it as homogeneous as possible so, it is difficult to extract any information from it. Therefore, if the entropy tends to a regular value close to 1, we can conclude the file is encrypted. Let's see.

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/0_entropy.png "Entropy 0")
![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/1_entropy.png "Entropy 1")
![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/2_entropy.png "Entropy 2")
![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-02-07-Camera-Keychain-reverse-engineering/images/3_entropy.png "Entropy 3")

Looks good. I think we can safely say there's no encryption at all. Actually, there're some empty spaces in those files, where the entropy is 0. Also, looks like there's a common pattern between the addresses 48000 and 52000 in all files.

Going back to the Binwalk results, looks like there're some pictures that can be extracted. They may be false positives so, we have to compare them with the JPEG and TIFF structures first. I found this image about the JPEG encoding which can be helpful (credits to Ange Albertini):

![alt text](https://raw.githubusercontent.com/corkami/pics/master/binary/JPG.png "JPEG structure") 

What matters to us is that a JPEG file starts with a 0xFFD8 sequence and ends in a 0xFFD9 one. Looking for them in a hex editor reveals that only the starting sequence is found, and then the EXIF string as Binwalk said. I'm not sure about the meaning of this. Is it a file template? Why is there no ending? We could dig a bit deeper, but I think it can result in a dead end. Let's try to focus on the OS.

Also, we still have no idea about the CPU architecture. The CPU architecture is the way a CPU is designed, which involves a lot of different aspects so, I'm not going to explain more than this. I tried to use Binwalk to identify the type of present opcodes but it is unable to find anything. This means either the code is not executable so, it's a database, or the CPU architecture is not supported by Binwalk, which implies is not a common one. The opcodes are the instructions that a CPU executes. A program written with these opcodes is called machine code.

I think this is enough for today so let's leave it here. A more advanced tool is needed: Ghidra, but that's a project for another day.

[Files in GitHub](https://github.com/Guillermo-Ramirez-Jimenez/Keychain-Camera)
