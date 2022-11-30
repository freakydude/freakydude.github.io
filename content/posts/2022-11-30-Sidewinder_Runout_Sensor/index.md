---
title: Use stock filament runout-sensor with Klipper3d
date: 2022-11-30
description: The post shows you how you can re-enable the stock Artillery Sidewinder X2 filament-runout sensor with the Klipper3D printer firmware. 
draft: true
tags: 
  - artillery
  - sidewinder
  - 3D printer
  - klipper
  - mainsail
categories:
  - maker
---

## Short Version

- Connect your filament runout-sensor to the z-endstop pins
- Configure a Klipper3D filament switch sensor section with the z-endstop pin (PA0)

## Long Version

The stock filament runout-sensor is connected to the TFT Display controller board. This is the reason why neither the Marlin firmware nor Klipper recognized it by default. For both firmware you have to connect the sensor to the Artillery Ruby mainboard instead of the TFT controller board.

There are some minor problems to solve:

1. Different voltage (3.3V vs 5V)
1. Connect wires to free usable pins
1. Configure Klipper

### Different voltage

The pins of the artillery ruby mainboard have either 5V or 24V. But the filament runout-sensor is connected to the TFT controller at a 3.3V pin.

Sadly no specs about the runout-sensor are around. So nobody knows, if 5V would be to much for it. But after some investigation, all similar sensors have a voltage range of 3~5V. So I tried it and it worked. But of course it's your own decision and risk.

### Connect wires

