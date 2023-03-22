---
title: Perfect input shaping - Install the ADXL345 accelerometer on Artillery Sidewinder/Genius Pro
date: 2023-03-22T00:00:00+01:00
description: About installing the ADXL345 accelerometer to calibrate easily input shaping for an Artillery Sidewinder with Klipper firmware.
draft: false
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

## Intro

Simply said: _Input Shaping_ or _Resonance Compensation_ is a technic to reduce vibrations and resonances by proactive adjusting the printer movements. It increases the max possible printer speed without reducing the print quality. More details can be found in Klipper [Resonance Compensation](https://www.klipper3d.org/Resonance_Compensation.html#resonance-compensation) documentation.

This article focuses on Klipper firmware, but also Marlin added Input Shaping as experimental feature in its latest Version. [see M593](https://marlinfw.org/docs/gcode/M593.html).

There is also a manual way to determine the resonance frequencies but this post shows you the steps to measure the frequencies by an ADXL345 accelerometer chip. You can get it for some bucks.

Ready? Let's start!

## Requirements

What do you need?

- Your printer (tested with Artillery Sidewinder X2)
- A working Klipper installation (tested with [MainsailOS](https://docs.mainsail.xyz/))
- Jumper cables or similar
- ADXL345 accelerometer module
- Adapter to mount it on your extruder [see my Design](https://www.thingiverse.com/thing:5511453)
- Raspberry Pi (or any other connected MCU with SPI interface)

## Install and wire the accelerometer

As always, Klipper has a very clean and useful documentation. It describes well how to [wire the accelerometer](https://www.klipper3d.org/Measuring_Resonances.html#wiring).

A example how to connect your ADXL345 to the SPI interface of the RaspberryPI. Here is a copy of the relevant wiring diagram:

![Wire ADXL345](images/adxl345-wire.png "Wire ADXL345")

I used standard jumper cables for that task, but in case you want to do some crafting, go on.

If you have the Sidewinder X1/X2 or Genius Pro you can use [my mount](https://www.thingiverse.com/thing:5511453) from Thingiverse to install the accelerometer on you extruder. I suggest V2, the sensor is in parallel to the xy-plane and mounted with screws.
If you own another printer, you have to look for different mounting solution.

![ADXL345 Mount](images/adxl345-photo.png "ADXL345 Mount")

## Build a Klipper MCU for PI

To make use of the SPI interface, you need to build and setup a additional Klipper firmware for your target. So you can configure it as an MCU in your config files.

### Software installation

[Detailed Klipper documentation](https://www.klipper3d.org/Measuring_Resonances.html#software-installation)

- Login to your MainsailOS PI
- Make sure, needed packages are installed. (In newer MainsailOS everything is already installed)

  ```sh
  sudo apt update
  sudo apt install python3-numpy python3-matplotlib libatlas-base-dev
  ```

  ```sh
  ~/klippy-env/bin/pip install -v numpy
  ```

### Build secondary MCU for PI

[RPi Micro-Controller documentation](https://www.klipper3d.org/RPi_microcontroller.html)

- First, make sure the Linux SPI driver is enabled by running `sudo raspi-config` and enabling SPI under the "Interfacing options" menu.

- Next, compile the Klipper micro-controller code, start by configuring it for the "Linux process":

  ```sh
  cd ~/klipper/
  make menuconfig
  ```

In the menu, set "Microcontroller Architecture" to "Linux process," then save and exit.

- Build and install the new micro-controller code, run:

  ```sh
  sudo service klipper stop
  make flash
  sudo service klipper start
  ```

- Copy a the matching klipper-mcu service, enable and start it.

  ```sh
  cd ~/klipper/
  sudo cp ./scripts/klipper-mcu.service /etc/systemd/system/
  sudo systemctl enable --now klipper-mcu.service
  ```

- Configure the new compiled RPi MCU in your `printer.cfg`

  ```yml
  [mcu rpi]
  serial: /tmp/klipper_host_mcu

  [adxl345]
  cs_pin: rpi:None
  axes_map: -y, x, z # Depends on mounting direction. Works for my V2 Mount

  [resonance_tester]
  accel_chip: adxl345
  probe_points:
      150, 150, 20 # mid of your bed (Sidewinder size: 300x300mm)
  ```

### Test and Calibration steps

- Open your G-Code console (for example in Mainsail)
- Check if the accelerometer respond. If not, double check the steps above and Klipper documentation.

  ```text
  ACCELEROMETER_QUERY
  ```

- A output should look similar to this

  ```text
  accelerometer values (x, y, z): 536.266849, -888.247130, 9918.759623
  ```

- Check if sensor and wires don't have to much noise

  ```text
  MEASURE_AXES_NOISE
  ```

- All values should be below 100

  ```text
  Axes noise for xy-axis accelerometer: 11.789601 (x), 16.017559 (y), 29.190110 (z)
  ```

If you have problems, consult [Checking the setup](https://www.klipper3d.org/Measuring_Resonances.html#checking-the-setup)

### Manual Test X- and Y-Axes

- Test x-axis resonances on G-Code console (for example in Mainsail)

  ```sh
  TEST_RESONANCES AXIS=X
  ```

- Login to your Raspberry Pi terminal (with ssh)
- Run the `calibrate_shaper.py` with the generated csv, also generate a beautiful diagram over the measured and calculated frequencies.

  Example:

  ```sh
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

- Test y-axis resonances on G-Code console (for example in Mainsail)

  ```sh
  TEST_RESONANCES AXIS=Y
  ```

- Login to your Raspberry Pi terminal (with ssh)
- Run the `calibrate_shaper.py` with the generated csv, also generate a beautiful diagram over the measured and calculated frequencies.

  Example:

  ```sh
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

- Configure _input_shaper_ section in `printer.cfg`. You can take the following snipped and replace your calculated and chosen frequencies and algorithm.

  Example:

  ```yml
  [input_shaper]
  shaper_freq_x: 55.0
  shaper_type_x: mzv
  shaper_freq_y: 41.6
  shaper_type_y: ei
  ```

### Alternative auto calibrate

If you are not interested in any details, you can use the automatic `SHAPER_CALIBRATE` script. See [Input Shaper auto-calibration](https://www.klipper3d.org/Measuring_Resonances.html#input-shaper-auto-calibration) documentation.

- In short:

  ```sh
  SHAPER_CALIBRATE
  ```

  or

  ```sh
  SHAPER_CALIBRATE AXIS=X or SHAPER_CALIBRATE AXIS=Y
  ```

- Don't forget to save your config

  ```sh
  SAVE_CONFIG
  ```

## Conclusion

Hope this was straight forward and you could successful measure your resonances. Now you should be able to print faster with your Artillery Sidewinder X2. Try to increase your acceleration and speed.

For further steps and more details it is always a very good idea to walk through the very good official documentations

- [Klipper Documentation](https://www.klipper3d.org/Overview.html)
- [Mainsail Documentation](https://docs.mainsail.xyz/)

Please leave comments, if something is wrong or missing. Or if you have questions.

Enjoy!

If you like, buy me a coffee

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/F2F7GC8PC)

freakyDude
