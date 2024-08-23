---
id: Arrays
aliases:
  - Arrays
tags: []
---

# Arrays

Allow us to group objects of the same type into an encapsulating object
- arrays look like pointers in many contexts, and pointers can refer to array objects

Based on C's assumptions about the abstract state machine, arrays can be described as follows:

> [!hint] Arrays are not pointers.
> They are objects that can be manipulated using pointer arithmetic.
> The name of an array is a constant pointer to the first element of the array. The array object itself is a contiguous block of memory that can be accessed using pointer arithmetic.
>> [!tip] Arrays and pointers are closely related however it is important to discuss arrays without talking about pointers in order to gain better understanding of C.

## Array declaration

```c
double a[10]; // declares an array of 10 doubles
signed b[N]; // declares an array of 10 signed integers
```

Visualization of the memory layout of an array of 10 integers:
[[Drawing 2024-08-23 14.57.55.excalidraw]]

- The type that composes the array can also be an array type this is called a multi-dimensional array:
```c
int c[3][4]; // declares a 3x4 array of integers
int ( d[3] )[4]; // same as above because [] binds to the left

// generally, the above declarations represent an indentifier which points to M objects of N type
int e[M][N]; // declares an array of M arrays of N integers
```
[[MultiD arrays 2024-08-23 15.33.25.excalidraw]]

## Array initialization

```c
int a[3] = {1, 2, 3}; // initializes the array with 1, 2, 3

// using designated initializers
int b[3] = { [0] = 1, [2] = 3 }; // initializes the array with 1, 0, 3
```

## Array operations

Arrays are really just objects of a different type than we have seen so far
> [!tip] An array in a condition evaluates to true
> This is because of how array decay works in C
>> [!important] We cannot evaluate arrays(to values) like other objects in C

> [!tip] There are array objects but no array values
> Thus arrays cannot be used as operands for operators; there is **no arithmetic on an array object**
>> [!tip] Arrays cannot be compared.
>>> [!tip] Arrays cannot be assigned to.

> [!hint] There are 4 operators left(from table of all operators) that can be used with arrays:
> - `[]` - we have seen it above
> - `&` - introduced later 
> - `sizeof` - introduced later 
> - *array decay operation* - introduced later 

## Array length 

Based on length there are two types of arrays:
- fixed length arrays 
    Exisitng in C since the beginning. This feature is similar to arrays in most other languages.
- variable length arrays
    Introduced in C99, this feature is unique to C. It allows the length of an array to be *determined at runtime*. **This has some restrictions on usage**

> [!tip] VLAs cannot have initializers
> As initialization is static and VLAs are dynamic

> [!tip] VLAs cannot be declared outside function scope

Lets see which arrays are FLAs and thus dont have the above restrictions:
> [!tip] Length of an FLA is determined by an #ICE (integer constant expression) or an *initializer list*
> Any integer type can be used in an ICE
>> [!important] Array length specification must be strictly positive
>
> When there is no length specified, the length is determined by the number of elements in the initializer list if any

```c
double E[] = { [3] = 42.0 , [2] = 37.0 , };
double F[] = { 22.0 , 17.0 , 1 , 0.5 , };
// such an initializer structure can always be determined at compile time
```
![[Pasted image 20240823163804.png]]

> [!note] All other array declarations are VLAs

> [!tip] An array with a length that is not an integer constant expression is an VLA

> [!tip] Length of an array can be computed with `sizeof` operator. This operator can be used to determine size of any object in C, so we can use it to determine the size of an array object by dividing array size by the size of an element of the array.
```c
size_t len = ( sizeof a ) / ( sizeof a[0] );
```
> [!important] Note also that the sizeof operator comes in two different syntactical forms. If applied to an object, as it is here, it does not need parentheses, but they would be needed if we applied it to a type

## Array as parameters

A special case is when arrays are passed as parameters to functions. Such params may have empty [].
An initializer is not possible for such parameters, as the length is determined by the caller.

> [!tip] **Innermost dimensions** of an array param to a function is *lost*

> [!tip] Don't use `sizeof` to determine the length of an array passed as a parameter

Since arrays dont evaluate to values, array params cannot be passed by value. They are always passed by reference.
> [!tip] Array parameters behave ==as if== passed by reference

