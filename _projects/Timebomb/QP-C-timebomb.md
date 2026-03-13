---
layout: post
title: TM4C123 TimeBomb
repo: "https://github.com/alexandrephl/tm4c-QP-C-timebomb"

description: >-
  Event-driven embedded demo on the EK-TM4C123GXL board using QP/C. The project showcases a hierarchical state machine,
  time events, button-driven transitions, and live runtime tracing with QSPY over UART.

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

main-image: /timebomb_header.png
---

## Project Overview
A **reactive embedded demo** built on the **EK-TM4C123GXL LaunchPad** using the **QP/C framework** from Quantum Leaps.  
The project demonstrates a clean **Active Object architecture**, a **hierarchical state machine**, **time events**, and **live runtime tracing** via **QSPY over UART0**.

## Why It Matters
This project focuses on a core embedded software challenge: building systems that react cleanly to asynchronous events without relying on polling loops or tangled control logic.

Instead of implementing behavior with ad hoc flags and `while` loops, the application is structured around:
- a **hierarchical state machine**
- **event-driven dispatching**
- **time-based transitions**
- **runtime observability** through tracing

This is valuable because the same design principles scale to real embedded products: user interfaces, controllers, fault handling, and real-time supervisory logic all benefit from deterministic event-driven architectures.

---

## System Architecture

### Components & Responsibilities

**TimeBomb Active Object**
- Encapsulates the application behavior as a reactive object.
- Processes asynchronous events through the QP/C event framework.
- Implements the full behavioral logic as a hierarchical state machine.

**Board / BSP Layer**
- Handles hardware-specific interactions:
  - buttons (SW1 / SW2)
  - LEDs
  - UART tracing interface
- Isolates low-level board access from the application state machine.

**QP/C Framework**
- Provides the event-driven execution model.
- Manages dispatching, state transitions, and time events.
- Supports the Active Object pattern used by the demo.

**QSPY / QS Tracing**
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

---

## Validation & Observability
A key strength of this project is that the behavior is not treated as a black box.

Using **QSPY** over UART0 at **115200 baud**, the system can be observed live:
- button events
- state transitions
- time-event activity
- LED-related behavior

This makes it possible to validate that the software reacts exactly as intended, which is demonstrated in the following video:
