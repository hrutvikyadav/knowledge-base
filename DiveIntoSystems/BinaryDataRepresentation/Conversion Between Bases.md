---
id: Conversion Between Bases
aliases:
  - Conversion Between Bases
tags: []
---

# Conversion Between Bases

Each of the number systems are encountered in different contexts. Sometimes we may need  to convert between them.

## Binary <-> Hex
> [!tip] Each hex digit holds one of 16 unique values and 4 binary bits also represent (`2^4`) 16 values

| Binary | Hex |
| ------------- | -------------- |
| 0000 | 0 |
| 0001 | 1 |
| 0010 | 2 |
| 0011 | 3 |
| 0100 | 4 |
| 0101 | 5 |
| 0110 | 6 |
| 0111 | 7 |
| 1000 | 8 |
| 1001 | 9 |
| 1010 | A |
| 1011 | B |
| 1100 | C |
| 1101 | D |
| 1110 | E |
| 1111 | F |

1. Now, to convert from hex -> binary, simply replace the binary value for the hex number
   Example `0xB491` to binary is calculated as -
   B    4    9    1
   1011 0100 1001 0001
   
   Hence, `0xB491` in binary is `0b1011010010010001`
2. To convert from binary -> hex,
   - Split the bits into chunks of 4 from *right to left*, if the last chunk does not have 4 bits, you can pad zeros
   - Now replace corresponding hex values
   Example 0b1111011001 to hex is calculated as -
   Split     <---------
   = 0011 1101 1001
     ^^ padding
        3    D    9

    Hence, `0b1111011001` in hex is `0x3D9`

## Decimal <-> Binary/Hex

### X -> Decimal
To convert any base number to decimal, label digits from right to left (0 to N) then apply the formula ![[Pasted image 20241109115217.png]]
> here B is the base in which the original number is and N is the number of digits

### Decimal -> Hex
To convert from decimal to hex
Ask how many multiples of a given power of 16 fit into the initial value?, i.e. divide the initial value by a power of 16, if quotient is 0, move on to the next smaller power of 16, else put the quotient in that powers place, and repeat with the remaining value.
Example - converting 420 into hex

| Power | Value | 
| ------------- | -------------- |
| 16^0 | 1 | 
| 16^1 | 16 | 
| 16^2 | 256 | 
| 16^3 | 4096 | 
| 16^4 | 65536 | 
| 16^5 | 1048576 | 
Take the highest power which is < inital value, in this case, 4096 > 420( initial value ) and 256 < 420. So the resulting hex number will have 3 digits

`d2`    `d1`    `d0`
??    ??    ??

420/256 = 1; remaining = 420 - 256\*1 = 164
`d2`    `d1`    `d0`
1     ??    ??

164/16 = 10; remaining = 164 - 16\*10 = 4
`d2`    `d1`    `d0`
1     A     ??

4/1 = 4; remaining = 4 - 1\*4 = 0
`d2`    `d1`    `d0`
1     A     4

Thus `420` -> `0x1A4`

### Decimal -> Binary
To convert decimal to binary
Ask does a power of 2 fit into the initial value. If yes then put one in that powers place and repeat with remaining value, else put zero and move on to next power
Example - convert 420 to binary

| Power | Value | 
| ------------- | -------------- |
| 2^0 | 1 | 
| 2^1 | 2 | 
| 2^2 | 4 | 
| 2^3 | 8 | 
| 2^4 | 16 | 
| 2^5 | 32 | 
| 2^6 | 64 | 
| 2^7 | 128 | 
| 2^8 | 256 | 
Start at the smallest power of 2 - 1 > initial value, in this case, 2^8 - 1 is 127 which is < 420( initial value ) and 2^9 - 1 is 511 > 420

`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
??    ??    ??    ??    ??    ??    ??    ??    ??

256 fits in 420 = 1; remaining 420 - 256 = 164
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     ??    ??    ??    ??    ??    ??    ??    ??

128 fits in 164 = 1; remaining 164 - 128 = 36
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     ??    ??    ??    ??    ??    ??    ??

64 fits in 36 = 0; remaining 36
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     ??    ??    ??    ??    ??    ??

32 fits in 36 = 1; remaining 36 - 32 = 4
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     ??    ??    ??    ??    ??

16 fits in 4 = 0; remaining 4
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     0     ??    ??    ??    ??

8 fits in 4 = 0; remaining 4
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     0     0     ??    ??    ??

4 fits in 4 = 1; remaining 4 - 4 = 0
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     0     0     1     ??    ??

2 fits in 0 = 0;
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     0     0     1     0     ??

1 fits in 0 = 0;
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     0     0     1     0     0 

Thus `420` -> `0b110100100`

To convert decimal to binary ==we can also use repeated division==
This produces the resulting number from right to left (`d0` to `dN`)
Take the initial value, if it
- is even, put 0 in `d0`'s place
- is odd, put 1 in `d0`'s place
divide by 2, then repeat with the quotient and next digit; terminate once the quotient reaches 0

Example convert 420 to binary with repeated division by 2

420 is even, 420/2 = 210
??    `d0`
??    0
210 is even, 210/2 = 105
??    `d1`    `d0`
??    0     0
105 is odd, 105/2 = 52
??    `d2`    `d1`    `d0`
??     1    0     0
52 is even, 52/2 = 26
??    `d3`    `d2`    `d1`    `d0`
??    0     1     0     0
26 is even, 26/2 = 13
??    `d4`    `d3`    `d2`    `d1`    `d0`
??    0     0     1     0     0
13 is odd, 13/2 = 6
??    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
??    1     0     0     1     0     0
6 is even, 6/2 = 3
??    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
??    0     1     0     0     1     0     0
3 is odd, 3/2 = 1
??    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
??    1     0     1     0     0     1     0     0
1 is odd, 1/2 = 0
`d8`    `d7`    `d6`    `d5`    `d4`    `d3`    `d2`    `d1`    `d0`
1     1     0     1     0     0     1     0     0

Thus 420 converted to binary (using repeated division) is `0b110100100`
