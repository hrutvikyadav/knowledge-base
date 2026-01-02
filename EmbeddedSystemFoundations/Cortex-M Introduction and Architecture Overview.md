---
id: Cortex-M Introduction and Architecture Overview
aliases:
  - Cortex-M Introduction and Architecture Overview
tags: []
---

# Cortex-M Introduction and Architecture Overview

## ARM Cortex Family
1. **Cortex-M**: Microcontroller profile
2. **Cortex-R**: Real-time profile
3. **Cortex-A**: Application profile

| Param | Application Processors | Real-time Processors | Microcontroller Processors |
| --------------- | --------------- | --------------- | --------------- |
| Design | High clock freq., Long pipeline, High performance, Multimedia support(Neon instruction set extension) | High clock freq., Long to medium pipeline, Deterministic(Low interrupt latency) | Shorter pipeline, Ultra low power, Deterministic(Low interrupt latency) |
| System Features | MMU, cache memory, ARM TrustZone security extension | MPU, cache memory, Tightly Coupled Memory | MPU, Nested Vectored Interrupt Controller, Wakeup interrupt controller, ARM TrustZone security extension in latest models |
| Target markets | Mobile computing, smart phones, energy efficient servers, high end processors | Industrial Microcontrollers, Automotives, Hard disk controllers, Baseband modem | Microcontrollers, Deeply embedded systems (sensors, MEMS, mixed signal IC), IoT |

### Glossary
- [ ] Fill in the glossary

## Cortex-M Family
`Cortex-M0`
`Cortex-M0+`
`Cortex-M1`
`Cortex-M3`
`Cortex-M4`
`Cortex-M7`
`Cortex-M23`
`Cortex-M33`

### Choice of processor based on scenario

> [!tip]
> Instruction set is a list of instructions available for a given processor/family of processors

## Cortex-M4 Instruction Set

1. `32-bit` ARM instruction set
    - Pre-1995
    - powerful 
    - required more program memory due to 32-bit instructions compared to 8-bit or 16-bit instructions
    - this was a problem because memory was/is expensive and having larger program memory consumed more power; thus the next instruction set was introduced
2. `16-bit` Thumb instruction set
    - at this point ARM had to provide support for both 32-bit and 16-bit instructions because the 16-bit thumb instruction set was only powerful enough to *handle some but not ALL the instructions*
    - However, the *`16-bit` thumb instruction set* provided better code density than its *`32-bit` ARM instruction set* counterpart
    - the downside of supporting both of these instruction sets was the performance overhead caused due to the need to switch between the two instruction sets; thus the next instruction set was introduced
3. `16-bit` and `32-bit` Thumb-2 instruction set
    - this instruction set was introduced to provide the best of both worlds by including both `16-bit` Thumb and `32-bit` Thumb instructions in the same instruction set
    - ARM was able to retain the high code density of the `16-bit` instruction set and the performance of the `32-bit` instruction set
    > [!hint]
    > Most Cortex-M processors only support the `Thumb-2` instruction set these days

### Data size definitions for ARM processors

| Terms | Size |
| -------------- | --------------- |
| Byte | `8-bit` |
| Half Word | `16-bit` |
| Word | `32-bit` |
| Double Word | `64-bit` |

## Overview of `Nucleo F303RE` board
Check the [ST website](https://www.st.com/en/evaluation-tools/nucleo-f303re.html)

## Reference documents
`ArmV7-ThumbInstruction-RefManual`
`stm32f303re-reference-guide`
`stm32f303re-user-guide`
`Cortex-M4-programming-manual`
`GDB+Cheat+Sheet`
`openocd-user-guide`

See - ./Resources/Section2

# Cortex-M Programmers Model
> `Cortex-M4` discussed here

## Programmers Model
### What does it mean?
The programmers model describes the tools, or the interface, we have available to us to write software for the processor. It includes the registers, memory, and instructions that we can use to write software.

For the Cortex-M4, we have:
2 Processor modes
- thread mode
    Used for application software execution
- handler mode
    Used for interrupt handling/exception handling
2 privilege levels
- privileged
    has access to all resources
- unprivileged
    has restricted access to resources
> We will use control register to decide privilege level for given piece of software

> [!tip]
> The thread mode can run in either privileged or unprivileged mode, while the handler mode always runs in privileged mode

For details refer to ./Resources/Section3/S2L7.pdf

### Switching between processor modes and privilege levels
[[Switching between CORTEX-M4 processor modes and privilege levels 2025-02-18 17.11.26.excalidraw]]

## General Registers and Process Specific Registers
GPRs - R0 - R12
MSP(Main Stack Pointer) - R13
> Used to store the main stack pointer (pointer to the top of the stack)
PSP(Process Stack Pointer) - R14
LR(Link Register) - R14
> Stores the return address when a subroutine is called i.e. after execution of the subroutine, the program counter(PC) is loaded with the value stored in LR
PC(Program Counter) - R15
> Stores the address of the next instruction to be executed

For details refer to ./Resources/Section3/S2L8.pdf

## Special Registers

For details refer to ./Resources/Section3/S2L9.pdf

## Lab session 1 General and Special Registers
## Lab session 2 xPSR and Control Registers

# Cortex-M Exception Model, Vector Table and VTOR

## Exception Model and Vector Table
...

## Vector Table Offset Register
Used to change where the Vector Table is located
- Useful when we want to execute custom handlers. We modify the vector table so that we can run our special hardfault handler i.e. make the harfault handler in the vector table point to some other location where we have our custom handler.
- Faster access if table is moved to SRAM instead of Flash memory.
- Dynamically change exception handlers based on program state.

