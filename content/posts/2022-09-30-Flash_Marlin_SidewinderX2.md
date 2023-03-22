---
title: Flash latest Marlin firmware to Artillery Sidewinder X2
date: 2022-09-30
description: Step by step guide to flash the my compiled Marlin firmware to the Artillery Sidewinder X2 printer
draft: false
tags:
  - artillery
  - sidewinder
  - 3D printer
  - marlin
categories:
  - maker
---

I show you how to flash the latest Marlin Firmware to your Artillery Sidewinder X2. I had to compile it myself, because Artillery did not update it yet. At time of writing, it's an adapted version of Marlin 2.1.2.

**Caution: No warranty, no support, do it only - if you know what you are doing, it's absolutely your own risk. I'm not responsible for any problems/defects...**

## What you got

Marlin 2.x, a precompiled Version for Artillery Sidewinder X2 (Artillery_Ruby v1.2 mainboard).

It's for all who have the new Artillery Sidewinder X2 with the new TFT (GD32F305). I compiled a minimal customized firmware for the Artillery Ruby mainboard (not for the TFT controller, there is no known support yet).

Basically I enabled some features (like Linear Advance or the Octoprint support ([Config & Plugin suggestions](https://community.octoprint.org/t/a-list-of-recommended-marlin-features/39048)).
I didn't enable all the features, which are relevant for the Bigtreetech TFT Firmware, because the custom firmware can't be flashed in case of the recently shipped GD32F305 chipset.

I also left unsupported hardware features disabled. For details, see the Configuration.h and Configuration_adv.h.

## One way to flash

### Prerequisites - Tools you need

- [Pronterface/Printrun](http://www.pronterface.com/)
- [STM32CubeProgrammer](https://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-programmers/stm32cubeprog.html#get-software) (you have to register for download)
- [Customized Marlin Artillery Sidewinder X2 Firmware](https://github.com/freakyDude/Marlin/releases/)

### Prepare and Flash

1. Install Pronterface
1. Install STM32CubeProgrammer
1. PowerOn your Artillery Sidewinder X2
1. Connect your PC/Mac/Laptop with USB to your Sidewinder
1. Start Pronterface
   1. Connect with 115200 Baud to your Printer
   1. Type `M997` at bottom right
   1. Error messages can be ignored
   1. If you using Windows, look into your device manager, if there is a "STM32 Bootloader"
1. Disconnect Pronterface
1. Start STM32CubeProgrammer
   1. Connect with USB
   1. **Make a backup of your original firmware**
   1. Flash the modified Firmware:
      1. Download the `firmware.bin` form the latest release from my github page [https://github.com/freakydude/Marlin/releases/](https://github.com/freakyDude/Marlin/releases/).
      1. In STM32CubeProgrammer, Open File, Select Firmware, Press Download
1. Disconnect USB cable from your printer, restart (power-off, on) your printer
1. Reset EEPROM with `M502` and save EEPROM settings with `M500` command

### Basic setup after reset

You reset your EEPROM (your printer configuration) to firmware defaults. You should setup some basic parameters at least, before you print your next wonderful thing.

- Your Z-Probe Offset (First-Layer, Babysteps) with `M851 Z-[your new value]`
- Your Extruder E-Steps with `M92 E[your new value]`
- Your Auto-Bed-Leveling. Home with `G28`, probe with `G29`, enable it with `M420 S1` afterwards

Don't forget to save your settings into your EEPROM again with `M500`

**You`re done. Hope I could help. Have fun, leave comments.**

_And special thanks to [PrEzi](https://www.thingiverse.com/PrEzi), who's guide helped me to understand and of course all the guys he mentioned already._

If you like, buy me a coffee [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F2F7GC8PC)

freakyDude
