# ESPHome + IKEA VINDRIKTNING Particulate sensor
This guide explains how to modify a low cost IKEA VINDRIKTNING particulate (air quality) sensor into a Home Assistant smart sensor and Bluetooth Low Energy (BLE) sensor scanner by adding a low cost ($5 USD) [ESP32](https://en.wikipedia.org/wiki/ESP32) microcontroller and running [ESPHome](https://esphome.io) software.

![ESPHome + IKEA VINDRIKTNING air quality sensor ](esphome/images/img1.jpg)

## Overview
The ESPHome + IKEA VINDRIKTNING device can perform 2 functions in a smarthome:
1. View particulate air quality measurements in Home Assistant
2. Scan for events from Bluetooth Low Energy (BLE) motion sensor, thermometer, and other sensors. Events from sensors can then be used in Home Assistant automations.

[ESPHome](https://esphome.io) software enables connecting sensors to Home Assistant using microcontrollers like ESP8622 and ESP32.

## Particulate Matter air quality measurements
Particulate Matter (PM) are microscopic particles that are in the air and is a harmful form of air pollution that can cause health problems like heart attacks and respiratory disease. The level of airborne particulate matter can change based on weather (stagnant air) or wildfires, so measuring your home air quality can provide insight on when one needs to be cautious outdoors.

## Home Assistant automations with Bluetooth LE sensors
With automations, Home Assistant can automatically respond and trigger actions based on sensor events. For example, turn on lights automatically at sunset. With motion sensors, automations can trigger turning on lights automatically when there's motion and turn lights off after no motino is detected for a set period of time.

## IKEA VINDRIKTNING air quality monitor
![IKEA VINDRIKTNING air quality sensor](esphome/images/img4.jpg)

IKEA VINDRIKTNING device measures air quality with its built-in PM1006 particulate matter sensor that measures fine particulate matter (PM2.5). It's designed as a small, unassuming white box that blends into the background in a home and is powered by a USB-C cable and 5V 2A wall adapter. It contains no built-in connectivity or smarthome functionality.

The sensor uses an optical sensing method to measure particle mass concentration (μg/m³).

LED color | PM2.5 concentration | Air quality
----------|---------------------|------------
Green     | 0 to 35 μg/m³       | Good
Amber     | 36 to 85 μg/m³      | OK
Red       | Above 86 μg/m³      | Not good

While the sensor has an accuracy of ±20μg/m³ or ±20% of reading, the IKEA VINDRIKTNING is only able to display air quality as 1 of 3 colors in stock form out of the box. It has no memory or history function. Modifying it by adding a ESP32 microcontroller and connecting it to Home Assistant will unlock the full functionality of the particulate sensor.

## ESP32 SoC microcontroller
ESP32 is a microcontroller that uses a System on Chip (SoC) processor that provides a lot of capability at a low price and small size. For $5 USD, the SoC has integrated 802.11 b/g/n Wi-Fi and Bluetooth v4.2 connectivity. It's size is slightly larger than a SD card. Programs can be transferred to the onboard flash memory through micro USB cable connected to your PC or wirelessly through Wi-Fi. Device can be powered by USB cable or directly by a 5V DC power source.

### Comparison to Arduino
Both ESP32 and Arduinos run a single program rather than an operating system. ESP32 single board computer (SBC) is much faster and has more connectivity than the similarly sized Arduino Nano microcontroller. ESP32 has 100x RAM and 10x flash memory of the Arduino Nano. Arduino Nano does not have Wi-Fi or Bluetooth.  Both devices can be powered by USB cable or directly by a 5V DC power source.

### Comparison to Raspberry Pi
Raspberry Pi uses faster  ARM SoC processor. Even the slower Raspberry Pi Zero is much more powerful (has 1000x more RAM than ESP32) and runs a full Linux operating system at a higher cost, and uses micro SD card for storage. Both Raspberry Pi Zero W and ESP32 support 802.11 b/g/n Wi-Fi and Bluetooth. Both devices can be powered by USB cable or directly by a 5V DC power source.
