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
    Answers the question How to accomplish X
- some high level intelligence -> *policy (algorithms to arrive at a decision)*
    In this case a **scheduling policy** decides which program to run out of many possible ones, based on some information
    Answers the question Which out of A, B, C

> [!tip]
> Policies are decoupled from mechanisms so that one can be swapped without concerning other

A process can be described by its **machine state** -
    - what resources the program can r/w during execution?
    - what resources does the running program depend on?
    - Ex: 
        - memory that the process can address (*address space*),
        - registers (many CPU instructions use registers), ...
          some of these are special like the PC, SP, and so on.
        - I/O devices like disk

## Process API
Generic Interface description of most modern systems
- Create
- Destroy
- Misc control
- Wait
- Status

Before actually running something the OS needs to do a few things
Loading a program from disk to memory
i.e. program to process
1. Load code and static data from disk to address space of the process in the memory
    eager v/s lazy loading - modern os prefer lazy loading
    -> Lazy loading directly ties into paging and swapping covered later in Memory virtualization

2. Some mem alloc for runtime stack, stack init (main with args), then give that to the process
3. Also alloc some heap mem for dynamically allocated data.
4. Some other init for I/O related resources. Ex. in unix each process always has 3 fds open for input, error, and output streams
Now the OS is ready to actually run the program by jumping to program entry point via a special mechanism
thus transferring CPU control to the program

## Process states
1. Running
2. Ready
3. Blocked

## Data Structures
Just like any other program, the os has a few key data structures which are used to keep track of various information.
An example might be a `process list` data structure used to keep track of ready processes and process specific information

```c
// the registers xv6 will save and restore 
// to stop and subsequently restart a process 
struct context { int eip; int esp; int ebx; int ecx; int edx; int esi; int edi; int ebp; }; 

// the different states a process can be in 
enum proc_state { UNUSED, EMBRYO, SLEEPING, RUNNABLE, RUNNING, ZOMBIE }; 

// the information xv6 tracks about each process 
// including its register context and state 
struct proc { // This is also sometimes called as PCB or process descriptor
    char *mem; // Start of process memory 
    uint sz; // Size of process memory 
    char *kstack; // Bottom of kernel stack for this process 
    enum proc_state state; // Process state 
    int pid; // Process ID 
    struct proc *parent; // Parent process 
    void *chan; // If !zero, sleeping on chan 
    int killed; // If !zero, has been killed 
    struct file *ofile[NOFILE]; // Open files 
    struct inode *cwd; // Current directory 
    struct context context; // Switch here to run process 
    struct trapframe *tf; // Trap frame for the current interrupt 
};
```

    With this conceptual view in mind, we will now move on to the nitty-gritty: the low-level mechanisms needed to implement processes, and the higher-level policies required to schedule them in an intelligent way.
    By combining mechanisms and policies, we will build up our understanding of how an operating system virtualizes the CPU. 
