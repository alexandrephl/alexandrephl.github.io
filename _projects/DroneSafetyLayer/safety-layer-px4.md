---
layout: post
title: PX4 Obstacle Avoidance Safety Layer
description:  >-
  Drone obstacle avoidance without trusting vision blindly: a deterministic safety supervision between computer-vision planner and PX4 autopilot
  
skills:
  - C (bare-metal + RTOS)  
  - FreeRTOS / CMSIS-RTOS  
  - STM32 HAL (isolated in BSP)  
  - MAVLink (UART DMA RX)  
  - DroneCAN / libcanard  
main-image: /safety-layer.jpg
---

---

## Why it matters 
Modern small UAVs increasingly rely on computer vision and autonomy, but those AI workloads are not deterministic (variable runtime, OS/GPU scheduling, resource contention). Meanwhile, flight control requires hard real-time deadlines.
This project tackles that integration problem by implementing a mixed-criticality architecture: a Raspberry Pi proposes avoidance maneuvers, an STM32 safety supervisor validates them deterministically (freshness, bounds, TTL/confidence, state constraints), and PX4 executes only safe commands, otherwise it falls back to a predictable failsafe path.
It demonstrates a practical way to enable advanced perception without compromising control-loop safety, including latency/jitter measurement on the communication chain.  

---

## System Architecture  

### Components & Responsibilities  
- **PX4 (Autopilot):**
  - Maintains stabilization and core flight control loops.
  - Accepts **Offboard velocity setpoints** over MAVLink and track setpoints.
  - Sends Heartbits & flying state data (vel...) over mavlink.
- **Raspberry Pi (Autonomy):**
  - Runs perception algorithm and identify obstacles with distances/speed.
  - Runs flight planning algorithm: Establish next target point, using best avoidance tragectory if needed.
  - Produces velocity setpoints *(vx, vy, vz, yaw_rate)* plus metadata *(timestamp, TTL, confidence)*, and sends them over CANBus.
- **STM32F446 (Safety Gatekeeper):**
  - Reads planning commands over CAN.
  - Reads UAV Heartbits & flying state data (vel...) over mavlink.
  - Validates every planning command under deterministic constraints: (freshness, bounds, TTL/confidence, state constraints, hardware constraints).
  - Emits safe commands only, otherwise forces a predictable failsafe behavior.

### Interfaces  
- **Pi ↔ STM32:** DroneCAN (UAVCAN v0) via libcanard (Classic CAN 2.0)
- **STM32 ↔ PX4:** MAVLink over UART (DMA RX for robust reception)

  ---

## Safety Gate Design  

### Command Validity Rules  

A planner command is considered valid only if:
- **Freshness:** received within a strict timeout (watchdog)
- **Bounds:** velocity and yaw-rate stay within configured limits
- **Planner metadata:** TTL/confidence meets minimum requirements (project-defined)
- **State compatibility:** commands are accepted only when the system is armed & in offboard state

### State Machine  

#present state machine diagram or code.  

---

## Validation & Results


The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://www.youtube.com/watch?v={**MhVw-MHGv4s**}&ab_channel=engineerguy
{% include youtube-video.html id="MhVw-MHGv4s" autoplay= "false"%}
{% include youtube-video.html id="XGC31lmdS6s" autoplay = "true" %}






