---
title: Must have G-Code macros to Klipper with Artillery Sidewinder X2
date: 2022-10-08
description: A first set of G-Code macros you really need to have to run your Artillery Sidewinder X2 with Klipper. 
draft: true
tags: 
  - artillery
  - sidewinder
  - 3D printer
  - klipper
  - mainsail
  - macros
categories:
  - maker
---

Klipper has the powerful possibility to add G-Code macros or even overwrite existing G-Codes. Klipper knows only about standard G-Codes ([Described in Klipper documentation](https://www.klipper3d.org/G-Codes.html)) by default. You have to configure everything that goes beyond. Scenarios are filament runout, pause and resume print, filament change, cancel objects etc. In case of the Artillery Sidewinder X2, you can also add some macros what should happen with the neopixel LED on your extruder. 

The basic idea is often not mine. Instead, I have gathered examples from documentation and adapted them to the Sidewinder X2 and my needs.

- Configure start and End G-Code in your slicer