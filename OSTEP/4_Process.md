---
id: 4_Process
aliases:
  - 4_Process
tags: []
---

# The process abstraction

Process is a fundamental abstraction provided by the OS to achieve the goal of allowing user to run a program
A process is just a running program.
> Actually the process just sits on disk, and the OS springs it into action by loading it into memory and executing code.

Any useful system will be running multiple processes at a time -> think of a PC; running browser, editor, servers, and much more at once.
We don't stop to think whether the CPU is available, we just run a program.
The CPU is a resource, and we would like to make it available to a process when needed.

> [!warning] CRUX
> Our problem, then, is HOW TO PROVIDE THE **ILLUSION** OF MANY CPUS?
> Although there are only a few physical CPUs available, how can the
> OS provide the illusion of a nearly-endless supply of said CPUs?

The OS creates this **ILLUSION** by virtualizing the CPU.
By running one process, stopping and running another process, and so on, the illusion of virtual CPUs is created.
This the concept of *time sharing* and allows users to run as many concurrent processes as needed.

To implement such virtualization we need
- some low level machinery -> *mechanisms (low level protocol/method \_to interact with hardware\_???)*
    We will see **context switch** implementation example later
- some high level intelligence -> *policy (algorithms to arrive at a decision)*
    In this case a **scheduling policy** decides which program to run out of many possible ones, based on some information

