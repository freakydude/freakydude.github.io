---
title: Use Armbian on your BigTreeTech CB1 board
date: 2023-10-21T00:00:00+01:00
description: Some steps to replace the outdated BigTreeTech cb1 linux image with the current armbian linux and also to enable network, CANBUS and your BigTreeTech TFT35 SPI.
draft: true
tags:
  - btt manta
  - armbian
  - bigtreetech
  - btt tft35 spi
  - canbus
  - touchscreen
  - 3D printer
  - klipper
  - mainsail
categories:
  - maker
  - developer
---

## Intro

This post is written in context of using the BigTreeTech CB1 as your klipper controller for your 3d printer. But most of the points here are also usable for other projects with the raspberry pi alternative. Its about cleaning up the network configuration, enable canbus if the cb1 is installed on a BigTreeTech manta port and last but not least to enable the bigtreetech tft35 spi to show your graphical environment. Here to be used with the well known klipperscreen project - but for whatever your project is about.

## Install basic armbian image

### Install on SD Card

### Install on internal EMMC storage

- [Some steps to replace the outdated BigTreeTech cb1 linux image with the current armbian linux and also enable network, canbus and your BigTreeTech TFT35 SPI.](https://github.com/bigtreetech/CB1?tab=readme-ov-file#cb1-emmc-version)

  - Download [sunxi-tools](https://github.com/bigtreetech/sunxi-tools)
  - For windows also download driver installer [zadig](https://zadig.akeo.ie/)
  - Download armbian Image [text](https://www.armbian.com/bigtreetech-cb1/). Suggest the current minimal/IOT version Bookworm Minimal

- Install driver
- Switch to external usb storage mode
- Flash armbian with balena etcher
- change dbo to emmc

## configure networkmanager

- connect via ethernet (ssh root@bigtreetech-cb1)
- sudo apt install network-manager
- sudo apt remove netplan.io (conflicting with systemd-networkd/networkmanager)
- remove /etc/systemd/network/
- nmcli device wifi connect `Your SSID` password `Your Password` ifname wlan0

## cleanup systemd-networkd

sudo systemctl disable systemd-networkd-wait-online.service
sudo systemctl disable NetworkManager-wait-online.service

## configure canbus

/etc/systemd/network/can0.link
-> has currently no effect, workaround with udev rules below

```systemd
[Match]
Type=can

[Link]

TransmitQueueLength=128
```

/etc/systemd/network/can0.network

```systemd
[Match]
Name=can0

[CAN]
BitRate=1000000
RestartSec=10s
```

### workaround

/etc/udev/rules.d/80_can.rules

klipper recommends tx_queue_len of 128

```sh
SUBSYSTEM=="net", ACTION=="add|change", KERNEL=="can*", ATTR{tx_queue_len}="128"
```

## configure tft graphics

/etc/X11/xorg.conf.d/99-tft35.conf

```xorg
Section "Device"
  Identifier      "FBDEV"
  Driver          ""
  Option          "fbdev" "/dev/fb0"
  Option          "SwapbuffersWait" "true"
EndSection
```

## disable systemd reboot watchdog

/etc/systemd/system.conf

```systemd
RebootWatchdogSec=off
```

## install klipper stack

manual or with kiauh
[Klipper Installation And Update Helper](https://github.com/dw-0/kiauh)

- step by step
- klipper
- moonraker
- mainsail
- klipperscreen

  - if asked for xorg or wayland use wayland
  - if asked for networkmanager, say yes

- optional
  - crownest (for cam streaming)
  - obico

## Conclusion

Please leave a comment if something is wrong or missing. Or if you have any questions.

Have fun!

If you like, buy me a coffee

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F2F7GC8PC)

freakyDude
