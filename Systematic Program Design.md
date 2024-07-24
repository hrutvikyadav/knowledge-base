---
id: Systematic Program Design
aliases:
  - Systematic Program Design
tags:
  - systematic-program-design
  - coursera
  - how to code simple data
  - how to code complex data
---

# Systematic Program Design

## Intro

- Difficulties in programming:
  1. How to go from a poorly formed problem to a well structured solution.
  2. How to break down complex problems into smaller, manageable parts.(That are well tested and easy to understand)

Systematic Program Design is a method to solve these problems.

## How to design functions

### Primitive Data

#### Recipe

1. Signature
2. Purpose
3. Stub
4. Examples i.e. tests
5. Template
6. Code

### Non Primitive Data

Recipe is similar to the [[Systematic Program Design#recipe|recipe]] for primitive data but with some additional steps.(Template and Inventory)
- Refer ==guidance on examples and check-expects== for the identified form of information.

> [!info] Non primitive data is data defined by the programmer.

#### Examples
[[Atomic Non Distinct#BestCity]]
[[Interval#AisleSeat]]
[[Enumeration#BumpUp]]
[[Itemization#CountdownToNewYear]]

### Compound Data

Recipe is similar to the [[Systematic Program Design#recipe|recipe]] for primitive data but with some additional steps.(Template and Inventory)
Compound data is data that is composed of different parts that naturally belong together.
Unlike atomic data, compound data is not a single value but a collection of values which are meaningful together.

#### Examples
[[Compound Data#CowABunga]]
[[Self Referential Data#Favorite IPL Team]]
[[Referential Data#SchoolTutionGraph]]
[[Binary Search Trees#AccountLookupInBST]]
> [!tip] Searching in a Binary Search Tree leads to *tree traversal*(`In Order`, `Pre Order` or `Post Order` traversal).
[[Mutually Referential Data#SumOfFiles]]
[[Mutually Referential Data#SearchForFile]]
> [!tip] Searching in a Arbitrary Arity Tree leads to *backtracking tree traversal*.

## How to design data

This deals with the problem of how to ==represent== *information in problem domain* as *data in program*.
> And conversely how to ==interpret== data in program as information in problem domain.

### HTDD recipe
First identify the form of information in the problem domain to decide what kind of data definition to use. Then follow the recipe.
> [!tip] The form of information in the problem domain is the key to deciding what kind of data definition to use.
> | When form of information to be represented -> | Use data definition for👇 | 
> | ------------- | -------------- | -------------- |
> | is Atomic | Simple Atomic Data | 
> | is numbers within a range | Interval | 
> | consists fixed number of distinct items | Enumeration | 
> | consists of 2 or more subclasses; at least one of them is non distinct | Itemization | 
> | consists of 2 or more items that naturally belong together | Compound data | 
> | naturally composed of different parts(types?) | References already defined type/s | 
> | is of arbitary size(unknown at compile time) | Self referencial or Mutually referencial | 

1. Struct definition where needed
2. Type comment
   > [!tip] Usually atomic data can be represented using primitive data types.
3. Interpretation
4. Examples
5. Template
   > [!tip] Use Data Driven Templates for guidance. Search for the type comment.

   > [!info] For templating ->
   > - Reference rule causes a Natural Helper Function to be added to the template.
   > - Self Reference rule causes a Natural Recursion call to be added to the template.
   > - Mutual Reference rule causes a Natural Mutual Recursion call to be added to the templates of the types involved.

#### Examples
1. [[Atomic Non Distinct]]
2. [[Interval]]
3. [[Enumeration]]
4. [[Itemization]]
5. [[Compound Data]]
6. [[Self Referential Data]]
7. [[Referential Data]]
8. [[Binary Search Trees]]
9. [[Mutually Referential Data]]
10. [[Two One-Ofs]]

## How to design world programs
- [ ] TODO
1. Domain analysis
   From the problem domain, identify *constant information*, *changing information*(state), `api`'s to be used from your choice of *UI framework*.
2. Data definition
3. Function design

### Examples
[[Compound Data#CowABunga]]

## Helper Functions
Used for the following reasons:
1. at *references to other non-primitive* data definitions (*natural helper* in the template)
2. to form a `function composition`
3. to handle a *knowledge domain shift*
4. to ==operate on arbitrary sized data **all at once**==

## Refactoring
Techniques to improve structure of code without changing its behavior.

### Locally scoped definitions

- used to encapsulate definitions to avoid name conflicts i.e. #encapsulation
  > Example [[Mutually Referential Data#OptimizedSearchForFile|OptimizedSearchForFile]]
- used to avoid recomputation of values i.e. #performance-optimization
  > Example [[Mutually Referential Data#OptimizedSearchForFile|OptimizedSearchForFile]]
- prevents conflicting variable names i.e. #variable-shadowing

### Abstraction
It is a technique to reduce redundancy in code by creating a new function that captures the commonality between two or more functions and abstracts away the details 
> [!info] For abstract functions, we need to follow the recipe backwards, as we arrive at the function first and then write the signature.(In modern languages, the `lsp` will show the signature upon hovering over the function name)

Goals of the module:
- Be able to identify 2 or more functions that are candidates for abstraction.
- Be able to design an abstract function starting with 2 or more highly repetitive functions (or expressions).
- Be able to design an abstract fold function from a template.
- Be able to write signatures for abstract functions.
- Be able to write signatures that use type parameters.
- Be able to identify a function which would benefit from using a built-in abstract function
- Be able to use built-in abstract functions

#### How to abstract
1. Identify the common/repeated code in the functions.
2. Identify the parts that vary between the functions.
3. Create a new function that captures the commonality and takes the varying parts as arguments.
  > [!tip] The varying part could be a function, in that case the new function is a #higher-order-function. This can be implemented in a language that supports #first-class-functions.

  > [!info] Some abstract functions are built-in functions in the language.

  > [!tip] It is common for the function passed into a higher-order function to be a lambda function that is bound to some variable in the enclosing scope. i.e. #closure

  > [!info] Abstract functions can be produced directly from templates. This can be wonderfully useful, especially for types involving [[Mutually Referential Data]] *(here, 2 or more closures come into play depending upon the number of types involved)*.
  > Example [[Abstraction#Fold Function]]

## Generative Recursion

Different kind of recursion in which the *data passed to the recursive call is ==generated==*, rather than being a part of the data passed to the current call
> Instead of passing something like (rest lox) to the recursive call, the value is somehow generated from the current value.

## Search

Expands on generative recursion

---

[^non-distinct]: Non-distinct-values-are-the-ones-which-represent-the*same(i.e.-non-distinct)-information*.-Ex->-CityName-can-be-*different-values*-like-"Pune",-"Mumbai"-but-they-*represent-the-same-underlying-information*---CityName-in-this-case
[^distinct]: Distinct-values-are-the-ones-which-represent-unique-information-in-the-problem-domain-on-their-own
[^non-distinct-vs-distinct]: Main-difference-is-observed-in-the-minimum-number-of-examples-needed(Examples-are-redundant-in-case-of-distinct-values)
