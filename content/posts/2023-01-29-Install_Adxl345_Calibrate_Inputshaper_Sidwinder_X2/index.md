---
title: Install the ADXL 345 accelerometer on Artillery Sidewinder/Genius and calibrate Klipper Resonance Compensation
date: 2023-01-29
description:
draft: true
tags:
    - artillery
    - sidewinder
    - genius
    - adxl345
    - 3D printer
    - klipper
    - mainsail
categories:
    - maker
---

## Requirements

-   Pin Cables
-   ADXL345
-   ADXL345 Lever adapter
-   RPI

## Connect to Rasberry PI

## Build Klipper MCU for PI

### Software installation

[https://www.klipper3d.org/Measuring_Resonances.html#software-installation](https://www.klipper3d.org/Measuring_Resonances.html#software-installation)

Login to your mainsail os PI

Make sure, needed packages are installed.

```sh
sudo apt update
sudo apt install python3-numpy python3-matplotlib libatlas-base-dev
```

```sh
~/klippy-env/bin/pip install -v numpy
```

### Build secondary MCU for PI

[https://www.klipper3d.org/RPi_microcontroller.html](https://www.klipper3d.org/RPi_microcontroller.html)

```sh
cd ~/klipper/
sudo cp ./scripts/klipper-mcu.service /etc/systemd/system/
sudo systemctl enable klipper-mcu.service
```

To compile the Klipper micro-controller code, start by configuring it for the "Linux process":

```sh
cd ~/klipper/
make menuconfig
```

In the menu, set "Microcontroller Architecture" to "Linux process," then save and exit.

To build and install the new micro-controller code, run:

```sh
sudo service klipper stop
make flash
sudo service klipper start
```

Make sure the Linux SPI driver is enabled by running sudo raspi-config and enabling SPI under the "Interfacing options" menu.

```yml
[mcu rpi]
serial: /tmp/klipper_host_mcu

[adxl345]
cs_pin: rpi:None
axes_map: -z, x, -y # Depends on mounting direction.

[resonance_tester]
accel_chip: adxl345
probe_points:
    150, 150, 20 # mid of your bed (Sidewinder size: 300x300mm)
```

Restart PI

### Test

G-Code Console

```
ACCELEROMETER_QUERY
```

Output

```
accelerometer values (x, y, z): 536.266849, -888.247130, 9918.759623
```

next

```
MEASURE_AXES_NOISE
```

```
Axes noise for xy-axis accelerometer: 24.348964 (x), 13.247702 (y), 13.386232 (z)
```

Not more than 100

Problems: [https://www.klipper3d.org/Measuring_Resonances.html#checking-the-setup](https://www.klipper3d.org/Measuring_Resonances.html#checking-the-setup)

### Manual Test X- and Y-Axes

```
TEST_RESONANCES AXIS=X
```

```
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o ~/printer_data/config/shaper_calibrate_x.png

Fitted shaper 'zv' frequency = 55.0 Hz (vibrations = 24.0%, smoothing ~= 0.057)
To avoid too much smoothing with 'zv', suggested max_accel <= 11800 mm/sec^2
Fitted shaper 'mzv' frequency = 49.2 Hz (vibrations = 0.0%, smoothing ~= 0.084)
To avoid too much smoothing with 'mzv', suggested max_accel <= 7100 mm/sec^2
Fitted shaper 'ei' frequency = 63.6 Hz (vibrations = 1.7%, smoothing ~= 0.080)
To avoid too much smoothing with 'ei', suggested max_accel <= 7500 mm/sec^2
Fitted shaper '2hump_ei' frequency = 73.0 Hz (vibrations = 0.0%, smoothing ~= 0.101)
To avoid too much smoothing with '2hump_ei', suggested max_accel <= 5900 mm/sec^2
Fitted shaper '3hump_ei' frequency = 87.4 Hz (vibrations = 0.0%, smoothing ~= 0.107)
To avoid too much smoothing with '3hump_ei', suggested max_accel <= 5600 mm/sec^2
Recommended shaper is mzv @ 49.2 Hz
```

```
TEST_RESONANCES AXIS=Y
```

```
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o ~/printer_data/config/shaper_calibrate_y.png

Fitted shaper 'zv' frequency = 48.6 Hz (vibrations = 3.9%, smoothing ~= 0.071)
To avoid too much smoothing with 'zv', suggested max_accel <= 9200 mm/sec^2
Fitted shaper 'mzv' frequency = 46.6 Hz (vibrations = 1.0%, smoothing ~= 0.094)
To avoid too much smoothing with 'mzv', suggested max_accel <= 6400 mm/sec^2
Fitted shaper 'ei' frequency = 52.0 Hz (vibrations = 0.0%, smoothing ~= 0.119)
To avoid too much smoothing with 'ei', suggested max_accel <= 5000 mm/sec^2
Fitted shaper '2hump_ei' frequency = 66.6 Hz (vibrations = 0.0%, smoothing ~= 0.122)
To avoid too much smoothing with '2hump_ei', suggested max_accel <= 4900 mm/sec^2
Fitted shaper '3hump_ei' frequency = 82.0 Hz (vibrations = 0.0%, smoothing ~= 0.122)
To avoid too much smoothing with '3hump_ei', suggested max_accel <= 4900 mm/sec^2
Recommended shaper is mzv @ 46.6 Hz
```

### Auto Calibrate

[https://www.klipper3d.org/Measuring_Resonances.html#input-shaper-auto-calibration](https://www.klipper3d.org/Measuring_Resonances.html#input-shaper-auto-calibration)

```
SHAPER_CALIBRATE

or SHAPER_CALIBRATE AXIS=X or SHAPER_CALIBRATE AXIS=Y
```

-->

```
SAVE_CONFIG
```
