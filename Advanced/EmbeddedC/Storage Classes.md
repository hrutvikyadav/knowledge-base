---
id: Storage Classes
aliases:
  - Storage Classes
tags:
  - has_resources
---

# Storage Classes

Storage classes are used to describe properties of a variable/function.
These properties include things like scope, visibility, life-time and so on.
1. Scope - defines the block scope in which the variable can be accessed in a file.
2. Life-time - defines the duration for which the variable exists in computer memory.
3. Visibility - defines whether the variable is visible only to a single src file or also to other src files with the right declarations (see [[Working with larger programs#Communication between files|extern]])

The 4 Storage classes can be split into 2 storage durations
1. **Automatic storage duration**
2. **Static storage duration**

## Syntax
- Storage class specifiers precede the type specifier in variable declarations i.e. `<str_class> <data_type> <identifier>`
- The same rule applies for all 4 classes below.

## Auto 
a local variable is an auto variable; the term is just not explicitly used.

The keyword `auto` is used to declare variables of **Automatic storage duration**.
Variables that have the `auto` specifier ->
- are created when the block that defines them is entered.
- exist while the block is active.
- are destroyed when the block is exited.

This is really the concept of local variables
> - Local variables are always block scoped (defined in a block)
> - Local variables by default have auto storage duration (the keyword `auto` is rarely used)
>   - But that does not mean it is completely useless!
>   - You can explicitly specify the `auto` keyword to indicate that you are overriding some external variable with the same name.
>   - You can also specify the keyword to express the intent that this variable should not be assigned a Storage class other than **auto**.
> - If an initial value is given to these variables, the variable is assigned that value on each function call.
> i.e. these variables are known as automatic local variables

## Extern
Usage with ->
1. Variables (Arrays are special case as in for 1d array you don't need to specify the size; and for 2d arrays you need to specify the size of 2nd dimension while leaving out the size for the 1st dimension)
2. Functions (we can use extern on functions in order to bypass having to include a header file with the prototype declaration)

## Static
Can be used with variables and functions
1. Variables
   - Local
     When used with local variables, it tells compiler to keep the variable around (with its initial value) for the entire life-time of the program.
     > Remember that local variables are created/destroyed when the block scope is entered/exited; adding static keyword to such a variable will stop this creation/destruction everytime the block executes, instead it will be initialized once at the start of the program and will be deinitialized once at the end of the program.
     > In other words, static local variables will preserve their values even after going out of scope.
     > static local variables have a default initial value of zero unlike auto local variables which don't have a default value.
     > static local variables are *allocated on the heap not on the stack*
     > If specified, the value of a static local at initialization must be a constant expression.
   - Global
     When used with global variables, it acts as opposite of `extern` and makes it so that the variable scope/visibility is limited to a single file
2. Functions
   When used with functions, it limits the scope/visibility of that function to a single file

## Register
We can add the `register` specifier to a variable to indicate to the compiler that we want to store this variable in a CPU register.
> [!warn]
> Even though such a specifier is given, the compiler can take the decision to store this variable in a register or not, depending upon certain factors.
> In some cases, the compiler will even apply optimizations to store some variables in registers even when the `register` specifier is not given.

# Challenges
<!-- TODO: fill up -->
1.
2.
3.
