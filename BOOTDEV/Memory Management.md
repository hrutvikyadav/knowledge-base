---
id: Memory Management
aliases:
  - Memory Management
tags: []
---

# Memory Management

Goals:

1. **Understand how and where programs store data in memory**. Variables, functions and objects don't get to exist for free. Where do they live as your code runs?
2. **Learn how to make programs more efficient**. Most performance related problems in backend software are memory related (at least in my experience). Learn how it all works so you can troubleshoot and optimize.
3. **Practice programming in a lower-level language**. C gets you much closer to the hardware than Python, JavaScript, or Go. By writing C, you'll learn a lot about how software works closer to the metal.
4. **Learn about garbage collection (and build your own)**. You will likely work in a garbage collected language at some point, whether that's Python, Go, JavaScript or something else. Best to understand what trade-offs are being made.

## C Basics

### Format specifiers
Here are some common ones:
1. `%d` - digit(integer)
2. `%f` - float
3. `%c` - character
4. `%s` - string(char *)

> Multiline strings are not supported in C. You can use a series of strings separated by whitespace to achieve the same effect.
```c
char *some_string = "This is a "
              "multiline "
              "string.";
```

### Variables
Variable redeclaration is not allowed in C. You can't declare a variable with the same name and same/different type in the same scope.
You can reassign a variable, but you can't change its type.
To declare a variable whose value will not change, use the `const` type qualifier.

### Functions

In C:
- `.c` files contain the implementation i.e. C code.
- `.h` files contain the function prototypes i.e. the function signatures.

To import code from another file, we include the header file.
If `exercise.h` file in included in `exercise.c` file and
`exercise.h` file is also included in `main.c` file, then
we can use functions implemented in `exercise.c` file, in `main.c` file.
> [!warning] a potential issue you might run into: multiple inclusions. If the same header file gets included more than once, you can end up with some nasty errors caused by redefining things like functions or structs.
> To prevent this, you can either use `#pragma once` at the top of your header file, or use `#ifndef` and `#define` guards.
>> [!info] One simple solution (and the one we'll use for the rest of this course) is `#pragma once`. Adding this line to the top of a header file tells the compiler to include the file only once, even if it's referenced multiple times across your program.

### Arithmetic Operators
> [!hint] The `/` operator performs integer division if both operands are integers. To perform floating-point division, at least one of the operands must be a floating-point number.

### Unit Testing
We will introduce testing with the [*munit*](https://nemequ.github.io/munit/) framework.

### Type sizes
Size of a type in C is not guarenteed to be the same on all platforms. To get the size of a type, use the `sizeof` operator.

However, some types are guaranteed to have a fixed size:
- `char` - 1 byte
    > [!hint] `char` is guaranteed to be 1 byte, but it may be signed or unsigned. To ensure that `char` is unsigned, use `unsigned char`.

> [!tip] The size_t type is a special type that is guaranteed to be able to represent the size of the largest possible object in the target platform's address space (i.e. can fit any single, non-struct value inside of it). It's also the type that sizeof returns

## Structs

## Pointers

## Enums

## Unions

## Stack and Heap

## Advanced Pointers

## Stack Data Structure

## Objects

## Refcounting GC

## Mark and Sweep GC

