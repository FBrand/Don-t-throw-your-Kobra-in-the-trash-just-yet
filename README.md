# Don-t-throw-your-Kobra-in-the-trash-just-yet

If you feel similarly about your Anycubic Kobra and you are wondering if there are any parts worth salvaging or if you should just throw the whole thing in the trash and get a different Printer the following might help:

1. Delete the crippled Anycubic firmware.
2. Fix the broken stepper driver addressing by desoldering a 0-Ohm resistor and bridging the pins next to it.
3. Grab a Raspberry PI (2,3,4, zero2), old notebook, old pc or something else, that can run linux and install klipper onto it.
4. Flash klipper onto the printer.
5. Setup and tune Klipper.
6. Enjoy a printer that actually does what it is supposed to, prints models as expected by the designer and never bother with anycubic special solutions ever again (at least on the software side)


More details, instructions comming soon...


Details:

1. Delete the crippled Anycubic firmware.

There is actually nothing to do here, but to mentally prepare to never have to deal whith this ... again.


2. Fix the broken stepper driver addressing by desoldering a 0-Ohm resistor and bridging the pins next to it.

The mainboard uses 4 TMC stepper drivers all connected to the same UART pin and given different addresses: X->0, Y->1, Z->2, E0->3
For some reason instead of using another TMC2209 Clone (which supports this addressing) as E0 driver, Anycubic chose to use a TMC2208 clone which alsways responds to/as Address 0.
The easiest way to make the original Mainboard work with Klipper is to change the UART address for the X-driver from 0 to 3 by resoldering R65 to R66 as seen here: https://klipper.discourse.group/t/support-for-hdsc-chips-hc32f460/2860/54

I do not expect there to ever be a "software only modification" given how simple the harware fis is.


3. Grab a Raspberry PI (2,3,4, zero2), old notebook, old pc or something else, that can run linux and install klipper onto it.

While a Raspberry PI will probably be the most common and easy device to install klipper on, every computer you own should be able tu run Linux (eg. Ubuntu) and in turn Klipper. If anyone knows a good, foolproof tutorial to install klipper on a current standard linux distribution, hit me up and I will link it here.
I had an older spare Raspi lying around and went the easy way: https://docs.mainsail.xyz/setup/mainsail-os


4. Flash klipper onto the printer.

As described in the official Klipper documentation here: https://www.klipper3d.org/Installation.html#building-and-flashing-the-micro-controller

In the make menuconfig choose the following:
![image](https://user-images.githubusercontent.com/14114342/222974441-c8745ce8-fa16-4bbc-ad93-c18ae662d799.png)

For me the HC32F460 was not present at first, but appeared once I updated the system: 
    for the os execute: sudo apt update && sudo apt upgrade
    for klipper update from the Web-UI or execute e git pull in the ~/klipper directory

5. Setup and tune Klipper.

The configs I am using can be fond in the printer_data/config folder in this repo.
You are welcome to read through them and use them after you made the neccessary changes.
Before doing anython other than turning the printer on and checking, that klipper can successfully connect to your Mainboard go through the following:
https://www.klipper3d.org/Config_checks.html

Also calibrate your Z-Offset (https://www.klipper3d.org/Probe_Calibrate.html) so Your Printhead does not bump into the bed!!
It might be a good Idea to set [probe] -> z_offset to 0 before calibrating for your machine.

I also set up KAMP (https://github.com/kyleisah/Klipper-Adaptive-Meshing-Purging).
My config for this can be found in this repo as well.

6. Enjoy a printer that actually does what it is supposed to, prints models as expected by the designer and never bother with anycubic special solutions ever again (at least on the software side)

