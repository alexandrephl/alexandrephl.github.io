---
layout: post
title: PX4 Obstacle Avoidance Safety Layer
repo: "https://github.com/alexandrephl/PX4Avoidance-SafetyLayer"
description:  >-
  Drone obstacle avoidance without trusting vision blindly: a deterministic safety supervision between computer-vision planner and PX4 autopilot.
  
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
  - Accepts **Offboard velocity setpoints** over MAVLink.
  - Publishes **heartbeat** and selected state feedback (e.g., velocity) over MAVLink.
- **Raspberry Pi (Autonomy):**
  - Runs perception algorithm to detect obstacles (distance/relative motion).
  - Runs planning to compute the next safe maneuver / trajectory.
  - Produces velocity setpoints *(vx, vy, vz, yaw_rate)* plus metadata *(timestamp, TTL, confidence)*.
  - Sends planner commands to the STM32 over CAN.
- **STM32F446 (Safety Gatekeeper):**
  - Receives planner commands via DroneCAN.
  - Receives PX4 status via MAVLink (heartbeat + selected state feedback).
  - Validates every planner command under deterministic constraints:
    - freshness watchdog
    - numerical validity + bounds
    - TTL/confidence policy
    - state compatibility (armed/offboard)
    - system/hardware constraints (project-defined)
  - Forwards **only safe commands** to PX4; otherwise enforces a deterministic failsafe behavior.

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

<img src="/_projects/DroneSafetyLayer/SafetyLayerDiagram.png" alt="Safety gate state machine" style="width:100%; max-width:900px; display:block; margin:0 auto; filter:none; mix-blend-mode:normal; opacity:1;">

---

## Validation & Results


The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://www.youtube.com/watch?v={**MhVw-MHGv4s**}&ab_channel=engineerguy
{% include youtube-video.html id="MhVw-MHGv4s" autoplay= "false"%}
{% include youtube-video.html id="XGC31lmdS6s" autoplay = "true" %}






