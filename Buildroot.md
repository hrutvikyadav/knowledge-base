---
id: Buildroot
aliases:
  - Buildroot
tags: []
---

# Buildroot

Cross platform toolchain for generating embedded linux custom distro targetting various controller boards 

## Why use embedded linux and the alternatives

1. Bare Metal programming
    - little to no sw overhead
    - low power consumption
    - high control of hardware (direct register access)
    - single purpose hardware dependent applications
    - strict timing control
    > Can be done on 8, 16 and some 32 bit microcontrollers
2. RTOS
    - scheduler overhead
    - high power microcontroller needed
    - high control of hardware (direct register access)
    - multithreading, some common libraries
    - multitasking (networking, GUI's etc)
3. Embedded linux
    - large overhead (scheduler, memory management, background tasks and so on)
    - microprocessor usually required often along with external RAM+NVM
    - lesser direct hardware control due to abstraction layers or files
    - multiple threads and processes, many common libraries available (highly portable code)
    - multiple complex tasks like networking, filesystem, GUI's, etc

## Get started

Clone the git repo to follow latest development
`git clone https://gitlab.com/buildroot.org/buildroot.git .`

Run `make list-defconfigs` to view the list of provided configurations.

Then run `make <your-board-here>` to generate a config file for your choosen board

Finally run `make` to generate the output image
