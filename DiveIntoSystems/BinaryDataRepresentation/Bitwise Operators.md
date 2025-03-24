---
id: Bitwise Operators
aliases:
  - Bitwise Operators
tags: []
---

# Bitwise Operators

## Bitwise AND

## Bitwise OR

## Bitwise XOR

## Bitwise NOT

## Bit shifting
Bit shifting involves shifting the bits of an operand by a given number of places. Overflowing bits are truncated and are replaced with 0's or 1's depending on which kind of shifting is applied.
Depending on whether the variable is being left shifted or right shifted, appropriate kind of bit shifting can be performed. In `C` depending on whether the variable was declared with the `signed` or `unsigned` qualifier, *the right kind of shifting is automatically performed*.

### Left shift
This will shift the operand to the left by the given number of spaces and append 0's to the result. The bits that overflow to the left are truncated.
> [!warning] Left shifting signed integers can lead to undefined behaviour.
> Avoid left shifting with signed integers.
> Don't left shift by a number greater than the number of bits used to store the variable.

### Right shift
This will shift the operand to the right by the given number of spaces and prepend bits to the result. The bits that overflow to the right are truncated.

#### Logical shifting
This will simply shift the bits and prepend 0's. This is used for unsigned integers.

#### Arithmetic shifting
This will shift the bits and prepend 0's or 1's depending on the `MSB` (whatever the `MSB` it gets prepended repeatedly). This is used for signed integers.

