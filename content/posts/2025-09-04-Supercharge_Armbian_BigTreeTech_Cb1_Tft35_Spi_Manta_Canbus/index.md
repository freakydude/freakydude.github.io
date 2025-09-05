---
title: "Supercharge Your 3D Printer: Modern Armbian Setup for BTT CB1/Manta M8P with CAN Bus"
date: 2025-09-04T00:00:00+01:00
description: Replace BTT's outdated Linux with Armbian, enable CAN Bus networking, and unlock your TFT35 SPI display's full potential for Klipper-based 3D printing.
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

The BigTreeTech CB1 is a powerful Raspberry Pi alternative for 3D printing enthusiasts, but its stock software often lags behind. By installing Armbian - a lightweight, optimized Linux distribution - you'll gain access to newer packages, better hardware support, and streamlined updates. This guide not only modernizes your CB1 but also unlocks advanced features:

1. **CAN Bus Support**: Reduce wiring complexity for multi-tool printers
2. **TFT35 SPI Integration**: Proper touchscreen functionality
3. **NetworkManager**: Simplified Wi-Fi/Ethernet management
4. **Klipper Optimization**: Prepare for fluid printing operations

While focused on 3D printing, these techniques apply to any CB1/Manta M8P project.

## Install basic armbian image

### Preparation

1. Download:
   - [Armbian Bookworm Minimal](https://www.armbian.com/bigtreetech-cb1/) (IoT optimized)
   - [Balena Etcher](https://www.balena.io/etcher/) (cross-platform flashing tool)

#### SD Card Installation

1. Insert ≥8GB SD card
2. Flash Armbian with Balena-Etcher

#### EMMC Installation

Some steps to replace the outdated BigTreeTech cb1 linux image with the current armbian linux and also enable network, canbus and your BigTreeTech TFT35 SPI.

[BTT Flash Documentation](https://github.com/bigtreetech/CB1?tab=readme-ov-file#cb1-emmc-version)

- Download [sunxi-tools](https://github.com/bigtreetech/sunxi-tools)
- For windows also download driver installer [zadig](https://zadig.akeo.ie/)
- Download armbian Image [text](https://www.armbian.com/bigtreetech-cb1/). Suggest the current minimal/IOT version Bookworm Minimal

- Install driver
- Switch to external usb storage mode
- Flash armbian with balena etcher
- change dbo to emmc

### Configure NetworkManager

Replace restrictive netplan with user-friendly networking:

1. Connect via Ethernet:

   ```bash
   ssh root@bigtreetech-cb1 # Default password: 1234
   ```

2. Update system & install NetworkManager:

   ```bash
   apt update && apt upgrade -y
   apt install network-manager -y
   apt purge netplan.io -y
   rm -rf /etc/systemd/network/*
   ```

3. Connect to Wi-Fi

   ```bash

   nmcli device wifi connect "SSID" password "PASSWORD" ifname wlan0
   nmcli connection modify "SSID" connection.autoconnect yes
   ```

4. Prevent boot delays from network services:

   ```bash
   sudo systemctl disable systemd-networkd-wait-online.service
   sudo systemctl disable NetworkManager-wait-online.service
   ```

### Configure CAN Bus

Enable high-speed tool communication:

/etc/systemd/network/can0.link

_Seems like this file has currently no effect on some systems. There is a workaround with udev rules below._

```systemd
[Match]
Type=can

[Link]
TransmitQueueLength=128
```

1. Create /etc/systemd/network/can0.network

   ```systemd
   [Match]
   Name=can0

   [CAN]
   BitRate=1000000
   RestartSec=10s
   ```

2. Create /etc/systemd/network/can0.link
   _The link file has currently no effect, workaround with udev rules below_

   ```systemd
   [Match]

   Type=can

   [Link]
   TransmitQueueLength=128
   ```

   **Workaround** Add udev rule /etc/udev/rules.d/80-can.rules:

   ```sh
   SUBSYSTEM=="net", ACTION=="add|change", KERNEL=="can*", ATTR{tx_queue_len}="128"
   ```

### TFT35 SPI Display Setup

/etc/X11/xorg.conf.d/99-tft35.conf

```xorg
Section "Device"
  Identifier      "FBDEV"
  Driver          ""
  Option          "fbdev" "/dev/fb0"
  Option          "SwapbuffersWait" "true"
EndSection
```

### Disable Reboot Watchdog

Edit `/etc/systemd/system.conf`

```systemd
RebootWatchdogSec=off
```

## Install Klipper Stack

Automated setup with KIAUH:

1. Download and install

   ```bash
   cd ~
   git clone https://github.com/dw-0/kiauh.git
   cd kiauh && ./kiauh.sh
   ```

2. Recommended selections:

   - Klipper: Latest version
   - Moonraker: API server
   - Mainsail: Modern web interface
   - KlipperScreen: Touchscreen UI (Choose Wayland backend)

   1. Optional Add-ons:
      - Crowsnest: Camera streaming
      - Obico: Remote monitoring and KI failure detection

Please leave a comment if there is something wrong or missing. Or if you have any questions.
Have fun!

If you enjoyed this article and would like to support my work, feel free to buy me a coffee! Your support helps me continue creating content. Thank you! [![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F2F7GC8PC)
