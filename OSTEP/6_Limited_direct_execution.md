---
id: 6_Limited_direct_execution
aliases:
  - 6_Limited_direct_execution
tags: []
---

# 6_Limited_direct_execution

Here we discuss the mechanism used to overcome the challenges faced when implementing CPU virtualization while maintaining control over CPU.
Without limited direct execution, the process execution is basically just the OS preparing the resources required for the process, load the program in memory, and start executing the main routine then return from main at which point the OS can clean up associated resources.
However, this creates some issues for virtualization->

1. The first is simple: if we just run a program, how can the OS make sure the program doesn’t do anything that we don’t want it to do, while still running it efficiently?
    The program might want to access some more resources as it progresses, so how can we let it perform such restricted operations while at the same time ensuring not to hand over complete system control to the process?
    This is accomplished with a few facets, specifically - software support -> system calls, hardware support -> user mode v/s kernel mode, trap table, return from trap instruction. Also the OS needs to worry about input (param) sanitization. All this together forms the *limited* part in the *direct execution*, thus called as **limited direct execution** protocol.
    The LDE works in 2 phases. 1st during boot, the OS need to setup the trap table, and 2nd during execution of a process, the user process calls a number associated with the desired system call, the trap will simultaneously jump to the code in kernel and raise the execution level to kernel mode. Before the jump, important parts of the user process needs to be saved to known registers (the kernel stack in x86), so that the OS can resume later when the trap routine is done.
    > system calls v/s procedure calls -> syscall is a procedure call in the C library which other people have written for us, in assembly, where the trap hardware instruction call resides along with other logic based on the agreed convention between the kernel and the library.
2. The second: when we are running a process, how does the operating system stop it from running and switch to another process, thus implementing the time sharing we require to virtualize the CPU? i.e. how to *regain control* from a running program?
    One way is cooperative scheduling approach, where processes are assumed to be well behaved, and the OS hopes that such process will *yeild* back to the OS, directly or indirectly.
    Now, how can the OS gain control **without cooperation** of processes? This is achieved by simply using *timer interrupt*. Recall that an interrupt is basically an execption with a high priority, and when a hw or sw interrupt arrives, normal execution is stopped to immediately service the interrupt handler routine.


