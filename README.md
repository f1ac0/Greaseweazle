# Greaseweazle
The Greaseweazle project by Keir Fraser is a USB floppy drive interface and software tool to read and write disk images to and from real floppies. It works at a low level so it can deal with formats that are unknown to the standard PC floppy disk controller.
https://github.com/keirf/Greaseweazle

The project in this repository is just a recrated "F1" board making use of the bare chips instead of a bluepill module, and designed to plug directly into a Shugart drive.
- It is small : it fits inside the 3.5" drive height and the ribbon width
- It is equipped with the female 34 pin connector to conveniently plug it in the drive, without a ribbon cable
- It connects to the PC with a micro USB cable
- It has a pass-through header for powering the drive (5v only). An external 5v/12v PSU is required only for drives requiring 12v or power hungry ones.
- It uses SMD stuff (maybe harder to solder)
- It is designed using the KiCAD EDA
- It is updated to support the DISKCHANGE signal for the (actually exeprimental) WinUAE floppy support by RobSmith
- It does not have the "F7" features like double drive support

I guess it is not better than the original boards, just different for those who prefer tiny stuff.

For the software part, it works with the original "F1" firmware and tool, but for the time being small modifications are required to use the DISKCHANGE feature with WinUAE.

# Disclaimer
This is a hobbyist project, it comes with no warranty and no support.

This version is not endorsed by the original author, please don't bother him with issues you might have because of it.

I publish this work using the same license as the original : The Unlicense.

If you find it useful, please reward the original author.

# BOM
- 1x STM32F103C8T6 or CS32F103C8T6
- 1x 3.3v LDO, either SPX3819M5-L-3-3 or XC6206P332MR
- 1x 8MHz HC-49 crystal
- 2x 1uF 0805 capacitors
- 3x 100nF 0805 capacitors
- 2x 22pf 0805 capacitors
- 2x 22 ohm 0805 resistors
- 1x 1K 0805 resistor
- 1x 10K 0805 resistor
- 1x 100K 0805 resistor (to reduce the BOM you can use a 10k)
- 1x 1K 0603x4 resistor array
- 1x optional 0805 LED and 0805 resistor (330 ohm or more)
- 1x Micro USB Female Socket B type
- 1x 2x17 2.54mm pin socket
- 1x FDD power cable

# Making it
Check for shorts at least between 5V, 3.3V, and GND traces before applying power !

The programming port does not need to be soldered since it needs to be programmed just once : you can just hold it in place during the few seconds required for programming.

For the software part, it works with the original "F1" firmware and tool with ID pads left open. For the time being a modification is required to use the DISKCHANGE feature with WinUAE since it needs to be detected as a "Plus" version : in src/mcu/stm32f1/board.c, replace the F1SM\_plus \_board\_config with the values from the F1SM\_basic one, then after flashing close the ID0(1) pads.

To program the uC, I personally use a STLINK clone dongle with the stlink tool : https://github.com/stlink-org/stlink
```
st-flash --format ihex write Greaseweazle-F1-v0.??.hex
```
To update through the USB cable, bridge CK and GND on the programming port before connecting the USB cable, then enter
```
Greaseweazle/gw update
```

# Using it
- Plug it to the 34 pin connector of the drive
- If your drive is 5v only, plug the power cord from the device to the power plug of the drive
- If the drive needs 12v, use an external power supply
- Plug the micro usb cable to the device
- Then install it and use it like an original Greaseweazle device : https://github.com/keirf/Greaseweazle/wiki/Software-Installation
```
Greaseweazle/gw read mydisk.scp
Greaseweazle/gw write mydisk.scp
```

With winUAE : http://amiga.robsmithdev.co.uk/winuae

