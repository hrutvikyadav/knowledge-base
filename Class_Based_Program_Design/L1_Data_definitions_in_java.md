---
id: L1_Data_definitions_in_java
aliases:
  - L1_Data_definitions_in_java
tags: []
---

# L1_Data_definitions_in_java

Design
- simple classes
- classes that refer to objects of other class(es)

## Types of data
Information v/s Data
Information is everywhere in the real world.
Data is inside the program, which is supposed to represent the information in the real world.

> When looking at either we should be able to interpret what it means in the other i.e.
> - Look at the program and interpret it as information in the domain.
> - Look at the information and interpret it as data in the program.

This is why the first step in designing a program is to identify appropriate way to represent the information.

> [!tip]
> In `racket`, we use comments to denote the type of a variable and its description.
> In `java` however the type is the part of the syntax to define the variable, and we can *optionally* still write a comment to add a description.

## Classes in Java

In racket we learned that if information to be represented consists of several related parts, we use a [[Compound Data|struct]] to represent the information.
Similarly in Java we use a *class* instead to represent related information

Before actually writing the Java code to represent any information, we will be drawing **class diagrams** in the UML style.
This is meant to give overview of the many classes when code gets complex and visualize the relationships between classes.

Example 
```
+----------------+
|  Book          |
+----------------+
| String name    |
| Author author  |----
| Number price   |   |
+----------------+   |
                     V
             +----------------+
             |  Author        |
             +----------------+
             | String name    |
             | Number id      |
             +----------------+
```

```java
class Book {
    String name;
    Author author;
    int price;

    // NOTE: a difference from racket is that along with the struct definition we got the contructors
    // and the accessors i.e. getters and setters for free. Here we need to write those ourselves.
    Book (String name, Author author, int price) {
        this.name = name;
        this.author = author;
        this.price = price;
    }
}

class Author {
    String name;
    int id;

    Author (String name, int id) {
        this.name = name;
        this.id = id;
    }
}
```

## How to create objects

> [!warn]
> Unlike racket, we cannot just create variables anywhere in the global scope.
> Every object created needs to be inside of a class.
> Plus it is good practice to create a separate `Examples<XYZ>` class to hold examples of the `<XYZ>` class.

> We won't instantiate the Examples classes, so we can leave an empty constructor.

```java
class ExamplesBooks {
    ExamplesBooks () {}

    // INFO: A separate examples class for Author was not created because as of now
    // in our class diagrams, Author is really just a part of Book.
    Author robertGreene = new Author ("Robert Greene", 1);
    Author chanakya = new Author ("Chanakya", 2);

    // NOTE: it is good idea to be more specific when referring to variables in an OOP lang
    // notice the `this.xyz` below which specifically denotes that xyz is the **field** in this class.
    // This avoids naming conflicts if there also exists another xyz accessible in this class (imported or defined somewhere else)
    Book howToInfluencePeople = new Book ("How To Influence People", this.robertGreene, 420);
    Book arthaShastra = new Book ("Artha Shastra", this.chanakya, 42069);

    // WARN: order of definition matters!!! the below won't work 
    // Book arthaShastra = new Book ("Artha Shastra", this.chanakya, 42069);
    // Book howToInfluencePeople = new Book ("How To Influence People", this.robertGreene, 420);
    // because this.chanakya does not exist yet.
    // Field access in general works like follows -> `someObject.someField` ->
    // if we try to access the field `this.howToInfluencePeople.author.name` our program will encounter runtime crash.
}
```

See also: [[L3_Methods_for_simple_classes]]

