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

- Pin Cables
- ADXL345
- ADXL345 Lever adapter
- RPI

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

Make sure the Linux SPI driver is enabled by running `sudo raspi-config` and enabling SPI under the "Interfacing options" menu.

```yml
[mcu rpi]
serial: /tmp/klipper_host_mcu

[adxl345]
cs_pin: rpi:None
axes_map: -y, x, z # Depends on mounting direction.

[resonance_tester]
accel_chip: adxl345
probe_points:
    150, 150, 20 # mid of your bed (Sidewinder size: 300x300mm)
```

Shutdown PI and remove from power

### Wiring ADXL345

[https://www.klipper3d.org/Measuring_Resonances.html?h=adxl#adxl345](https://www.klipper3d.org/Measuring_Resonances.html?h=adxl#adxl345)

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
Axes noise for xy-axis accelerometer: 11.789601 (x), 16.017559 (y), 29.190110 (z)
```

Not more than 100

Problems: [https://www.klipper3d.org/Measuring_Resonances.html#checking-the-setup](https://www.klipper3d.org/Measuring_Resonances.html#checking-the-setup)

### Manual Test X- and Y-Axes

```
TEST_RESONANCES AXIS=X
```

```
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o ~/printer_data/config/shaper_calibrate_x.png

Fitted shaper 'zv' frequency = 53.8 Hz (vibrations = 11.0%, smoothing ~= 0.060)
To avoid too much smoothing with 'zv', suggested max_accel <= 11300 mm/sec^2
Fitted shaper 'mzv' frequency = 55.0 Hz (vibrations = 0.0%, smoothing ~= 0.067)
To avoid too much smoothing with 'mzv', suggested max_accel <= 8900 mm/sec^2
Fitted shaper 'ei' frequency = 69.4 Hz (vibrations = 1.4%, smoothing ~= 0.067)
To avoid too much smoothing with 'ei', suggested max_accel <= 9000 mm/sec^2
Fitted shaper '2hump_ei' frequency = 81.8 Hz (vibrations = 0.0%, smoothing ~= 0.081)
To avoid too much smoothing with '2hump_ei', suggested max_accel <= 7400 mm/sec^2
Fitted shaper '3hump_ei' frequency = 98.4 Hz (vibrations = 0.0%, smoothing ~= 0.085)
To avoid too much smoothing with '3hump_ei', suggested max_accel <= 7100 mm/sec^2
Recommended shaper is mzv @ 55.0 Hz
```

```
TEST_RESONANCES AXIS=Y
```

```
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o ~/printer_data/config/shaper_calibrate_y.png

Fitted shaper 'zv' frequency = 39.6 Hz (vibrations = 18.1%, smoothing ~= 0.102)
To avoid too much smoothing with 'zv', suggested max_accel <= 6100 mm/sec^2
Fitted shaper 'mzv' frequency = 30.8 Hz (vibrations = 1.0%, smoothing ~= 0.215)
To avoid too much smoothing with 'mzv', suggested max_accel <= 2800 mm/sec^2
Fitted shaper 'ei' frequency = 41.6 Hz (vibrations = 1.3%, smoothing ~= 0.186)
To avoid too much smoothing with 'ei', suggested max_accel <= 3200 mm/sec^2
Fitted shaper '2hump_ei' frequency = 40.2 Hz (vibrations = 0.0%, smoothing ~= 0.334)
To avoid too much smoothing with '2hump_ei', suggested max_accel <= 1700 mm/sec^2
Fitted shaper '3hump_ei' frequency = 48.0 Hz (vibrations = 0.5%, smoothing ~= 0.356)
To avoid too much smoothing with '3hump_ei', suggested max_accel <= 1500 mm/sec^2
Recommended shaper is ei @ 41.6 Hz
```

Configure input_shaper section in printer.cfg

```yml
[input_shaper]
shaper_freq_x: 55.0
shaper_type_x: mzv
shaper_freq_y: 41.6
shaper_type_y: ei
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
