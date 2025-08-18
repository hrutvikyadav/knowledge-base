---
id: Anatomy of a process
aliases:
  - Anatomy of a process
tags: []
---

# Anatomy of a process

## Anatomy intro
A Process is divided into sections when it is copied to memory from disk. Each of these sections play a role to aid the execution of the process
[[Process view in memory 2025-06-23 14.41.19.excalidraw]]

## Program vs Process

- When a program is run we get a process
- process **lives in memory**
- uniquely identified with ID
- The `IP`/PC points to the next instruction *in memory* to be executed 
    - For the current process, the `IP` points to the next instruction for that program
    - If a context switch happens ( either a switch to other process or kernel mode switch ) the value of the `IP` is saved like a checkpoint for later reference.
- Process Control Block (PCB)
    - stores information related to processes
    > [!hint]
    > updating the PCB is ==costly== (because it lives in memory and there is a cost associated with read and writes)
    > we need to be efficient when we decide to pay this cost

### Demo of running / debugging a process
[[Demo 1 2025-06-23 15.15.57.excalidraw]]

## Simple Process Execution

- Everything starts with the kernel deciding to schedule our program to be executed on a CPU core; before this step the cpu registers are blank.
- elf header of the program (in unix systems) has information on what address to start execution from, *this address* (i.e.) the first line of main function is *loaded into the cpu* `PC`/`IP`
    > [!tip]
    > register access also has some associated cost, we need to be efficient with this because the cost adds up.
- the CPU still does not know what is the actuall instruction at the address to which the `PC` points, so it will **fetch** the instruction at that address into the instruction Register (`IR`)
- now this fetched instruction will be **decoded**
- then comes the actual **execution** of the decoded machine code
- and finally the kernel instructs to increement the PC by the *word size* on that CPU(8 bytes for 64 bit CPUs) and we are ready to fetch the next instruction

### CPU related costs

- Registers X ns
- L1 X ns
- L2 (sometimes shared) X ns
- L3 (shared) X ns
- Memory X micro s
- Disk X ms

> [!hint]
> CPU read from memory usually happens in burst reads, which means if you want to read something at some address, the next 64 bits are read in burst mode. And all that data pierces through the L caches like a bullet.
> This is where caching comes in because the data that is **next to each other** is cached. Which means if your code is next to each other, increementing the PC and fetching the next instruction will be faster because it will be cached, it will only be cache miss if the code lives far away from the current position.

## Code/text section
The machine code(assembly) lives here.

## Data/static section
Data related to program is put here.
Has 2 parts -
1. Read only
    - constants
2. Read/write
    - initialized (variables)
    - uninitialized (variables)
This data is available for the lifetime of the process (static)

## Stack

- Every function call gets a stack frame allocated to it for storing local variables and calling functions from functions will just add more frames to the stack.
- This has the advantage of sequential read/writes because everything is next to each other on the stack.
- The stack in real time is implemented *from high to low* addresses and it shrinks with respect to addresses as it grows.
- The stack pointer points to the top of the stack ??and the base pointer points to the bottom??
- *Stack space is limited* look out for stack overflow.
> Many of the stack overflow bugs are due to leaving things on the stack and not cleaning up properly.

- In nested funtion calls, we need to save some things before we push a new stack frame, mainly the current base pointer and the current program counter need to be stored somewhere in order to return correctly later.
- The link register stores the program counter value for the caller function and the first thing in the stack frame will be the previous base pointer.

### Process execution with stack

- Function params are passed through registers, If a function has 4 params, 4 registers will be prepared for that function call.
    There is another register `ret` which is used by function calls to write the return value and the caller will read from that register to get the return value.
- ajsdfklaj

## Heap

- The heap is a dumping ground for dynamically(explicitly) allocated data that has to be accessed across multiple functions
- It grows *from low to high*
- The allocated memory remains untill explicitly cleaned up (ususally by deallocating in case of manual memory management and by GC in garbage collected languages)
    > This is a source of many memory related bugs. Especially if memory remains allocated even though no one is referencing it, this is called a memory leak
    > Another one is related to dangling pointers where the memory was deallocated even though someone was referencing it. This can further cause double free issues which is dangerous as it can crash the process.
- syscalls `malloc`, `free`, `new`, `calloc`
    > [!hint]
    > when allocating with malloc, along with the actuall data, some space is also used to store metadata about the allocation which is later used by free to determine how much to deallocate.
- 

### Pointers

- Points to a memory location in the heap or the stack (i.e. in the process addressable space)
- Pointer can live in any section in the processes memory layout
- Stores the address of the first byte
- Type of the pointer helps determine the size of the data being pointed to.

### Performance

Stack v/s Heap; who is faster and why
- Memory management is built into the stack which is more efficient.
- Heap has additional overhead of 
    1. Kernel mode switch (not context switch) to make syscalls
    2. Storing headers(metadata) is an additional write to memory which has some cost, along with that parsing the metadata during free, updating the metadata(for ref counting, and so on) during every access, which is all part of GC also adds additional cost with the heap.
- Heap is random
- Stack is *sequential* **but** space is limited

<!-- WRONG - Stack has another advantage of burst reads from memory which works nicely with CPU cache lines, which makes it even more fast and efficient -->

> [!hint]
> struct alignment is key to improve perf, along with general variable arrangement. Because ==**we love sequential r/w**==
>> *Again* caching works the same no matter whether data is read from stack or heap

> [!info]
> Escape analysis is a nice concept to read more about; which basically analyses code and if possible, avoids unnecessary heap allocations.
> This is done in Go and Java

### Program break

`brk` and `sbrk`
- another way to allocate and deallocate memory
- old
- not recommended to use

## Demo
Highlight everything discussed in above sections.

1. `top` lists the table of processes
2. **proc fs** of the kernel exposes info related to processes
3. user applications like `top` use this **proc fs** to display information for the user in a pretty format
4. `/proc/<PID>/maps` gives information about memory mapping for all sections we discussed

> [!hint]
> the memory layout of a process will also include the code sections for any linked libraries.
> because a library is just code at the end of the day, and that is how a program can access apis from any library.

> [!tip] 
> For each section, at minimum the OS has to allocate a block in memory equivalent to the ==page size== for that system (for 64 bit system that will be 8 bytes).
> page size and virtual memory are discussed in detail in the next section.

---

Practice things in the demo by taking a look at a complicated process like postgres
Read more assembly and gain more familiarity with cpu registers (SFR more than GPR)

