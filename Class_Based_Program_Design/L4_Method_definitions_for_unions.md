---
id: L4_Method_definitions_for_unions
aliases:
  - L4_Method_definitions_for_unions
tags: []
---

# L4_Method_definitions_for_unions
- Design methods for unions of classes of data.
- Dynamic dispatch.
- Practice using wish lists.

## Adding methods to interfaces

Interfaces define the contract for any class that implements said Interface.
Which means the calling code can be certain that if an Interface says that a method is available then any Class that implements the Interface will have that method available.

> [!tip]
> For now remomber that any method inside a class that comes from an interface needs to be preceded with the `public` keyword.
> Also we should avoid adding methods to such a class that are not in the interface.

### Example for Shapes
Here are our two classes that represent shapes - we defined them to implement the common interface IShape.

The class diagram below shows our ambitious program - of designing four methods for these classes.
```
                  +-----------------------------------+
                  | IShape                            |
                  +-----------------------------------+
                  +-----------------------------------+
                  | double area()                     |
                  | double distanceToOrigin()         |
                  | IShape grow(int inc)              |
                  | boolean isBiggerThan(IShape that) |
                  +-----------------------------------+
                                 |
                                / \
                                ---
                                 |
               -------------------------------
               |                             |
+-----------------------------------+        |
| Circle                            |        |
+-----------------------------------+        |
| int x                             |        |
| int y                             |        |
| int radius                        |        |
| String color                      |        |
+-----------------------------------+        |
| double area()                     |        |
| double distanceToOrigin()         |        |
| IShape grow(int inc)              |        |
| boolean isBiggerThan(IShape that) |        |
+-----------------------------------+        |
                                             |
                      +-----------------------------------+
                      | Square                            |
                      +-----------------------------------+
                      | int x                             |
                      | int y                             |
                      | int size                          |
                      | String color                      |
                      +-----------------------------------+
                      | double area()                     |
                      | double distanceToOrigin()         |
                      | IShape grow(int inc)              |
                      | boolean isBiggerThan(IShape that) |
                      +-----------------------------------+
```

We want to design the following methods that would work for any shape - the two we have defined now, and any other shape class we may define in the future (for example a Triangle class).
```java
// to compute the area of this shape
double area();
 
// to compute the distance from this shape to the origin
double distanceToOrigin();
 
// to increase the size of this shape by the given increment
IShape grow(int inc);
 
// is the area of this shape bigger than the area of the given shape?
boolean isBiggerThan(IShape that);
```

#### Design `area` method
To compute the area of a shape in DrRacket, the function purpose, signature and header would have been:
```racket
;; to compute the area of the given shape
;; area : Shape -> Number
(define (area ashape) ...)
```
and the template would have been:
```racket
;; to compute the area of the given shape
;; area : Shape -> Number
(define (area ashape)
  (cond
       [(circle? ashape) ...]
       [(square? ashape) ...]))
```
In Java the methods that deal with each type of object are defined within the corresponding class definitions for those objects. So the area method that computes the area of a circle is defined in the Circle class, and the area method that computes the area of a square is defined in the Square class.
> [!hint]
> In Java we dont need to check for the union members i.e. cond is not needed.
> This is instead handled by [[#Dynamic Dispatch]]

A union of several classes in Java is represented by an interface type. We would like to assure that every class that implements our IShape interface does indeed define the method area. The interface definition, that so far was just an empty shell of code, is now used to define method headers for some methods. Interfaces define a signature: one that must be upheld by every class that claims to implement the interface, and that can be relied upon by every client of the interface. Specifically, every class that implements the interface is required to implement all methods that the interface specifies. In turn, whenever we use any object of the type specified by the interface, we are guaranteed that we can invoke every method defined in the interface on this object.

    For now it seems redundant to have to write public; surely Java can simply detect that methods were declared in an interface, so why do we have to write it ourselves? The full answer to that will have to wait until Object Oriented Design...

New keyword: Whenever we define methods in a class that were declared in an interface, we must mark them as public. Our code would look like this:

```java
// to represent a geometric shape
interface IShape {
  // to compute the area of this shape
  double area();
  // We'll add more methods later
}
 
// to represent a circle
class Circle implements IShape {
  int x; // represents the center of the circle
  int y;
  int radius;
  String color;
 
  Circle(int x, int y, int radius, String color) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
  }
 
  /* TEMPLATE
     FIELDS:
     ... this.x ...                   -- int
     ... this.y ...                   -- int
     ... this.radius ...              -- int
     ... this.color ...               -- String
     METHODS
     ... this.area() ...              -- double
  */
 
  // to compute the area of this shape
  public double area() {
    return Math.PI * this.radius * this.radius;
  }
}
 
// to represent a square
class Square implements IShape {
  int x; // represents the top-left corner of the square
  int y;
  int size;
  String color;
 
  Square(int x, int y, int size, String color) {
    this.x = x;
    this.y = y;
    this.size = size;
    this.color = color;
  }
 
  /* TEMPLATE
     FIELDS:
     ... this.x ...               -- int
     ... this.y ...               -- int
     ... this.size ...            -- int
     ... this.color ...           -- String
     METHODS:
     ... this.area() ...                  -- double
  */
 
  // to compute the area of this shape
  public double area() {
    return this.size * this.size;
  }
}
 
class ExamplesShapes {
  ExamplesShapes() {}
 
  IShape c1 = new Circle(50, 50, 10, "red");
  IShape s1 = new Square(50, 50, 30, "red");
 
  // test the method area in the classes that implement IShape
  boolean testIShapeArea(Tester t) {
    return
    t.checkInexact(this.c1.area(), 314.15, 0.01) &&
    t.checkInexact(this.s1.area(), 900.0, 0.01);
  }
}
```

### Exercise

## Dynamic Dispatch
See above examples, the shapes `s1`, `c1`, `t1` are all said to be `IShape` and yet when we call the `.area()` method on each of them, the method corresponding to the said class is called without conflicts.

> Statically, we only know that different objects of different classes adhere to certain interface, thus *all classes implement the methods* in the interface and yet at runtime *for a given object, the method from it's own class* will be invoked is made possible by Dynamic dispatch.
> The fact that the dynamically, the correct method will be called based on the type is called **Dynamic dispatch**.

## Resusable Abstractions
See also [[Systematic Program Design#Helper Functions]] and [[Systematic Program Design#Abstraction]]

> [!hint]
> As opposed to `racket` where we would write a helper function to abstract away duplicate code, in `Java` we need to write a helper Class for our abstraction.
> This is because we can't just have a function in OOP languages, we need a method that belongs to a class.

Let’s implement the distanceToOrigin method, which will return the distance to the edge of a circle, or the distance to the top-left corner of a square. If we naively “just start writing” code, we’ll wind up with this poorly-designed result:
```java
class Circle implements IShape {
  ...
  /* TEMPLATE
     FIELDS:
     ... this.x ...                   -- int
     ... this.y ...                   -- int
     ... this.radius ...              -- int
     ... this.color ...               -- String
     METHODS
     ... this.area() ...              -- double
     ... this.distanceToOrigin() ...  -- double
  */
  public double distanceToOrigin() {
    return Math.sqrt(this.x * this.x + this.y * this.y) - this.radius;
  }
}
class Square implements IShape {
  ...
  /* TEMPLATE
     FIELDS:
     ... this.x ...                   -- int
     ... this.y ...                   -- int
     ... this.size ...                -- int
     ... this.color ...               -- String
     METHODS
     ... this.area() ...              -- double
     ... this.distanceToOrigin() ...  -- double
  */
  public double distanceToOrigin() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
  }
}
```
Notice that these two methods are very similar, are operating on nearly identical data, and are producing nearly the same result. If only we didn’t have to write out the Pythagorean formula twice – chances are good we’ll make a mistake at least once. And what if in the future we add more kinds of IShapes: will we have to write the formula again?

Also confusing: while both shapes have x and y fields, they mean slightly different things: the center of the circle versus the top-left corner of the square.

At this point, the design recipe for abstraction says “make a helper function”. But Java has no functions: it only has classes, interfaces, and methods. But that gives us an idea. We can solve all these problems at once, by factoring out the x and y fields into a helper class, which we will call CartPt (for Cartesian Point).

```java
// To represent a 2-d point by Cartesian coordinates
class CartPt {
  int x;
  int y;
  CartPt(int x, int y) {
    this.x = x;
    this.y = y;
  }
}
 
class Circle implements IShape {
  CartPt center; // NEW!  And its name is far more helpful
  int radius;
  String color;
  Circle(CartPt center, int radius, String color) {
    this.center = center;
    this.radius = radius;
    this.color = color;
  }
  ...
}
class Square implements IShape {
  CartPt topLeft; // NEW!  And its name is far more helpful
  int size;
  String color;
  Square(CartPt topLeft, int size, String color) {
    this.topLeft = topLeft;
    this.size = size;
    this.color = color;
  }
  ...
}
```

    Do Now!
    Revise the class diagram above to include CartPt and these changes to Circle and Square.

Now we can add methods to CartPt that might be helpful to us in implementing methods for IShapes:

```java
class CartPt {
  ...
  // To compute the distance from this point to the origin
  double distanceToOrigin() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
  }
}
```

    Do Now!
    Revise the distanceToOrigin methods in Circle and Square to delegate to this method on CartPt.

```java
class Circle implements IShape {
  ...
  /* TEMPLATE
     FIELDS:
     ... this.center ...                    -- CartPt
     ... this.radius ...                    -- int
     ... this.color ...                     -- String
     METHODS
     ... this.area() ...                    -- double
     ... this.distanceToOrigin() ...        -- double
     METHODS ON FIELDS ----- NEW!
     ... this.center.distanceToOrigin() ... -- double
  */
  public double distanceToOrigin() {
    return this.center.distanceToOrigin() - this.radius;
  }
}
class Square implements IShape {
  ...
  /* TEMPLATE
    FIELDS:
    ... this.topLeft ...                    -- CartPt
    ... this.size ...                       -- int
    ... this.color ...                      -- String
    METHODS
    ... this.area() ...                     -- double
    ... this.distanceToOrigin() ...         -- double
    METHODS ON FIELDS ----- NEW!
    ... this.topLeft.distanceToOrigin() ... -- double
  */
  public double distanceToOrigin() {
    return this.topLeft.distanceToOrigin();
  }
}
```
Much better! This is one common example of an important principle in program design: Don’t Repeat Yourself. If there is a way to refactor your code, to extract common elements and separate them into reusable abstractions, it’s probably a good idea to do that.

In this case, we get to extend our template with a new section, methods of fields, that gives us access to exactly the new functionality that we need.

    Do Now!
    Design a method boolean contains(CartPt point) for IShapes that returns true if the given point is within the shape.

What’s so special about Cartesian coordinates, anyway?
There is a subtle, but very powerful, benefit to abstracting x and y into their own class. Notice now that there is no code, outside the CartPt class itself, that knows or cares about x or y fields. So if we wanted to, we could represent our points as PolarPt, with r and theta fields instead.

    Do Now!
    Design the PolarPt class. Revise the data definitions we have so far, so that Squares and Circles could accept a PolarPt instead of just a CartPt. (Hint: now that a point is not just a CartPt, but one of CartPt or PolarPt, what new construction do you need to inform Java about this relationship?)

    Do Now!
    What goes wrong when trying to implement the contains method?

## Bigger than Method

Let's do the isBiggerThan method. It asks whether this shape’s area is greater than the given shape’s area. If only we had a way to compute the area of shapes, we’d be done. Wait – we are!

    Do Now!
    Design the isBiggerThan method for Circle and Square. Did you get a sense of deja vu on implementing the second one?

```java
// to represent a circle
class Circle implements IShape {
  ...
  /* TEMPLATE
     FIELDS
     ... this.center ...                    -- CartPt
     ... this.radius ...                    -- int
     ... this.color ...                     -- String
     METHODS
     ... this.area() ...                    -- double
     ... this.distanceToOrigin() ...        -- double
     ... this.isBiggerThan(IShape that) ... -- boolean
     METHODS FOR FIELDS:
     ... this.center.distanceToOrigin() ... -- double
  */
  // is the area of this shape bigger than the area of the given shape?
  public boolean isBiggerThan(IShape that) {
    /*---------------------------------------------------
    // TEMPLATE for this method:
    // EVERYTHING from our class-wide template...
    ... this.center ...                    -- CartPt
    ... this.radius ...                    -- int
    ... this.color ...                     -- String
 
    ... this.area() ...                    -- double
    ... this.distanceToOrigin() ...        -- double
    ... this.isBiggerThan(IShape) ...      -- boolean
 
    ... this.center.distanceToOrigin() ... -- double
 
    // PLUS methods on the parameters
    ... that.area() ...                    -- double
    ... that.distanceToOrigin() ...        -- double
    ... that.isBiggerThan(IShape) ...      -- boolean
    ---------------------------------------------------*/
    return this.area() > that.area();
  }
}
class Square implements IShape {
  ...
  /* TEMPLATE
     FIELDS
     ... this.topLeft ...                   -- CartPt
     ... this.size ...                      -- int
     ... this.color ...                     -- String
     METHODS
     ... this.area() ...                    -- double
     ... this.distanceToOrigin() ...        -- double
     ... this.isBiggerThan(IShape that) ... -- boolean
     METHODS FOR FIELDS:
     ... this.topLeft.distanceToOrigin() ...-- double
  */
  // is the area of this shape bigger than the area of the given shape?
  public boolean isBiggerThan(IShape that) {
    /*---------------------------------------------------
    // TEMPLATE for this method:
    // EVERYTHING from our class-wide template...
    ... this.topLeft ...                   -- CartPt
    ... this.size ...                      -- int
    ... this.color ...                     -- String
 
    ... this.area() ...                    -- double
    ... this.distanceToOrigin() ...        -- double
    ... this.isBiggerThan(IShape) ...      -- boolean
 
    ... this.topLeft.distanceToOrigin() ...-- double
 
    // PLUS methods on the parameters
    ... that.area() ...                    -- double
    ... that.distanceToOrigin() ...        -- double
    ... that.isBiggerThan(IShape) ...      -- boolean
    ---------------------------------------------------*/
    return this.area() > that.area();
  }
}
```

    The implementations of both methods are identical! We’ll see in Lecture 9: Abstract classes and inheritance how to eliminate this duplication
