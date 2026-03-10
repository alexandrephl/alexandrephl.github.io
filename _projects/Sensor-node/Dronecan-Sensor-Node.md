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
