---
layout: post
title: Dronecan Sensor Node - BMP280
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

