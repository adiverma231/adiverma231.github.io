---
title: Agrovolo Drone
date: 2025-12-15 00:00:00 +0800
tags: [Embedded, Firmware, PCB, Autonomy, Python, C] 
pin: true
image:
  path: /assets/images/agrovolo/agrovolo_frame.png
  alt: VTOL
  class: shadow
---

Agrovolo is an autonomous imaging UAV built for agricultural applications, developed as part of Embedded Systems @ Purdue and an ECE senior design team. The drone flies over crop fields, captures overhead imagery, and runs image processing to generate maps that give farmers actionable data on crop health, plant density, and field stress for a low-cost.

![3D Render — Isometric View](/assets/images/agrovolo/render_iso.jpg)
![3D Render — Top View](/assets/images/agrovolo/render_top.jpg)
![3D Render — Side View](/assets/images/agrovolo/render_side.jpg)


## System Overview

The drone is a VTOL (vertical takeoff and landing) quadrotor with a 3D-printed fuselage. Onboard compute is handled by a Raspberry Pi 4, which manages image capture, storage, and a real-time green-plant preview during flight. Two Pi Camera Module v3 cameras connect to the Pi via CSI ribbon cables. A Teensy 4.0 serves as the flight controller, interfacing with the IMU and GPS and handling motor control over the 2.4GHz radio link from the handheld ground station.

![Onboard Raspberry Pi Schematic](/assets/images/agrovolo/onboard_pi_schematic.png)


Ground controller being used to spin up props and control motor speed: [video](https://github.com/adiverma231/adiverma231.github.io/blob/main/assets/literature/IMG_3321.MOV)

## Imaging Pipeline

1. Both cameras capture raw images over the field during flight and write them to the Pi's microSD card
2. A quick green-plant preview is generated in-flight to confirm capture — heavy processing is deferred to save power
3. After landing, images are transferred to a laptop and stitched into a georeferenced map (OpenDroneMap)
4. Python analysis scripts process the stitched map to extract field health data

What we can do with Infrared alone:
- Canopy cover percentage (green vs bare soil)
- Stand counts and plant density heatmaps
- Weed hotspot detection by color and texture
- Growth monitoring over time via repeated flights
- Visible stress detection (yellowing, gaps, irregular patches)

Future upgrade path includes NoIR or proper multispectral cameras to enable NDVI and other vegetation indices for earlier, more accurate stress detection.

## Ground Controller

![Ground Controller Schematic](/assets/images/agrovolo/ground_controller_schematic.png)

The handheld ground station communicates with the drone over 2.4GHz radio. It handles flight commands and triggers the imaging system. The controller firmware is written in C.

## Flight Controller

![Flight Controller Board Render](/assets/images/agrovolo/MCU_board.png)

The Teensy 4.0-based flight controller handles real-time attitude estimation using the onboard IMU, processes GPS data for position hold, and drives the four BLDC motor controllers. Firmware is written in C and runs on a bare-metal RTOS.

## Some Design Considerations
- 3D-printed fuselage designed to be aerodynamics-friendly and lightweight — full print stats available below
- Raspberry Pi chosen for its native CSI camera support and Python ecosystem, keeping the imaging stack simple and well-documented
- Heavy image processing deliberately offloaded to post-flight to reduce onboard power draw during flight
- IR-only camera chosen for v1 to nail the mapping pipeline before adding complexity
- 2.4GHz radio link selected for reliable range and low latency for motor control commands
- Modular camera mount designed to allow future swap to NoIR or multispectral sensors
- Ground station schematic kept simple to make the RF link easy to debug and iterate on

## BOM

Full bill of materials available [here](https://github.com/adiverma231/adiverma231.github.io/blob/main/assets/literature/agrovolo_BOM.pdf)

3D Print Statistics [here](https://github.com/adiverma231/adiverma231.github.io/blob/main/assets/literature/agrovolo2_print_details.pdf)
