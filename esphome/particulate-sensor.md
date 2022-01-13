# ESPHome + IKEA VINDRIKTNING Particulate sensor
This guide explains how to modify a low cost IKEA VINDRIKTNING particulate (air quality) sensor into a Home Assistant smart sensor and Bluetooth Low Energy (BLE) device scanner by adding a low cost ($5 USD) [ESP32](https://en.wikipedia.org/wiki/ESP32) single board computer (SBC) and running [ESPHome](https://esphome.io) software.

## Overview
The ESPHome + IKEA VINDRIKTNING device can perform 2 functions in a smarthome:
1. View particulate air quality measurements in Home Assistant
2. Scan for events from Bluetooth Low Energy (BLE) motion sensor, thermometer, and other sensors and use them in Home Assistant automations.

### View particulate air quality measurements

### ESP32 SoC microcontroller
ESP32 is a microcontroller that uses a System on Chip (SoC) processor that provides a lot of capability at a low price and small size. For $5 USD, the SoC has integrated 802.11 b/g/n Wi-Fi and Bluetooth v4.2 connectivity. It's size is slightly larger than a SD card. Programs can be transferred to the onboard flash memory through micro USB cable connected to your PC or wirelessly through Wi-Fi. Device can be powered by USB cable or directly by a 5V DC power source.

#### Comparison to Arduino
Both ESP32 and Arduinos run a single program rather than an operating system. ESP32 single board computer (SBC) is much faster and has more connectivity than the similarly sized Arduino Nano microcontroller. ESP32 has 100x RAM and 10x flash memory of the Arduino Nano. Arduino Nano does not have Wi-Fi or Bluetooth.  Both devices can be powered by USB cable or directly by a 5V DC power source.

#### Comparison to Raspberry Pi
Raspberry Pi uses faster  ARM SoC processor. Even the slower Raspberry Pi Zero is much more powerful (has 1000x more RAM than ESP32) and runs a full Linux operating system at a higher cost, and uses micro SD card for storage. Both Raspberry Pi Zero W and ESP32 support 802.11 b/g/n Wi-Fi and Bluetooth. Both devices can be powered by USB cable or directly by a 5V DC power source.
