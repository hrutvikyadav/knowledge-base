---
id: Arithmetic
aliases:
  - Arithmetic
tags: []
---

# Arithmetic

> [!tip] Value operators: The Form column gives the syntactic form of the operation, where @ represents the operator and a and possibly b denote values that serve as operands.
> For arithmetic and bit operations, the type of the result is a type that reconciles the types of a and b.
> For some of the operators, the Nick column gives an alternative form of the operator, or lists a combination of operators that has special meaning.
> Most of the operators and terms will be discussed later

Operator | Nick | Form | Type restriction a |Type restriction b | Type restriction Result | x
--- | --- | --- | --- | --- | --- | ---
 |  |  |  |  |  | 

> [!tip] Object operators: the Form column gives the syntactic form of the operation, where @ represents the operator, o denotes an object, and a denotes a suitable additional value (if any) that serves as an operand.
> An additional * in the Type column requires that the object o be addressable.

Operator | Nick | Form | Type | Result | x
--- | --- | --- | --- | --- | ---
 |  |  |  |  |

> [!tip]  Type operators: these operators return an integer constant (ICE) of type `size_t`. They have function-like syntax with the operands in parentheses

Operator | Nick | Form | Type of T |
--- | --- | --- | --- |
 |  |  |  |

---

The +, -, * operators work as expected and braces can be used to group expressions.
```c
size_t a = 45;
size_t b = 7;
size_t c = (a - b) *2;
size_t d = a - b*2;
```

The +, - operators have unary variants.
```c
size_t c = (+a + -b) *2; // this gives the same result as (a - b) * 2 i.e. 76
```

> Even for `unsigned` types, *negation* and *difference* are well defined. Regardless of the values, the computation will have valid result i.e. between `0` and `SIZE_MAX`.
> One of the properties of `size_t` is that +-* arithmetic always works where it can

> [!note]
> - Unsigned arithmetic is always well defined
> - The operations +, -, and * on size_t provide the mathematically correct result if it is representable as a size_t.

When result is not in range, and therefore not representable as `size_t`, we consider **overflow**.

