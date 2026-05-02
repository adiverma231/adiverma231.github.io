---
title: Agrovolo Drone
date: 2025-12-15 00:00:00 +0800
tags: [C, Computer Vision, VTOL, Microcontroller, KiCAD] 
pin: true
image:
  path: /assets/images/agrovolo_frame.png
  alt: Crop Drone
  class: shadow
---

As part of UBC Rapid, I designed a Filament Recycler that controls the following peripherals:
- ILI9431 SPI Display, which is compatible with the LVGL graphics library
- Buttons and Rotary Encoder
- Two TMC2209 Stepper Drivers
- Four MOSFET Based PWM 5V Fan Controllers
- EEPROM for extra memory to accommodate the graphics library
- One I2C sensor I/O, for hall effect filament diameter measuring sensor
- One SPI sensor I/O, because there were extra pins
- Logic level converter for controlling an external motor driver, for the large shredder motor

In order to power all of circuitry on this PCB, I implemented a 24V to 5V Buck Converter and a 5V to 3.3V LDO, which can provide power at 24V, 5V, and 3.3V. This PCB can also be programmed and powered
through a USB-C port or serial wire debug pins.
  


### Microcontroller and Boot Switch Schematic
![STM32](/assets/images/prev_board.png){: .w-75 .mx-auto .d-block }

- I used an STM32F4 as the microcontroller for this project

### Stepper Driver
![Stepper Schematic](/assets/images/prev_board.png){: .w-75 .shadow .rounded-10 }

- Two TMC2209 steppermotor drivers powered by 24V input and communicates via UART to the STM32




