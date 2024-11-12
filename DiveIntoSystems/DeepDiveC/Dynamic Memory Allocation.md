---
id: Dynamic Memory Allocation
aliases:
  - Dynamic Memory Allocation
tags: []
---

# Dynamic Memory Allocation

Pointers are commonly used in C to dynamically allocate memory.
This is useful when you need to grow or shrink your data structures at runtime.
For example when you don't know the size of an array at compile time because lets say it depends on user input.

## Heap Memory
When a process is started, the OS allocates a block of memory to the process.
This is called the ==*program/process addressable memory space*==.
It has 4 regions:
1. **Text/Code segment** - contains the executable code.(instructions)
2. **Data segment** - contains the global and static variables.
3. **Stack** - contains the function call stack.
4. **Heap** - contains the dynamically allocated memory.

> [!info] The stack and heap are the regions which can grow and shrink during the execution of the program.

When a program dynamically requests memory at runtime, the heap provides a chunk of memory whose address must be assigned to a pointer variable(the pointer lives on stack)
[[Dynamic memory allocation C 2024-10-17 14.39.25.excalidraw]]
From here, you can use the usual pointer operations to access the memory.(dereference, pointer arithmetic etc.)

## Malloc and Free
The C standard library provides 2 functions to allocate and deallocate memory on the heap:
1. **malloc** - allocates a block of memory of a specified size and returns a pointer to the first byte of the block.
    ```c
    int *p = (int *)malloc(5 * sizeof(int));
    ```
    Here, `p` is a pointer to an integer. We are allocating memory for 5 integers.
    If the allocation is successful, `malloc` returns a pointer to the first byte of the block.
    If the allocation fails, `malloc` returns `NULL`.
    After the call to malloc in above example, the stack and heap could look like this [[malloc example 2024-10-17 15.04.03.excalidraw]]
2. **free** - deallocates the memory block pointed to by the pointer.
    ```c
    free(p);
    // it is good practice to set the pointer to NULL after freeing the memory
    p = NULL;
    ```
    Here, `p` is the pointer to the memory block allocated by `malloc`.
    After calling `free`, the memory block is deallocated and can be used for other purposes.
    > [!hint] The free function only needs the base address of the memory block to deallocate it. It doesn't need to know the size of the block.
    > This is because when allocating memory, malloc stores the size of the block in a data structure called the **heap metadata** which is located just before the base address of the block returned by malloc.

