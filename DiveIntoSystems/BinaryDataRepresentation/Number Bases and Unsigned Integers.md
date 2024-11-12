---
id: Number Bases and Unsigned Integers
aliases:
  - Number Bases and Unsigned Integers
tags: []
---

# Number Bases and Unsigned Integers

## Decimal Numbers

## Binary Numbers

## Hexadecimal Numbers

## Storage Limitations
Theoretically we can represent an infinite amount of unsigned numbers with any of the number systems, however in practice, a Programmer has to decide the number of bits needed to store a value before declaring a variable to hold that value. This is imp for the following reasons
- Before storing a value, a program must allocate storage space for it. In C, declaring a variable tells the compiler how much memory it needs based on its type.
- Hardware storage devices have finite capacity. Whereas a system’s main memory is typically large and unlikely to be a limiting factor, storage locations inside the CPU that are used as temporary "scratch space" (i.e., registers) are more constrained. A CPU uses registers that are limited to its word size (typically 32 or 64 bits, depending on the CPU architecture).
- Programs often move data from one storage device to another (e.g., between CPU registers and main memory). As values get larger, storage devices need more wires to communicate signals between them. Hence, expanding storage increases the complexity of the hardware and leaves less physical space for other components

The number of bits then decide the range of values that can be represented by that variable. Attempting to store a value larger than this range will lead to **overflow** and will cause the value to wrap around the endpoints of the range.
For example, subtracting 1 from 0 will result in the maximum value (in case of an unsigned integer)
[[Representation of finite numbers 2024-11-07 11.15.13.excalidraw]]

> [!tip] The largest possible unsigned values that a N bit binary number can store is `2^n - 1` (including 0)

