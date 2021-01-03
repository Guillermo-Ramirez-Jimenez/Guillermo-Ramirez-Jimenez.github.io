I guess if you're reading this you have a PSP (probably not a GO one) and have always wondered why you couldn't connect your fancy wireless bluetooth headphones or earphones to it. Well, I figured out a way to do it which involves some hardware modification.

One of the pros of this method is it can be easily applied to a big ammount of devices the same way. The main pro is that it requires soldering and, in the PSP specific case, removing the UMD drive just because of space constraints.

What you need:
PSP (obviously): any non-GO model should be fine. Looks like the GO model already works with its built-in bluetooth.
Bluetooth sound module: described below.
Transistor: 
Switch: any SPDT switch that you can fit inside the PSP should work.
Soldering equipment: soldering iron, soldering tin, wires, etc.
The bluetooth sound module:







As you can see, the device is quite simple. The big rounded button is to power it on, the switch on the side is for choosing between receiver (RX) and transmitter (TX) mode, the micro usb is the charging port and the mini jack is the audio input.

Once disassembled the following can be seen:





(poner fotos aqui)

This module has a not really common feature that allows it to work as a bluetooth emitter and send audio to (in theory) any bluetooth audio receiver. This is the main part of our mod so, what's left is connecting it to the PSP.

First, we need to remove some components that aren't needed so we can later connect it to the PSP and save some space too.

This device's powered by a standard lipo battery. To make the assembly simpler and save some space, we're going to remove it and connect the power pins to the PSP battery.

The mode switch can be removed and the TX pin is soldered to the central one so it's always in transmitter mode.

The usb is also removed as there is no need to charge any battery. In theory it could be used to charge the PSP one, but it's completely useless as it already has its own charging circuit. It could be removed, but it involves PCB trimming and I think it's worthless because of the time it'd take for the space you'd get.

Now that everything we don't need has been removed, we can connect the remaining to the PSP.

The connections are divided in two blocks: the power ones and the data ones. The power connections are just the PSP power pins to the battery pins in the bluetooth device. The data connections are the two sound channels from the PSP to the board's mini jack ones.

Where do we connect the cables to the PSP? To the audio mini jack. But these connectors actually have 4 pins: left channel, right channel, ground and detection. What is the detection pin? When you plug your headphones that pin is connected to ground so the device detects it and makes the adjustments needed. In some cases it can even detect the type of device connected depending on the impedance that shorts the pin to ground. In the PSP case, it can't detect what is attached, but we still need to short it whenever we switch on the bluetooth so the audio out is enabled and the speakers disabled.

To make the switch possible, we're using a standard n-channel mosfet which basically works as an isolated electronic switch in this configuration.

The final assembly can be seen in the diagram below:

(diagrama aquí)

What's left is soldering the components together. You can mount the mosfet and switch on a cheap prototype PCB as I did. After you've finished it, just glue it to the PSP with a glue gun. In my case I didn't care and removed the UMD drive, but you may be able to fit it somewhere while keeping it.

Done! Enjoy your bluetooth audio enabled PSP!

NOTE: I noticed the sound output is really weak so you need to turn the volume up to the max in the PSP or in the target device. I recommend the PSP to reduce the noise sent. It could be fixed replacing the PSP audio driver, the bluetooth module's amplifier or placing an additonal one in the middle, but I find it really tedious for the result we could get.