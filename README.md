# NINA-W102 firmware fork for ESP32-CAM

This is the NINA-W102 fork for ESP-CAM. The ESP32-CAM board will be used as co-processor for Adafruit feather compatible MCUs (like Adafruit AirLift). Currently only supports WIFI, camera work has not yet started.

This firmware uses [Espressif's IDF](https://github.com/espressif/esp-idf)

## Wiring Diagram

```
                          +-- > 5
                          |
                +---------|-----+                         +-------------------+
                |    ESP_ |     |                    ~RST | o                 |
                |    RST--+     |            +------ 3V3  | o     Feather     |
             5V | o           o | 3V3 -------+       AREF | o     Wing        |
            GND | o    ___    o | io16-->10(RDY)  +- GND  | o                 |
  MISO <-- io12 | o   / _ \   o | io0             |  A0   | o               o | VBAT
  MOSI <-- io13 | o  ( (_) )  o | GND ------------+  A1   | o               o | EN
 9(CS) <-- io15 | o   \___/   o | VCC                A2   | o               o | VUSB
   SCK <-- io14 | o    |||    o | U0RX               A3   | o               o | 13
            io2 | o    |||    o | U0TX               A4   | o               o | 12
            io4 | o    |||    o | GND                A5   | o               o | 11
                |   [=======]   |           io14 <-- SCK  | o               o | 10(RDY) --> io16
                |               |           io13 <-- MOSI | o               o | 9(CS) --> io15
                |   ESP32-CAM   |           io12 <-- MISO | o               o | 6
                +---------------+                    RX   | o               o | 5 --> ESP_RST
                                                     TX   | o               o | SCL
                                                     FREE | o               o | SDA
                                                          +-------------------+
```
Please note: A wire needs to be soldered to the ESP32-CAM's reset button (on the backside, the one close to the yellow capacitor) which can be connected to pin 5 on the feather board. 

## Contributing to nina-fw

Please be aware that by contributing to this project
you are agreeing to the [Code of Conduct](https://github.com/adafruit/nina-fw/blob/master/code-of-conduct.md).
Contributors who follow the [Code of Conduct](https://github.com/adafruit/nina-fw/blob/master/code-of-conduct.md)
are welcome to submit pull requests and they will be promptly
reviewed by project admins. Please join the [Discord](https://adafru.it/discord) too.

The NINA firmware version needs to be updated in four places in this repo:
1. CommandHandler.cpp
1. combine.py
1. Makefile
1. CHANGELOG

## Building

The firmware shipped in Adafruit's products is compiled following these
instructions. These may differ from the instructions included in the
original Arduino firmware repository.

1. [Download the ESP32 toolchain](https://docs.espressif.com/projects/esp-idf/en/v3.3.1/get-started/index.html#setup-toolchain)
1. Extract it and add it to your `PATH`: `export PATH=$PATH:<path/to/toolchain>/bin`
1. Clone **v3.3.1** of the IDF: `git clone --branch v3.3.1 --recursive https://github.com/espressif/esp-idf.git`
1. Set the `IDF_PATH` environment variable: `export IDF_PATH=<path/to/idf>`
1. Run `make firmware` to build the firmware (in the directory of this read me)
1. You should have a file named `NINA_W102-x.x.x-cam.bin` in the top directory
1. Use appropriate tools (esptool.py, appropriate pass-through firmware etc)
   to load this binary file onto your board. Example: esptool.py --port /dev/ttyS6 --before no_reset --baud 115200 write_flash 0 NINA_W102-1.7.0-cam.bin
    a. If you do not know how to do this, [we have an excellent guide on the Adafruit Learning System for upgrading your ESP32's firmware](https://learn.adafruit.com/upgrading-esp32-firmware)


## License

Copyright (c) 2018 Arduino SA. All rights reserved.

This library is free software; you can redistribute it and/or
modify it under the terms of the GNU Lesser General Public
License as published by the Free Software Foundation; either
version 2.1 of the License, or (at your option) any later version.

This library is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public
License along with this library; if not, write to the Free Software
Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA
