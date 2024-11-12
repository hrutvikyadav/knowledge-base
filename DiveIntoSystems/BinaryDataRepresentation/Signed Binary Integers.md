---
id: Signed Binary Integers
aliases:
  - Signed Binary Integers
tags: []
---

# Signed Binary Integers

As the space used by variable is finite, to store signed numbers it needs to account for `-ve values` , `0` and `+ve values`
Manipulating a signed number also involves a process for negating a number
Number systems are usually designed to represent an equal split of negative and non-negative(including 0) numbers
Signed number encodings use one bit to determine if the number is non-negative(0) or negative(1). Usually this is the leftmost bit, also called the most significant bit(`MSB`)

The 2 encodings presented are signed magnitude and two's complement

## Signed magnitude
In this encoding, the `MSB` is exclusively used to determine the sign of the number and does not contribute any value to the overall number
This has some disadvantages, so the next encoding is used in today's world

## 2's complement
In this encoding, the `MSB` determines the sign of the number however, if the bit is 1, then it contributes the highest absolute value to the sum, otherwise it ends up contributing nothing to the overall sum
The conversion of N bit value to decimal using 2's complement is similar to [[Conversion Between Bases#X -> Decimal]] with the difference that if the `MSB` is 1, rather than contributing a positive value to the sum, it contributes a negative value to the sum
![[Pasted image 20241112121542.png]]

### Negation
Negating a 2's complement representation of a number is a little complicated than negating a signed magnitude representation(which involves just flipping the `MSB`)
However there is a shortcut -
1. Flip all the bits of the given number
2. Add one to the result

This gives the negative equivalent of the original number (in 2's complement)

> [!hint] A sequence of bits can be interpreted in more than one way.
> The programmer has to instruct the compiler how to interpret a given sequence of bits
> Unsigned representation can represent ==2^N== non-negative values whereas signed i.e. 2's complement representation can represent ==2^N== i.e. (2^N /2) negative and (2^N /2) non-negative values.
> [[Range of values represented by N bits 2024-11-12 14.12.14.excalidraw]]
>> [!tip] In `C` this can be achieved using the `unsigned` and `signed` modifiers
>> For `printf`, there are separate format specifiers for both unsigned and signed

## Sign extension
If we want to perform arithmetic on two numbers that are stored as different bit variables, we need to perform sign extension on the number with the lesser bits.
This involves taking the `MSB` of this number and repeating it as many times as needed to match the bits of the other number
> [!warning] Unsigned Zero Extension
> This(repeating `MSB`) only holds true for signed integers. For unsigned integers, we need to perform *Zero extension* as the `MSB` here actually contributes to the value of the number and is not used to determine the sign.

