---
title: PSP BT sound mod
author: Guille
tags: PSP bluetooth BT mod
---

I guess if you're reading this you probably have a PSP (probably not a GO one) and have always wondered why you couldn't connect your fancy wireless bluetooth headphones or earphones to it. Well, I figured out a way to do it which involves some hardware modification.

One of the pros of this method is it can be easily applied to a wide ammount of devices the same way. The main con is that it requires soldering and, in the PSP specific case, removing the UMD drive because of space constraints.

What you need:
* PSP (obviously): any non-GO model should be fine. Looks like the GO model already works with its built-in bluetooth.
* Bluetooth sound module: described below.
* Transistor: any N-channel enhancement mode mosfet should work. It's prefered over a BJT beacuse of efficiency.
* Switch: any SPDT switch that you can fit inside the PSP should work.
* Soldering equipment: soldering iron, soldering tin, wires, etc.

The bluetooth sound module:

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-01-12-PSP-BT-sound-mod/images/IMG_20190302_202052.jpg "BT module")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-01-12-PSP-BT-sound-mod/images/IMG_20190302_202224.jpg "BT module side")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-01-12-PSP-BT-sound-mod/images/IMG_20190302_202437.jpg "BT module top")

As you can see, the device is quite simple. The big rounded button is to power it on, the switch on the side is for mode, choosing between receiver (RX) and transmitter (TX), the micro usb is the charging port and the mini jack is the audio input/output.

Once disassembled the following can be seen:

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-01-12-PSP-BT-sound-mod/images/IMG_20190302_234749.jpg "BT module disassembled top")

![alt text](https://github.com/Guillermo-Ramirez-Jimenez/Guillermo-Ramirez-Jimenez.github.io/raw/main/_posts/2021-01-12-PSP-BT-sound-mod/images/IMG_20190302_234844.jpg "BT module disassembled bottom")

This module has a not really common feature that allows it to work as a bluetooth emitter and send audio to (in theory) any bluetooth audio receiver.

First, we need to remove some components that aren't needed so we can connect it later to the PSP and save some space and weight too.

This device's powered by a standard lipo battery. To make the assembly simpler, we're going to remove it and connect the power pins to the PSP battery.

The mode switch can be removed if the TX pin is soldered to the central one so it's always in transmitter mode (TX).

The usb is also removed as there is no need to charge any battery. In theory you could charge the PSP battery with it, but it's redundant as the PSP already has its own battery charging ICs. If you've enough ability and patience, you can try to trim the PCB removing the charging ICs to save more space and weight, and possibly enabling the module to be placed inside the PSP without removing the UMD drive.

Now that everything we don't need has been removed, we can connect the remaining to the PSP.

The connections are divided in two blocks: the power ones and the data ones. The power connections are just the PSP power pins to the battery pins in the bluetooth device. The data connections are the two sound channels from the PSP to the board's mini jack ones.

Where do we connect the cables to the PSP? To the audio mini jack. But these connectors actually have 4 pins: left channel, right channel, ground and detection. What is the detection pin? When you plug your headphones, that pin is connected to ground so the device detects it and makes the adjustments needed. In some cases it can even detect the type of device connected, afaik, not the PSP case. Still, we've to change it to disable the speakers.

So, to sum up, we need to switch on the device by driving power into it and enable the audio out by pulling the detection pin to ground. Both are done with a single switch at the same time.

For the detection pin switching we're using a 2N7000, a standard N-channel mosfet which is controlled by the same switch as the detection pin. The power switching is handled by the switch itself.

The final assembly can be seen in the diagram below:





What's left is soldering the components together. You can mount the 2N7000 and switch on a cheap prototype PCB as I did. After you've finished it, just glue it to the PSP with a glue gun, adhesive tape or similar. It's important to note that installing it involves disassembling and reassembling the majority of the PSP, which I'll not cover here.

Done! Enjoy your bluetooth audio enabled PSP!

An extension of this project could be a circuit that enables the BT by pressing certain PSP buttons at the same time rather than using an additional switch. It could be easily done with a MCU like an Arduino, which could be used for other additional tasks, or with some logic circuits.

NOTE: I noticed the sound output is really weak so you need to turn the volume up to the max in the PSP or in the target device. I recommend the PSP to reduce the noise sent. It could be fixed replacing the PSP audio driver, the bluetooth module's amplifier or placing an additonal one in the middle, but that's something for another day.
