---
id: L2_Data_definitions_unions
aliases:
  - L2_Data_definitions_unions
tags: []
---

# L2_Data_definitions_unions
- Design of classes that represent a disjoint union of sets of data.
- Extending unions to represent self-referential data.

## Unions
> Closely related to enum **but** where *each or at least one* enum member is non primitive data, see [[Enumeration]] and [[Itemization]]

> [!hint]
> Think algebraic data types in rust.
> `enum TaskStatus { Todo, InProgress(ExpectedCompletionDate), Hold(String - reason), Block(Task that is blocking) }`
> This is a union(TaskStatus) of sets(each member) where at least one is non distinct(disjoint)
> Todo **is-a** TaskStatus, InProgress **is-a** TaskStatus and so on...
>> [!tip]
>> **is-a** relationship is represented with an Interface in Java as we will see below.

Suppose we want to represent train stations for the subway and for the commuter lines. Each station has a name and the name of the line that serves it.

    We’ll see in a few days how to represent stations like Downtown Crossing, with multiple train lines available. Much later, we may talk about the challenges of how to represent stations like Ruggles, that are both subway and commuter rail stations.

A subway station also has a price it costs to get on the train. (This is a simplification: For the commuter rail the price also depends on the exit station, but for now we’ll assume that all commuter rail customers are traveling between their entry station and South Station, so the prices depend only on where they board.) Additionally, a station on the commuter line may be skipped by the express trains, so we need to record this information.
Examples:
- Harvard station on the Red line costs $1.25 to enter
- Kenmore station on the Green line costs $1.25 to enter
- Riverside station on the Green line costs $2.50 to enter
- Back Bay station on the Framingham line is an express stop
- West Newton stop on the Framingham line is not an express stop
- Wellesley Hills on the Worcester line is not an express stop

In racket we would do something like
```racket
;; IStation is one of
;; -- T Stop
;; -- Commuter Station
 
;; T Stop is (make-tstop String String Number)
(define-struct tstop (name line price))
 
;; Commuter Station is (make-commstation String String Boolean)
(define-struct commstation (name line express))
 
(define harvard (make-tstop "Harvard" "red" 1.25))
(define kenmore (make-tstop "Kenmore" "green" 1.25))
(define riverside (make-tstop "Riverside" "green" 2.50))
 
(define backbay (make-commstation "Back Bay" "Framingham" true))
(define wnewton (make-commstation "West Newton" "Framingham" false))
(define wellhills (make-commstation "Wellesley Hills" "Worcester" false))
```

These data definitions can also be represented by the following class diagram:

```
             +----------+
             | IStation |
             +----------+
             +----------+
                   |
                  / \
                  ---
                   |
       -----------------------
       |                     |
+--------------+    +-----------------+
| TStop        |    | CommStation     |
+--------------+    +-----------------+
| String name  |    | String name     |
| String line  |    | String line     |
| double price |    | boolean express |
+--------------+    +-----------------+
```

Here we introduce new concepts-
Interface in Java is a common type that can be implemented by multiple classes.
Above, a TStop **is-a** IStation and CommStation **is-a** IStation

```java
// to represent a train station
interface IStation {
}
 
// to represent a subway station
class TStop implements IStation {
  String name;
  String line;
  double price;
 
  TStop(String name, String line, double price) {
    this.name = name;
    this.line = line;
    this.price = price;
  }
}
 
// to represent a stop on a commuter line
class CommStation implements IStation {
  String name;
  String line;
  boolean express;
 
  CommStation(String name, String line, boolean express) {
    this.name = name;
    this.line = line;
    this.express = express;
  }
}

class ExamplesIStation{
  ExamplesIStation() {}
 
  /*
   Harvard station on the Red line costs $1.25 to enter
   Kenmore station on the Green line costs $1.25 to enter
   Riverside station on the Green line costs $2.50 to enter
 
   Back Bay station on the Framingham line is an express stop
   West Newton stop on the Framingham line is not an express stop
   Wellesely Hills on the Worcester line is not an express stop
  */
 
  IStation harvard = new TStop("Harvard", "red", 1.25);
  IStation kenmore = new TStop("Kenmore", "green", 1.25);
  IStation riverside = new TStop("Riverside", "green", 2.50);
 
  IStation backbay = new CommStation("Back Bay", "Framingham", true);
  IStation wnewton = new CommStation("West Newton", "Framingham", false);
  IStation wellhills = new CommStation("Wellesley Hills", "Worcester", false);
}
```

## Self-referencial Unions

Suppose we want to represent an ancestry tree for a person, naming the ancestors as far as we can remember, and using “unknown” for those nobody can remember or trace.

> [!tip]
> Two important skills you need to practice early are
> - design a representation of the given information as data
> - interpret the given data as the information it represents

So, here, to understand what the data shown above represents, we may want to draw the ancestor tree, so we can tell easily how the different people are related. The given data represents the following ancestor tree:

```
             Dan
          /       \
      Jane        John
    /      \      /  \
  Mary   Robert  ?    ?
 /   \    /   \
?     ?  ?     ?
```

class diagram that represents this data definition looks like this:
```
             +------------------+
             |  +-------------+ |
             |  |             | |
             v  v             | |
           +-----+            | |
           | IAT |            | |
           +-----+            | |
             / \              | |
             ---              | |
              |               | |
      -----------------       | |
      |               |       | |
+---------+   +-------------+ | |
| Unknown |   | Person      | | |
+---------+   +-------------+ | |
+---------+   | String name | | |
              | IAT mom     |-+ |
              | IAT dad     |---+
              +-------------+
```
> Note the two kinds of arrows here: Person and Unknown both implement IAT, while Person also has two IAT fields.
> Both the arrows represent the 2 relationships- **is-a**(denotes union), **has-a**(denotes self-reference or reference)

```java
// to represent an ancestor tree
interface IAT{ }
 
// to represent an unknown member of an ancestor tree
class Unknown implements IAT{
  Unknown() {}
}
 
// to represent a person with the person's ancestor tree
class Person implements IAT{
  String name;
  IAT mom;
  IAT dad;
 
  Person(String name, IAT mom, IAT dad) {
    this.name = name;
    this.mom = mom;
    this.dad = dad;
  }
}

// examples and tests for the class hierarchy that represents
// ancestor trees
class ExamplesAncestors{
  ExamplesAncestors() {}
 
  IAT unknown = new Unknown();
  IAT mary = new Person("Mary", this.unknown, this.unknown);
  IAT robert = new Person("Robert", this.unknown, this.unknown);
  IAT john = new Person("John", this.unknown, this.unknown);
 
  IAT jane = new Person("Jane", this.mary, this.robert);
 
  IAT dan = new Person("Dan", this.jane, this.john);
}
```

