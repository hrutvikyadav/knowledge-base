---
id: HTDC
aliases:
  - HTDC
tags:
  - how-to-design-classes
  - class-based-design
---

# HTDC

## PREFACE
Conventional text books at this level present object-oriented programming as an extension of imperative programming. The reasoning is that computations interact with the real world where physical objects change their state all the time. For example, people get sick and become patients; balls drop from a hand and change location; and so on.
Therefore, the reasoning goes, computational objects, which represent physical objects, encapsulate and hide the state of the world for which they are responsible and, on method calls, they change state.
Naturally, people begin to *express computations as sequences of assignment statements* that change the value of variables and fields (instance variables).
We ==disagree== with this perspective and put classes and the design of classes into the center of our approach.
> [!tip]
> Instead of using assignment opereators all over the code to update state, **we send a message to the object; i.e. call a method on the object; to inform it of a state change which needs to be processed.** The caller tells the what, the developer has already defined the how in the methods.

In “How to Design Programs” we defined classes of data. As we developed larger and larger programs, it became clear that the design of a program requires the introduction of many classes of data and the development of several functions for each class. The rest is figuring out how the classes of data and their functions related to each other.
> Here we show students how object-oriented programming languages such as C# and Java support this effort with syntactic constructs. We also refine the program design discipline.

### Purpose and Background
The goal of this chapter is to develop data modeling skills.
We assume that students have understood data definitions in Parts I, II, and III of How to Design Programs. That is, they should understand what it means to represent atomic information (numbers, strings), compounds of information (structures), unions of information (unions), and information of arbitrary size (lists, trees); ideally students should also understand that a program may rely on an entire system of interconnected data definitions (family trees, files and folders).

In the end, students should be able to design a system of classes to represent information. In particular, when given a system of classes and a piece of information, they should be able to create objects and represent this information with data; **conversely**, given an instance of a class in the system, they should be able to interpret this object as information in the “real” world [^exercise] .

## Varieties of data
In [[Systematic Program Design]] we learned that identifying information from a problem domain is key to solving any problem.
First we determine the input, the output data, then we decide how to represent this domain information in our program.
Only then we start designing functions.

Therefore in any new language we first need to know how to respresent information for our program.
- In the previous course we used a combination of English comments and the scheme constructors to create data definitions.
- In this course we will use a programming language that has syntax to describe classes of data in a program, Where *classes* basically means *collection* and *data* is often refered to as **object**. Which is why these languages are called object-oriented language.

For most problems, a single class cannot represent all information in problem domain, instead we will end up *defining a many classes,* **and** the *relationship between them*. Along with this, if we encounter a bunch of related classes in a program, we should be able to interpret them as information in problem domain.

### Primitive Forms of Data
#atomic
Similar to BSL, Java supports primitive forms of data with some differences. 

### Classes
#non-atomic

- class definition
- fields, default values i.e. const, shared attributes
- constructors

> [!note]
> A note on examples in oop languages -
> It is good practice to create a separate class for examples that go together - makes easier to test later(similar to check-expects in BSL)
> ex. Coffee with CoffeeExamples

#### Exercises on plain Classes
1. GPS location class
2. ...
3. ...

#### Designing Classes

### Class references, Object containment
#### Exercises on object containment
#### Designing Classes that refer to Classes

### Unions of Classes
#### Types v/s Classes
#### Exercises on Unions
#### Designing Unions of Classes

### Unions, Self references and Mutual references
#### Containment in Unions - 1
#### Containment in Unions - 2
#### Exercises on Containment in Unions

### Designing Class Hierarchies
#### Exercises
#### Case Study - Fighting UFO's

### Intermezzo 1
#### Vocabulary and Grammar
#### Meaning
#### Syntax errors, type errors and runtime errors

## Functional Methods

[^exercise]: Go through MITS11 and Firmware project to identify related classes, what is the relationship and come up with UML diagrams that document your findings
