---
layout: post
title: Dronecan Sensor Node - BMP280
repo: https://github.com/alexandrephl/PX4Avoidance-SafetyLayer
description:  >-
  A DroneCAN (UAVCAN v0) sensor node running on STM32F446.  It publishes a clean air-data subset on the CAN bus (heartbeat + pressure + temperature) using a BSP layer, a dedicated sensor driver, and libcanard for transport.

skills:
  - C (bare-metal + RTOS)  
  - FreeRTOS / CMSIS-RTOS  
  - STM32 HAL (isolated in BSP)   
  - DroneCAN / libcanard
  - STM32CubeIDE
  - Breadboarding for circuit testing
  - Low-level drivers (CAN, UART, I2C...)

main-image: /safety-layer.jpg
---
---

## Why It Matters
DroneCAN is widely used for **robust sensor/actuator networks** in UAV systems because it provides a structured message model on top of CAN.  
This project demonstrates an end-to-end embedded implementation of a **real sensor node**: hardware acquisition → engineering units → deterministic periodic publishing → external validation on a real CAN bus.

---

## System Architecture  

### Components & Responsibilities  

- **STM32F446 (Node MCU)**:
  - Runs the real-time scheduling (FreeRTOS/CMSIS-RTOS).
  - Owns CAN/I2C/UART peripheral access through a BSP abstraction layer.
  - Acquires raw sensor registers over I2C and computes engineering units using Bosch compensation formulas.
  - Publishes DroneCAN (UAVCAN v0) messages on Classic CAN (2.0):
    - `uavcan.node.Heartbeat`
    - `uavcan.equipment.air_data.StaticPressure` (Pa)
    - `uavcan.equipment.air_data.StaticTemperature` (K)

- **BMP280 (Pressure/Temperature Sensor)**:
  - Provides raw temperature + pressure registers over I2C.
 
- **PX4 (Autopilot)**:
  - Subscribes to the DroneCAN sensor messages on the CAN bus.
  - Uses the published measurements for integration/validation.

## What I Implemented
- CAN TX/RX integration with libcanard (UAVCAN v0 transfers)
- Periodic **heartbeat** publishing (RTOS-driven)
- Periodic **pressure** and **temperature** publication
- Basic node configuration (node ID, node name, bit-rate)
- External validation scripts using **python-can** + **gs_usb** (USB–CAN)

## Validation & Results  

The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://www.youtube.com/watch?v={**MhVw-MHGv4s**}&ab_channel=engineerguy
{% include youtube-video.html id="MhVw-MHGv4s" autoplay= "false"%}
{% include youtube-video.html id="XGC31lmdS6s" autoplay = "true" %}


<div style="float:right; margin: 0 0 1rem 1rem; font-size: 28px;">
  <a href=repo target="_blank" rel="noopener">
    <i class="fa-brands fa-github"></i>
  </a>
</div>
