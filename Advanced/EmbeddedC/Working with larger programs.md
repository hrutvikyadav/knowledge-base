---
id: Working with larger programs
aliases:
  - Working with larger programs
tags: []
---

# Working with larger programs

## dividing program across multiple files, compiling all files together

## Compiling on the command line

## Makefiles

## Communication between files
- Functions from different files can communicate with each other through **(external)global variables**.
  > [!hint]
  > global variable are usually discouraged because they create tight coupling between files.
  > Which makes difficult to debug. Use sparingly; prefer `static`.
- The compiler compiles each module independently.
  > [!tip]
  > This means variable, type, and function definitions are ==not shared across multiple modules==.
  > It is up to the programmer to make sure compiler has sufficient information to compile what it needs to compile.
  > - When calling any function in a module make sure to include the prototype declaration at the top of the file.
  > - If any function in a module(same file) wants to access a variable, we can declare it as global i.e. outside all function scopes `int some_variable = 69;` at the top of the file.
  > - If another module wants to access the said variable we can use `extern` keyword, which is an extension to the concept of a global variable. We declare the same variable in the new module with the `extern` keyword like `extern int some_variable;`; after declaring, the variable from the other module can be accessed.

### global vs static
Sometimes we want a variable to be global but not external i.e. local to one module but available for use for all functions of that module.
This is where we use `static`.

## Using header files effectively
Use include files to group common information related to a program.
This will result in better code reuse, and all developers working on a program share the same information.

## Heap and Stack memory allocation
There are 3 main types of memory that we can use for a program.
1. Static
2. [[#Stack]]
3. [[#Heap]]

The choice we make will decide how and where things are stored.
It is important to understand differences and advantages/disadvantages of these three types of memory storage. It will help in designing efficient and scalable programs.

### Stack
In the memory layout of program, stack (see [[Anatomy of a process#Stack|Stack in OS]]) is a special section of memory used to temporarily store variables.
Stack is an LIFO data structure which is optimized for use by the OS. Whenever a function gets called, a frame gets allocated on the stack and local variables in the function are pushed to the stack.
It is easier to keep track of the stack than the heap.
Allocation and Deallocation happens automatically, no need to manually intervene.
Stack keeps growing and shrinking as data gets pushed and popped.
It cannot grow beyond a limit, so keep and eye out for stack overflow scenarios.

### Heap
It is a hierarchical data structure, a large pool of memory available for dynamic use (see [[Anatomy of a process#Heap|Stack in OS]])
We can allocate, deallocate as needed.
Memory needs to be managed manually.
When using heap, look out for memory leaks(forgot to free) and double free(attempt to free memory that is either not allocated *or* allocated to someone else) errors.
Has more overhead than stack.

### When to use stack v/s heap
- Use heap if:
    - you need to allocate a large block of memory for a huge data structure.
    - you need to reference a variable for a long time.
    - you need dynamic memory i.e. dealing with arbitrary data.
- Use stack if:
    - you need many variables for a small lifetime of the entire program, more specifically if the variable lifetime is tied to certain function.
    - size of data is known beforehand.

> [!tip]
> Whenever possible, try to use stack
> If more fine tuned control is needed go for the heap
