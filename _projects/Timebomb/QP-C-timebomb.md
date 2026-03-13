---
layout: post
title: TM4C123 TimeBomb
repo: "https://github.com/alexandrephl/tm4c-QP-C-timebomb"

description: >-
  A reactive embedded demo built on the EK-TM4C123GXL LaunchPad using the QP/C framework.  The project demonstrates a clean Active Object architecture, a hierarchical state machine, time events, and live runtime tracing via QSPY over UART0.

skills:
  - C (bare-metal + RTOS)
  - TM4C
  - BSP conception
  - QP/C
  - Active Object architecture
  - Hierarchical state machines
  - Event-driven systems
  - QSPY tracing
  - Code Composer Studio

main-image: 
---

---

## Why It Matters

This project addresses a fundamental embedded software challenge: how to design systems that respond cleanly to asynchronous events without falling into polling loops, scattered flags, or hard-to-maintain control logic. Here, the behavior is organized around a hierarchical state machine, event-driven dispatching, time-based transitions, and live runtime tracing. These concepts are important because they scale directly to real embedded products, where user interfaces, controllers, fault handling, and supervisory logic all benefit from deterministic and well-structured event-driven architectures.

---

## System Architecture

### Components & Responsibilities
- **TimeBomb Active Object**
  - Encapsulates the application behavior as a reactive object.
  - Processes asynchronous events through the QP/C event framework.
  - Implements the full behavioral logic as a hierarchical state machine.
- **Board / BSP Layer**
  - Handles hardware-specific interactions:
    - buttons (SW1 / SW2)
    - LEDs
    - UART tracing interface
  - Isolates low-level board access from the application state machine.
- **QP/C Framework**
  - Provides the event-driven execution model.
  - Manages dispatching, state transitions, and time events.
  - Supports the Active Object pattern used by the demo.
- **QSPY / QS Tracing**
  - Captures runtime events and state transitions over UART0.
  - Provides visibility into the system behavior while it executes.

---

## Functional Behavior

### Main Concept
The demo models a countdown device controlled by two buttons:

- **SW2** toggles the system between **disarmed** and **armed**
- **SW1** starts or restarts the countdown when the system is armed
- If the countdown reaches zero, the system enters the **boom** state
- In the **boom** state, all LEDs are turned on
- The system can then be reset manually or by restarting execution

### State Machine

<div class="project-diagram-center">
  <div class="project-diagram-wrap">
    <img class="project-diagram" src="/_projects/Timebomb/Timebomb_diagram.png" alt="Safety gate state machine">
  </div>
</div>

---

## Validation & Observability
A key strength of this project is that the behavior is not treated as a black box.

Using **QSPY** over UART0 at **115200 baud**, the system can be observed live:
- button events
- state transitions
- time-event activity
- LED-related behavior

This makes it possible to validate that the software reacts exactly as intended, which is demonstrated in the following video:
