---
id: Bit Manipulation
aliases:
  - Bit Manipulation
tags: []
---

# Bit Manipulation

## Binary numbers and bits
## Bitwise operators (logical and shifting)
## Bitmasks and bitfields

### Bitmasks in C Programming

#### What is a Bitmask?

A bitmask is data used for bitwise operations - a bit pattern with some bits set to 1 and others set to 0. It allows you to manipulate multiple bits in a byte simultaneously using a single bitwise operator.

#### Why Use Bitmasks?

**Memory Efficiency**: Instead of creating 32 separate boolean variables to store state data, you can use a single 32-bit integer where each bit represents a different true/false value.

**Example Use Case**: 
- Binary: `0 0 0 0 1 0 1`
- Reading right to left: bit 0 is true (first variable), bit 1 is false (second variable), bit 2 is true (third variable)

**Applications**:
- Storing multiple state flags in a single integer
- Controlling hardware through port values
- Color representation (e.g., 3 bits for RGB channels)
- Configuration settings in embedded systems

#### Bitmask Operations

##### 1. Turning Bits ON (Using OR `|`)

To set specific bits to 1 while leaving others unchanged:

```c
int flags = 15;      // Binary: 0000 1111
int mask = 182;      // Binary: 1011 0110
flags = flags | mask; // Result: 1011 1111
```

**Rule**: 
- Any bit OR 0 = itself (unchanged)
- Any bit OR 1 = 1 (set to 1)

The 1s in the mask will set the corresponding bits in flags to 1.

##### 2. Turning Bits OFF (Using AND `&` with NOT `~`)

To clear specific bits (set to 0) while leaving others unchanged:

```c
int flags = 15;           // Binary: 0000 1111
int mask = 182;           // Binary: 1011 0110
flags = flags & ~mask;    // Result: 0000 1001
```

**Rule**:
- Any bit AND 1 = itself (unchanged)
- Any bit AND 0 = 0 (cleared)

The negation (`~`) flips all bits in the mask. The 1s in the original mask become 0s, clearing those bits in flags.

##### 3. Toggling Bits (Using XOR `^`)

To flip bits (1→0, 0→1) while leaving others unchanged:

```c
int flags = 15;        // Binary: 0000 1111
int mask = 182;        // Binary: 1011 0110
flags = flags ^ mask;  // Result: 1011 1001
```

**Rule**:
- 1 XOR b = 0 if b is 1, or 1 if b is 0 (toggled)
- 0 XOR b = b (unchanged)

All bits set to 1 in the mask will toggle the corresponding bits in flags.

##### 4. Checking Bit Values (Using AND `&`)

To check if a specific bit is set:

```c
int mask = 2;  // Binary: 0010 (checking bit 1)

if ((flags & mask) == mask) {
    // Bit 1 is set to 1
}
```

**Important**: Don't use `if (flags == mask)` as other bits will make the comparison fail. You must mask the other bits first using AND.

#### Best Practices

1. **Mask Width**: A bitmask should be at least as wide as the value it's masking (e.g., 32-bit mask for 32-bit integer)

2. **Constant Masks**: Define masks as constants for readability:
   ```c
   #define SPEAKER_BIT 0x02  // Bit 1 for speaker control
   ```

3. **Think in Binary**: Understand the binary representation of your values when working with bitmasks

4. **Visualization**: Think of 0s in the mask as "opaque" (hiding bits) and 1s as "transparent" (showing/affecting bits)

#### Common Patterns

| Operation | Operator | Pattern | Effect |
|-----------|----------|---------|--------|
| Set bits | OR (`\|`) | `flags \| mask` | Sets bits where mask has 1s |
| Clear bits | AND + NOT (`& ~`) | `flags & ~mask` | Clears bits where mask has 1s |
| Toggle bits | XOR (`^`) | `flags ^ mask` | Flips bits where mask has 1s |
| Check bits | AND (`&`) | `(flags & mask) == mask` | Tests if bits are set |

#### Real-World Example: IBM PC Hardware Control

To turn on the internal speaker on an IBM PC, you need to set bit 1 while leaving other hardware control bits unchanged:

```c
#define SPEAKER_MASK 0x02
hardware_port = hardware_port | SPEAKER_MASK;  // Turn speaker on
```

This is more efficient and safer than reading, modifying, and writing back individual values.

### Data Packing with Bitwise Operators

#### Overview

Data packing is the technique of storing multiple values within the bits of a single integer variable to conserve memory. This is especially valuable in embedded systems where memory is limited and large tables of state data need to be stored efficiently.

#### Why Pack Data?

**Memory Efficiency**: If you don't need the entire byte or integer to represent data, you can pack multiple values into a single variable.

**Use Cases**:
- Boolean flags (true/false conditions) - only need 1 bit each
- Small integer ranges - can use fewer than 32 bits
- Large tables with many state values
- Hardware register programming in embedded systems

**Memory Savings Example**:
- Traditional approach: 5 separate integers = 5 × 4 bytes = 20 bytes
- Packed approach: 1 integer with packed bits = 4 bytes (80% memory reduction)

#### Two Methods for Data Packing in C

1. **Bitwise Operators with Bitmasks** (covered in this section)
   - Uses unsigned int or long variables
   - Manipulates bits using bitwise operators
   - More awkward but widely used in existing codebases
   - Must be recognized when reading legacy code

2. **Bit Fields** (covered in next lecture)
   - Uses structure syntax
   - Easier and more readable
   - Access like normal structure members

#### Practical Example: Packing Five Values

**Requirements**:
- `f1`, `f2`, `f3`: Boolean flags (3 bits total)
- `type`: Integer ranging from 1-255 (8 bits)
- `index`: Integer ranging from 0-100,000 (18 bits)
- **Total**: 29 bits (fits in a 32-bit integer)

**Bit Layout**:
```
[3 unused][f1][f2][f3][type: 8 bits][index: 18 bits]
|-------- 32 bits total --------|
```

**Variable Declaration**:
```c
unsigned int packed_data;  // Holds all 5 values in 32 bits
```

#### Setting Bits in Packed Data

##### Setting the Type Field to a Specific Value

**Method 1**: Direct shift and OR (assumes field is already 0)
```c
packed_data = packed_data | (7 << 18);
```

**Method 2**: Setting to variable `n` (0-255) with validation
```c
packed_data = packed_data | ((n & 0xFF) << 18);
```

##### Clearing Before Setting (Safe Method)

To set a field that might contain existing data:

**Step 1**: Clear the type field (set to 0)
```c
packed_data &= ~(0xFF << 18);
```
- Creates a mask with 0s in the type field positions
- 1s everywhere else preserve other bits

**Step 2**: Set the new value
```c
packed_data |= ((n & 0xFF) << 18);
```

**Combined Operation**:
```c
// Clear type field and set new value in one statement
packed_data = (packed_data & ~(0xFF << 18)) | ((n & 0xFF) << 18);
```

**Why This is Complex**: 
- Must manually calculate bit positions
- Need to create appropriate bitmasks
- Risk of corrupting adjacent fields
- Difficult to read and maintain

#### Reading Bits from Packed Data

Extracting values is simpler than setting them.

**General Pattern**:
1. Shift the desired field to the lower order bits
2. AND with a mask to isolate only those bits

**Extracting the Type Field**:
```c
n = (packed_data >> 18) & 0xFF;
```
- Shifts type field to positions 0-7
- Masks with `0xFF` to get only the 8 bits

#### Real-World Example: Color Packing

Storing RGB color values in a single unsigned long (24 bits used):

**Bit Layout**:
```
[8 unused][Blue: 8 bits][Green: 8 bits][Red: 8 bits]
```

**Code Implementation**:
```c
#define BYTE_MASK 0xFF

unsigned long color;         // Packed color value
unsigned char red, green, blue;

// Extract individual color components
red   = color & BYTE_MASK;              // Lower 8 bits
green = (color >> 8) & BYTE_MASK;       // Middle 8 bits
blue  = (color >> 16) & BYTE_MASK;      // Upper 8 bits
```

**Advantages**:
- Stores 3 color values in 1 variable
- Efficient for large arrays of colors
- Common in graphics and display drivers

#### Common Bit Positions and Shift Values

| Field Location | Shift Left (Write) | Shift Right (Read) |
|----------------|--------------------|--------------------|
| Bits 0-7 | `<< 0` | `>> 0` |
| Bits 8-15 | `<< 8` | `>> 8` |
| Bits 16-23 | `<< 16` | `>> 16` |
| Bits 24-31 | `<< 24` | `>> 24` |

#### Best Practices

1. **Define Constants**: Create named constants for bit positions and masks
   ```c
   #define TYPE_SHIFT 18
   #define TYPE_MASK 0xFF
   #define INDEX_SHIFT 0
   #define INDEX_MASK 0x3FFFF  // 18 bits
   ```

2. **Clear Before Setting**: Always clear a field before setting new values to avoid corruption

3. **Document Bit Layouts**: Comment the bit structure clearly in your code
   ```c
   // packed_data layout:
   // Bits 0-17:  index (18 bits)
   // Bits 18-25: type (8 bits)
   // Bits 26-28: f3, f2, f1 flags
   // Bits 29-31: unused
   ```

4. **Use Unsigned Types**: Always use `unsigned int` or `unsigned long` to avoid sign extension issues

5. **Validate Input**: AND input values with appropriate masks before shifting

#### Advantages vs Disadvantages

**Advantages**:
- Maximum memory efficiency
- Works on any C compiler
- Fine-grained control over bits
- Used in hardware register programming

**Disadvantages**:
- Complex and error-prone code
- Difficult to read and maintain
- Manual calculation of bit positions
- Easy to corrupt adjacent fields
- No compiler checking for field boundaries

#### When to Use This Technique

**Good Use Cases**:
- Memory-constrained embedded systems
- Large lookup tables with many small values
- Hardware register manipulation
- Protocol implementations with bit-level fields
- Legacy code maintenance

**Consider Bit Fields Instead When**:
- Code readability is important
- Frequent access to packed fields
- Structure-like access is desired
- Compiler support for bit fields is available

#### Comparison with Bit Fields (Preview)

While bitwise operators give you maximum control, bit fields (covered next) provide:
- Structure member syntax (easier access)
- Compiler handles bit manipulation
- More readable and maintainable code
- Automatic boundary checking

However, bitwise operators remain essential for understanding low-level operations and working with hardware registers in embedded systems.

### Bit Fields for Data Packing

#### Overview

Bit fields provide a cleaner, more readable alternative to bitwise operators for packing data. They allow you to specify individual bits or groups of bits as named members within a structure, making code much easier to write and maintain.

#### What are Bit Fields?

A bit field is a structure member that specifies exactly how many bits should be allocated for storing that member. Instead of manually manipulating bits with operators and masks, you define the bit width in the structure declaration and access members like normal structure fields.

**Key Advantages over Bitwise Operators**:
- No manual shifting required
- No bitmask calculations needed
- Cleaner, more readable syntax
- Compiler handles bit manipulation automatically
- Access members using standard dot notation

#### Basic Syntax

**Format**:
```c
struct structure_name {
    unsigned int member_name : bit_width;
};
```

**Components**:
- `unsigned int` or `signed int` - explicitly specify signedness (recommended)
- `member_name` - variable name for the bit field
- `:` - colon separator
- `bit_width` - integer constant (0 to platform int size, typically 0-32)

#### Practical Example: Recreating the Packed Data Structure

Recall the example from the previous lecture with 5 values in 29 bits:

```c
struct packed_struct {
    unsigned int       : 3;   // Unnamed - padding (3 unused bits)
    unsigned int f1    : 1;   // Flag 1 (1 bit)
    unsigned int f2    : 1;   // Flag 2 (1 bit)
    unsigned int f3    : 1;   // Flag 3 (1 bit)
    unsigned int type  : 8;   // Type value 0-255 (8 bits)
    unsigned int index : 18;  // Index value 0-100,000 (18 bits)
};                            // Total: 32 bits
```

**Bit Layout**:
```
[3 unused][f1][f2][f3][type: 8 bits][index: 18 bits]
```

#### Using Bit Fields

##### Declaration and Initialization

**Simple Declaration**:
```c
struct packed_struct packed_data;
```

**Initialization with Values**:
```c
// Using designated initializers
struct packed_struct packed_data = {
    .f1 = 1,
    .f2 = 0,
    .f3 = 1,
    .type = 7,
    .index = 5000
};

// Or in order (less recommended)
struct packed_struct packed_data = {1, 0, 1, 7, 5000};
```

##### Setting Bit Field Values

**Simple Assignment** (no shifting or masking needed):
```c
packed_data.type = 7;           // Set type to 7
packed_data.index = 50000;      // Set index to 50000
packed_data.f1 = 1;             // Set flag f1 to true
```

**Variable Assignment**:
```c
unsigned int n = 9;
packed_data.type = n;           // Set type to n
```

**Automatic Truncation**:
If you assign a value too large for the bit field, only the lower bits that fit are stored:
```c
packed_data.type = 266;         // type is 8 bits (max 255)
                                // Only lower 8 bits stored: 10 (266 & 0xFF)
```

##### Reading Bit Field Values

**Direct Access**:
```c
unsigned int bit = packed_data.type;    // Automatically extracted and shifted
```

**In Expressions**:
Bit fields automatically convert to integers in expressions:
```c
unsigned int i = packed_data.index / 5 + 1;

if (packed_data.f2 == 1) {
    // Flag f2 is set
}

int result = packed_data.type * 2;
```

#### Comparison: Bit Fields vs Bitwise Operators

| Operation | Bitwise Operators | Bit Fields |
|-----------|-------------------|------------|
| **Setting** | `packed_data = (packed_data & ~(0xFF << 18)) \| ((n & 0xFF) << 18);` | `packed_data.type = n;` |
| **Reading** | `n = (packed_data >> 18) & 0xFF;` | `n = packed_data.type;` |
| **Readability** | Complex, requires bit knowledge | Clear, self-documenting |
| **Maintenance** | Error-prone, hard to modify | Easy to update |
| **Compiler Support** | Universal | Universal (C89+) |

#### Special Features

##### Unnamed Bit Fields (Padding)

Use unnamed bit fields for padding or alignment:
```c
struct example {
    unsigned int       : 3;   // 3 bits of padding
    unsigned int value : 5;   // 5-bit value
};
```

##### Zero-Width Bit Fields (Alignment)

Force alignment to the next storage unit boundary:
```c
struct aligned {
    unsigned int a : 3;
    unsigned int   : 0;   // Force next field to new boundary
    unsigned int b : 5;
};
```

##### Mixed Structure Members

Combine normal members with bit fields:
```c
struct mixed_struct {
    int count;              // Regular int (32 bits)
    char c;                 // Regular char (8 bits)
    unsigned int f1 : 1;    // Bit field (1 bit)
    unsigned int f2 : 1;    // Bit field (1 bit)
    unsigned int type : 8;  // Bit field (8 bits)
};
```

#### Type Support

**Standard Types** (all C versions):
```c
signed int field : 5;       // Signed bit field
unsigned int field : 5;     // Unsigned bit field (recommended)
```

**C99/C11 Addition**:
```c
_Bool flag : 1;             // Boolean bit field (C99+)
bool flag : 1;              // Boolean bit field (C99+ with stdbool.h)
```

**Recommendation**: Always use explicit `signed int` or `unsigned int` to avoid hardware-dependent behavior.

#### Restrictions and Limitations

1. **No Arrays**: Cannot create arrays of bit fields
   ```c
   unsigned int flags[3] : 1;  // ❌ NOT ALLOWED
   ```

2. **No Address-Of Operator**: Cannot take the address of a bit field
   ```c
   unsigned int *ptr = &packed_data.f1;  // ❌ NOT ALLOWED
   ```

3. **No Pointers to Bit Fields**: No such type exists
   ```c
   // No syntax for "pointer to bit field"
   ```

4. **Width Limits**: Bit width must be 0 to the size of int on your platform (usually 32)

#### Best Practices

1. **Always Specify Signedness**:
   ```c
   unsigned int field : 5;  // ✅ Good - explicit
   int field : 5;           // ⚠️ Avoid - implementation-dependent
   ```

2. **Document Bit Layouts**:
   ```c
   struct control_register {
       unsigned int enable    : 1;   // Bit 0: Enable flag
       unsigned int mode      : 2;   // Bits 1-2: Operating mode
       unsigned int           : 5;   // Bits 3-7: Reserved
       unsigned int interrupt : 1;   // Bit 8: Interrupt enable
   };
   ```

3. **Use Meaningful Names**:
   ```c
   unsigned int is_valid : 1;       // ✅ Clear intent
   unsigned int b : 1;              // ❌ Unclear
   ```

4. **Group Related Fields**:
   ```c
   struct status {
       // Flags group
       unsigned int error   : 1;
       unsigned int warning : 1;
       unsigned int ready   : 1;
       
       // Data group
       unsigned int value   : 8;
       unsigned int index   : 16;
   };
   ```

#### Real-World Applications

##### Hardware Register Mapping

Perfect for mapping hardware control/status registers:
```c
struct uart_control_reg {
    unsigned int tx_enable  : 1;  // Transmit enable
    unsigned int rx_enable  : 1;  // Receive enable
    unsigned int parity     : 2;  // Parity mode (00=none, 01=odd, 10=even)
    unsigned int stop_bits  : 1;  // Stop bits (0=1bit, 1=2bits)
    unsigned int data_bits  : 2;  // Data bits (00=5, 01=6, 10=7, 11=8)
    unsigned int            : 1;  // Reserved
};
```

##### Protocol Headers

Efficient for network protocol or communication headers:
```c
struct packet_header {
    unsigned int version    : 4;   // Protocol version
    unsigned int type       : 4;   // Packet type
    unsigned int priority   : 2;   // Priority level
    unsigned int ack        : 1;   // Acknowledgment flag
    unsigned int more       : 1;   // More packets follow
    unsigned int seq_num    : 12;  // Sequence number
    unsigned int length     : 8;   // Payload length
};
```

##### Configuration Flags

Compact storage for system configuration:
```c
struct system_config {
    unsigned int debug_mode     : 1;
    unsigned int verbose        : 1;
    unsigned int auto_save      : 1;
    unsigned int log_level      : 3;  // 0-7 levels
    unsigned int timeout        : 10; // Timeout in seconds (0-1023)
};
```

#### Memory Efficiency Example

**Traditional Approach**:
```c
// 5 separate integers = 20 bytes
int f1, f2, f3;        // 12 bytes
int type;              // 4 bytes
int index;             // 4 bytes
// Total: 20 bytes
```

**Bit Field Approach**:
```c
struct packed_struct {
    unsigned int       : 3;
    unsigned int f1    : 1;
    unsigned int f2    : 1;
    unsigned int f3    : 1;
    unsigned int type  : 8;
    unsigned int index : 18;
};
// Total: 4 bytes (29 bits used, 3 unused)
// Memory savings: 80%
```

#### Portability Considerations

1. **Bit Field Order**: The order bits are packed is implementation-defined (some systems pack left-to-right, others right-to-left)

2. **Alignment**: Padding may be added between bit fields depending on the compiler

3. **Size**: The size of the underlying storage unit may vary

4. **For Maximum Portability**: Document assumptions and test on target platforms, especially for hardware register access in embedded systems

#### When to Use Bit Fields

**Use Bit Fields When**:
- Code readability is important
- Frequently accessing/modifying packed data
- Mapping hardware registers
- Protocol implementation
- Not concerned about bit-level portability

**Use Bitwise Operators When**:
- Need precise control over bit layout
- Working with binary file formats
- Performance-critical bit manipulation
- Interfacing with strictly defined hardware specifications
- Cross-platform bit-level compatibility required

#### Summary

Bit fields provide a convenient, readable way to pack data in C structures. They're ideal for embedded systems where memory efficiency matters and hardware register access is common. The syntax is straightforward, and the compiler handles all the complex bit manipulation automatically, making code much easier to write and maintain than manual bitwise operations.
