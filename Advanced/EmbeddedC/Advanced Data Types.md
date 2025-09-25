---
id: Advanced Data Types
aliases:
  - Advanced Data Types
tags: []
---

# Advanced Data Types

## `#define`
The define preprocessor directive is used to define constants in a program
The statement continues till the next newline character.
So the extent of a define statement is a single line but occurences of backslash + newline are deleted at the time of preprocessing. Which means we can spread the directive over several lines for readability with the use of `\\n`.
> [!note]
> There is no `=` because this is not a variable assignment. Also there is no semicolon at the end.
By convention only capital letters and underscores are used for naming.
These statements are usually grouped at the top of source files or inside header files for use in multiple source files.

`const` v/s `#define`:
Using const is sometimes more preffered because it does the type checking.
Whereas define only replaces all occurences of that text with a sequence of characters.

## Typedef
Used to define new type aliases for an existing primitive datatype.
Why?
- It makes the intent of code clearer. Code is more descriptive.
- Affects maintainability. If something changes only one line needs to be changed.

> Typedef can also be useful for complicated type casting.
<!-- TODO: add more info -->

## Varible length arrays
## Flexible array numbers
## Complex number types
