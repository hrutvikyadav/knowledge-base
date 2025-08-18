---
id: Scripts_Raze
aliases:
  - Scripts_Raze
tags: []
---

# Scripts_Raze

## Intro
- Hi I am Hrutvik Yadav and I have 3 years of experience in embedded systems and full stack software; specifically in the domain of Battery and ECU testing
    - 3 yrs at Arbin
- This is a video demonstrating my work profile

## Overview

- Starting at the application layer, we have a cross-platform desktop client using React and Tauri. It serves as the user interface for test operators to test and monitor DUT
- This application communicates with a backend WebAPI built in ASP.NET using C#.
- The backend acts as a bridge between the client application and the embedded system.— exposing REST APIs that trigger socket-level communication with embedded devices on the same network.
- The embedded system control batteries electrical parameters and communicates with the ECU

<!-- - The embedded system communicates with batteries/ECU's, in order to manipulate battery's electrical parameters and monitor ECU data -->

## Embedded system

- The embedded system is powered by a STM32 microcontroller — specifically the STM32H743.
- The firmware, developed in-house using C++, is flashed into our embedded system.
- Our system allows users to test the battery packs in various real-world scenarios: charging, discharging, EV drive cycle simulations, and even sudden anomalies like thermal spikes or voltage irregularities.

## Day-to-day
My day-to-day work is hands-on across the stack. On the frontend, I build and refine the Tauri-based UI. On the backend, I write, debug, and review ASP.NET WebAPI code to ensure reliable communication.

At the firmware level, I develop for ARM architecture, focusing on protocol implementations — especially CAN, UDS, Smbus and MQTT 
