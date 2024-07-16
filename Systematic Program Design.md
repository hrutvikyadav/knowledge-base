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


---

[^non-distinct]: Non-distinct-values-are-the-ones-which-represent-the*same(i.e.-non-distinct)-information*.-Ex->-CityName-can-be-*different-values*-like-"Pune",-"Mumbai"-but-they-*represent-the-same-underlying-information*---CityName-in-this-case
[^distinct]: Distinct-values-are-the-ones-which-represent-unique-information-in-the-problem-domain-on-their-own
[^non-distinct-vs-distinct]: Main-difference-is-observed-in-the-minimum-number-of-examples-needed(Examples-are-redundant-in-case-of-distinct-values)
