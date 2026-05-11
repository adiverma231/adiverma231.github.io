---
title: Purdue Solar Racing - Steering Wheel
date: 2026-03-17 00:00:00 +0800
tags: [Embedded, Firmware, C, CAN] 
pin: true
image:
  path: /assets/images/artemis/SteerWheel_pcb_layout.png
  alt: Steering wheel
  class: shadow
---

The steering wheel is the driver's only interface to Artemis ('25-26 solar car iteration). I designed the PCB and wrote the firmware for the board, which handles all driver inputs and communicates state to the rest of the car over CAN.

![PCB Render](/assets/images/steering-wheel/pcb_render.jpg)

## Hardware

The board is built around an RP2350 microcontroller. Driver inputs are handled through a button matrix — multiplexing multiple buttons across a reduced GPIO footprint rather than dedicating a pin per switch. This keeps the design compact and leaves room for future input expansion without a PCB revision.

![Schematic](/assets/images/artemis/SteerWheel_schematic.png)

Since the RP2350 has no native CAN peripheral, I used an external CAN-FD controller (TCAN4550-Q1) connected over SPI to interface with the vehicle bus. The board is powered off the car's 12V auxiliary rail, regulated down to 3.3V for the MCU and logic.

![PCB Layout](/assets/images/artemis/SteerWheel_pcb_layout.png)

## Firmware

Firmware is written in C using Arduino IDE and HAL. The main loop scans the button matrix, debounces inputs, and packages the driver state into CAN frames using the team's shared [`can-lib`](https://github.com/Purdue-Solar/pico_canlib); a C++ abstraction layer that wraps the FDCAN peripheral across the team's board ecosystem.

Button states are packed into a message and transmitted on the CAN bus at a fixed interval. The VCU and other nodes consume these messages to act on driver commands — things like mode changes, hazard inputs, and regen adjustments.

## Some Design Considerations

- Capabilities:
  - Cruise control (enable, up, down)
  - Lights (left/right turn, brights, hazards)
  - Regen braking (toggle)
  - PTT Radio
  - Horn
- Button matrix to minimize GPIO usage and simplify future input additions
- Shared `can-lib` submodule used for consistency across all Artemis boards
- Debounce handled in firmware rather than hardware to reduce component count and allow tuning without a board respin
- 3.3V regulated supply derived onboard rather than relying on the harness to supply clean logic voltage
- Connector placement kept to board edges for clean harness routing inside the chassis

## Repository

Firmware source: [github.com/Purdue-Solar/lux-steering-wheel](https://github.com/Purdue-Solar/artemis26-steering-wheel)