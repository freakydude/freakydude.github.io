---
title: "Flash the Latest Marlin Firmware to the Artillery Sidewinder X2"
date: 2022-09-30
description: Step-by-step guide to flash my compiled Marlin firmware to the Artillery Sidewinder X2 (Artillery Ruby v1.2 mainboard) with Pronterface and STM32CubeProgrammer.
draft: false
tags:
  - artillery
  - sidewinder
  - 3d printer
  - marlin
categories:
  - maker
---

I show you how to flash the latest Marlin firmware to your Artillery Sidewinder X2. I had to compile it myself because Artillery has not updated it yet. At the time of writing, it's an adapted version of Marlin 2.1.2.

> **Caution:** No warranty, no support. Do it only if you know what you are doing. It's absolutely your own risk, and I'm not responsible for any problems or defects.

## Requirements

### Software

- [Pronterface/Printrun](http://www.pronterface.com/)
- [STM32CubeProgrammer](https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-programmers/stm32cubeprog.html#get-software) (you have to register to download it)
- [Customized Marlin Artillery Sidewinder X2 Firmware](https://github.com/freakyDude/Marlin/releases/)

## What you get

Marlin 2.x, a precompiled version for the Artillery Sidewinder X2 (Artillery Ruby v1.2 mainboard).

It's for everyone who has the new Artillery Sidewinder X2 with the new TFT (GD32F305). I compiled a minimal customized firmware for the Artillery Ruby mainboard (not for the TFT controller, as there is no known support yet).

Basically, I enabled some features, like Linear Advance or the OctoPrint support ([config and plugin suggestions](https://community.octoprint.org/t/a-list-of-recommended-marlin-features/39048)).
I didn't enable all the features that are relevant for the BigTreeTech TFT firmware, because the custom firmware can't be flashed on the recently shipped GD32F305 chipset.

I also left unsupported hardware features disabled. For details, see the Configuration.h and Configuration_adv.h.

## Flash the firmware

1. Install Pronterface
1. Install STM32CubeProgrammer
1. Power on your Artillery Sidewinder X2
1. Connect your PC/Mac/laptop with USB to your Sidewinder
1. Start Pronterface
   1. Connect to your printer with 115200 baud
   1. Type `M997` at the bottom right
   1. Error messages can be ignored
   1. If you are using Windows, look into your device manager to check whether there is a "STM32 Bootloader"
1. Disconnect Pronterface
1. Start STM32CubeProgrammer
   1. Connect with USB
   1. **Make a backup of your original firmware**
   1. Flash the modified firmware:
      1. Download the `firmware.bin` from the latest release on my [GitHub releases page](https://github.com/freakyDude/Marlin/releases/).
      1. In STM32CubeProgrammer, open the file, select the firmware, and press Download
1. Disconnect the USB cable from your printer and restart it (power off, then on)
1. Reset the EEPROM with `M502` and save the EEPROM settings with the `M500` command

## Set up the basic parameters after the reset

You reset your EEPROM (your printer configuration) to the firmware defaults. You should at least set up some basic parameters before you print your next wonderful thing.

- Your Z-probe offset (first layer, babysteps) with `M851 Z-[your new value]`
- Your extruder E-steps with `M92 E[your new value]`
- Your auto bed leveling: home with `G28`, probe with `G29`, and enable it afterwards with `M420 S1`

Don't forget to save your settings into your EEPROM again with `M500`.

## Conclusion

You're done. I hope I could help.

Special thanks to [PrEzi](https://www.thingiverse.com/PrEzi), whose guide helped me to understand this, and of course to all the people he already mentioned.

{{< support >}}
