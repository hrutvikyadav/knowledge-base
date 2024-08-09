---
id: Aquaintance
aliases:
  - Aquaintance
tags: []
---

# Aquaintance

> [!warning] Buckle up
> C is a language that allows programmers to shoot themselves in the foot as does not stop you from doing anything as it assumes you know what you are doing.
> For the moment, we impose some restrictions so that we avoid these footgun situations. Making it clear how the restrictions avoid these situations along the way.
>> [!note] The most dangerous construct in C are `casts`. These are skipped in this level. Other pitfalls are less easy to avoid and are explained in this level.
>> [!info] Some style considerations to note:
>>> [!tip] These are explained later; for now just follow them.
>>> - We bind type modifiers and qualifiers to the left: We want to separate **identifiers** visually from their **type**
>>>   ```c
>>>   char* name;
>>>   // here the type is `char*` and the identifier is `name`
>>>   ```
>>>   We also apply this left binding rule to **qualifiers** like `const` and `volatile`
>>>   ```c
>>>   char const* const path_name ;
>>>   // here the type is `char const* const` and the identifier is `path_name`
>>>   // the first const qualifies the char to its left, the * makes it to a pointer, and the second const again qualifies what is to its left
>>>   // this means the pointer is constant and the data it points to is constant i.e. neither the pointer nor the data can be modified
>>>   ```
>>> - We do not use continued declarations: We want to avoid confusion about the type of the identifier
>>>   ```c
>>>   unsigned const * const a, b;
>>>   // Here, b has type unsigned const: that is, the first const goes to the type, and the second const only goes to the declaration of a
>>>   ```
>>> - We use array notation for pointer parameters: This is done wherever the pointer is assumed *not to be `NULL`*
>>>   ```c
>>>   /* These emphasize that the arguments cannot be null . */
>>>   size_t strlen ( char const string[static 1]) ;
>>>   int main (int argc, char* argv[argc +1]) ;
>>>   /* Compatible declarations for the same functions . */
>>>   size_t strlen ( const char *string ) ;
>>>   int main (int argc, char **argv ) ;
>>>   // The first stresses the fact that strlen must receive a valid (non-null) pointer and will access at least one element of string. 
>>>   // The second summarizes the fact that main receives an array of pointers to char: the program name, argc-1 program arguments, and one null pointer that terminates the array
>>>   ```
>>> - We use function notation for function pointer parameters: This is done wherever the function pointer is assumed *not to be `NULL`*
>>>   ```c
>>>   /* These emphasize that the handler argument cannot be null . */
>>>   int atexit(void handler(void));
>>>   /* Compatible declaration for the same function . */
>>>   int atexit(void (*handler)(void));
>>>   // Here, the first declaration of atexit emphasizes that, semantically, it receives a function named handler as an argument and that a null function pointer is not allowed. Technically, the function parameter handler is “rewritten” to a function pointer much as array parameters are rewritten to object pointers, but this is of minor interest for a description of the functionality
>>>   ```
>>> - We define variables as close to their first use as possible: Lack of variable initialization, especially for pointers, is one of the major pitfalls for novice C programmers. This is why we should, whenever possible, combine the declaration of a variable with the first assignment to it: the tool that C gives us for this purpose is the definition: a declaration together with an initialization
>>> - We use prefix notation for code blocks: This is done to make it easier to see where a block starts and ends

1. Everything is about control 
   - Conditional execution 
   - Iterations 
   - Multiple selection 
   - Summary 
     > - Numerical values can be directly used as conditions for if statements; 0 represents “false,” and all other values are “true.”
     > - There are three different iteration statements: for, do, and while. for is the preferred tool for domain iterations.
     > - A switch statement performs multiple selection. One case runs into the next, if it is not terminated by a break.
   - Exercises
     - [ ] Add the if (i) condition to the program, and compare the output to the previous.
     - [ ] Try to imagine what happens when i has value 0 and is decremented by means of the operator --.
     - [ ] Analyze listing 3.1 by adding printf calls for intermediate values of x.
     - [ ] [^Exs-5] Describe the use of the parameters argc and argv in listing 3.1.
     - [ ] [^Exs-6] Print out the values of eps1m01, and observe the output when you change them slightly.
     - [ ] [^Exs-7] Test the example switch statement in a program. See what happens if you leave out some of the break statements.
## Expressing computations 
> [!info] Expressions are code snippets that evaluate to a value. In this section *values* and *objects* involved in the computation are mostly of type `size_t`.
> Such values correspond to sizes so they can't be negaive. This can be useful to represent natural numbers. However, computers are finite and can't represent infinite values. So, the largest value that can be represented is `SIZE_MAX`.
>> [!note] The type size_t represents values in the range `[0, SIZE_MAX]`.
>> Depending on platform, `SIZE_MAX` can be quite large:
>> - 2^16 - 1 = **65535** on `16-bit` systems; only found on embedded systems these days
>> - 2^32 - 1 = **4294967295** on `32-bit` systems; found on some older systems
>> - 2^64 - 1 = **18446744073709551615** on `64-bit` systems; found on most modern systems
>
> The standard header stdint.h provides SIZE_MAX such that you don’t have <stdint.h> to figure out that value yourself.
> *Non-negative* numbers are represented in C as the **`unsigned integer` types**.
> *Operators* like `+`, `-`, `*`, `/`, `%`, `&`, `|`, `^`, `<<`, `>>`, `~`, `!`, `&&`, `||`, `?:`, `==`, `!=`, `<`, `>`, `<=`, `>=` are used to express computations.
> They can be used to operate on *values* and *objects*.
### Arithmetic
[[Arithmetic]]
### Operators that modify objects 
### Boolean context 
### The ternary or conditional operator 
### Evaluation order 
### Summary 
## Basic values and data 
### The abstract state machine 
### Basic types 
### Specifying values 
### Implicit conversions 
### Initializers 
### Named constants 
### Binary representions 
### Summary
## Derived data types 
### Arrays 
### Pointers as opaque types 
### Structures 
### New names for types: type aliases 
### Summary 
## Functions 
### Simple functions 
### main is special 
### Recursion 
### Summary 
## C library functions 
### General properties of the C library and its functions 
### Mathematics 
### Input, output, and file manipulation 
### String processing and conversion 
### Time 
### Runtime environment settings 
### Program termination and assertions 
### Summary