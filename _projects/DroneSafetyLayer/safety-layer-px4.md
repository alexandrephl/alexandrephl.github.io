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
This project tackles that integration problem by implementing a mixed-criticality architecture: a Raspberry Pi proposes avoidance maneuvers, an STM32 safety supervisor validates them deterministically (freshness, bounds, TTL/confidence, state constraints), and PX4 executes only safe commands—otherwise it falls back to a predictable failsafe path.
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
  
### Embeed images
{% include image-gallery.html images="/safety-layer.jpg" height="400" %} 


## Embedding youtube video
The second video has the autoplay on. copy and paste the 11-digit id found in the url link. <br>
*Example* : https://www.youtube.com/watch?v={**MhVw-MHGv4s**}&ab_channel=engineerguy
{% include youtube-video.html id="MhVw-MHGv4s" autoplay= "false"%}
{% include youtube-video.html id="XGC31lmdS6s" autoplay = "true" %}

you can also set up custom size by specifying the width (the aspect ratio has been set to 16/9). The default size is 560 pixels x 315 pixels.  

The width of the video below. Regardless of initial width, all the videos is responsive and will fit within the smaller screen.
{% include youtube-video.html id="tGCdLEQzde0" autoplay = "false" width= "900px" %}  

<br>

## Adding a hozontal line
---

## Starting a new line
leave two spaces "  " at the end or enter <br>

## Adding bold text
this is how you input **bold text**

## Adding italic text
Italicized text is the *cat's meow*.

## Adding ordered list
1. First item
2. Second item
3. Third item
4. Fourth item

## Adding unordered list
- First item
- Second item
- Third item
- Fourth item

## Adding code block
```ruby
def hello_world
  puts "Hello, World!"
end
```

```python
def start()
  print("time to start!")
```

```javascript
let x = 1;
if (x === 1) {
  let x = 2;
  console.log(x);
}
console.log(x);

```

## Adding external links
[Wikipedia](https://en.wikipedia.org)


## Adding block quote
> A blockquote would look great if you need to highlight something


## Adding table 

| Header 1 | Header 2 |
|----------|----------|
| Row 1, Col 1 | Row 1, Col 2 |
| Row 2, Col 1 | Row 2, Col 2 |

make sure to leave aline betwen the table and the header


