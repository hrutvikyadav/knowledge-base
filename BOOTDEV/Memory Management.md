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
### Struct definition
A struct is a user-defined data type that groups related data together. It's like a class in object-oriented programming languages.
```c
struct Person {
    char name[50];
    int age;
    float height;
};
```

### Struct initialization
```c
struct Person person = {"John", 30, 5.8};
```
or
```c
struct Person person = {.name = "John", .age = 30, .height = 5.8};
```
or we can also do zero initialization
```c
struct Person person = {0};
```

### Struct field access
If we have a struct variable identifier `person`, we can access its fields like this with the `.` operator:
```c
printf("Name: %s\n", person.name);
printf("Age: %d\n", person.age);
printf("Height: %f\n", person.height);
```
For a struct pointer, we use the `->` operator:
```c
struct Person *person_ptr = &person;
printf("Name: %s\n", person_ptr->name);
// update age
person_ptr->age = 31;

// or we can also use the dereference operator
(*person_ptr).age = 31;
```

### Struct typedef
Instead of writing `struct Person` every time we want to declare a variable of type `Person`, we can use a `typedef`:
```diff
+ typedef struct Person {
     char name[50];
     int age;
     float height;
+ } person_t;
```
and then declare a variable like this:
```c
person_t person = {"John", 30, 5.8};
// i.e. use person_t instead of struct Person every time
```
> [!note] the _t suffix is a common convention for naming typedefs or types in C.
> [!tip] We can optionally avoid naming the struct in the typedef, and write `typedef struct { ... } person_t;` instead.
> [!tip] We can also use `sizeof` to get the size of a struct.

### Struct Memory Layout
The memory layout of a struct is determined by the size of its fields and the alignment requirements of the platform.
The size of a struct is the sum of the sizes of its fields, but the actual size may be larger due to padding.
The padding is added to ensure that the fields are aligned correctly in memory and it varies based on the platform.
[data-structure-alignment](https://en.wikipedia.org/wiki/Data_structure_alignment)
Struct field ordering actually matters because of padding. Fields are laid out in memory in the order they are declared.
Sometimes you can reorder fields to reduce padding and make the struct smaller.
> [!tip] As a rule of thumb, ordering your fields from largest to smallest will help the compiler minimize padding

### Struct Forward Declaration
To declare a struct that references itself (a recursive struct), you can use a forward declaration:
```c
typedef struct Node node_t; // note the same name as the struct below
typedef struct Node {
    int data;
    node_t *next;
} node_t;
```

## Pointers
Pointers are variables that store the memory address of another variable. Thus, they "point" to the location of a value.
### Pointer declaration
```c
int *p;
```
Here, `p` is a pointer to an integer. The `*` operator is used to declare a pointer.

### Pointer initialization
```c
int x = 10;
int *p = &x;
```
Here, `p` is initialized with the address of `x`.

### Pointer dereferencing
To access the value at the memory address pointed to by a pointer, we use the `*` operator:
```c
printf("%d\n", *p);
```

### Pointer arithmetic
When you add an *integer* to a **pointer**, the **pointer** moves by that many bytes. i.e. the ==*integer* times the *sizeof the data type*== gets added to the **pointer**. This is useful for iterating over arrays.
```c
int arr[5] = {1, 2, 3, 4, 5};
int *p = arr;
printf("%d\n", *p); // prints 1 i.e. the first element of the array
p++;
printf("%d\n", *p); // prints 2 i.e. the second element of the array

// now that the pointer points to the second element, we can also access the elements using array notation
printf("%d\n", p[2]); // prints 4 i.e. the fourth element of the array
// this is the same as *(p + 2)
printf("%d\n", *(p + 2)); // prints 4
```

### Arrays and Pointers
An array is a pointer, but it is not just any pointer, it is a pointer to the whole block of memory allocated to the array.
However most of the times, arrays decay into a pointer to the first element of the array.

#### Arrays decay when
1. Passed to a function - That's why you can't pass an array to a function by value like you do with a struct; instead, the array name decays to a pointer.
2. Used in an expression involving pointers (pointer declaration, pointer arithmetic, indexing etc.)

#### Arrays dont decay when
1. Used with the `sizeof` operator - `sizeof(arr)` will give you the size of the whole array, not the size of a pointer.
2. Used with the `&` operator - `&arr` will give you the address of the whole array, not the address of the first element.
3. Array initialization and declaration - `int arr[5] = {1, 2, 3, 4, 5};` is an array, not a pointer.

## Enums

## Unions

## Stack and Heap

## Advanced Pointers

## Stack Data Structure

## Objects

## Refcounting GC

## Mark and Sweep GC

