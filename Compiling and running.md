---
id: Compiling and running
aliases:
  - Compiling and running
tags: []
---

# Compiling and running

C is a compiled language which means that the source code you write is translated into machine code by a `compiler` before it is run.
> The output of the compiler is a `binary` code i.e. `executable` file which can be understood by the computer

Which compiler to use depends on the platform you are working on. For example, on `Linux` you can use `gcc` and on `Windows` you can use `cl` which is the `Microsoft C compiler`.
> This is because the actual `machine code` that the computer understands is different for different platforms, i.e. platform dependent

> [!info] When compiling with `gcc` a file with errors will sometimes produce an executable regardless. To make the compiler reject such programs, use the `-Wall` and `Werror` flags when compiling.

The C language provides an abstraction(`assembler`) over the various machine languages and the *compiler is responsible* for making sure that the *code compiled for your target platform* is #portable and *works on different platforms*.
> [!tip] A correct C program is portable between different platforms.
> portable means: wherever you run that program, its behavior should be the same

