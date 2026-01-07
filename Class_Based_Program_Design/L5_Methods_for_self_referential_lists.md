---
id: L5_Methods_for_self_referential_lists
aliases:
  - L5_Methods_for_self_referential_lists
tags: []
---

# L5_Methods_for_self_referential_lists
Designing classes to represent lists.
- Methods on lists
- including basic recursive methods
- sorting

See [[Self Referential Data]]
> [!hint]
> Keep in mind (from `racket`) the **one of** and conversely the **is-a** relationship, which suggests an interface in `Java`
> along with the **has-a**, conversely **belongs-to** which suggests a reference to another or self.

Representing lists
The following class diagram defines a class hierarchy that represents a list of books in a bookstore:
```
               +--------------------------------+
               | ILoBook                        |<----------------------+
               +--------------------------------+                       |
               +--------------------------------+                       |
               | int count()                    |                       |
               | double salePrice(int discount) |                       |
               | ILoBook allBefore(int y)       |                       |
               | ILoBook sortByPrice()          |                       |
               +--------------------------------+                       |
                                |                                       |
                               / \                                      |
                               ---                                      |
                                |                                       |
                  -----------------------------                         |
                  |                           |                         |
+--------------------------------+   +--------------------------------+ |
| MtLoBook                       |   | ConsLoBook                     | |
+--------------------------------+   +--------------------------------+ |
+--------------------------------+ +-| Book first                     | |
| int count()                    | | | ILoBook rest                   |-+
| double salePrice(int discount) | | +--------------------------------+
| ILoBook allBefore(int y)       | | | int count()                    |
| ILoBook sortByPrice()          | | | double salePrice(int discount) |
+--------------------------------+ | | ILoBook allBefore(int y)       |
                                   | | ILoBook sortByPrice()          |
                                   | +--------------------------------+
                                   v
                   +--------------------------------+
                   | Book                           |
                   +--------------------------------+
                   | String title                   |
                   | String author                  |
                   | int year                       |
                   | double price                   |
                   +--------------------------------+
                   | double salePrice(int discount) |
                   +--------------------------------+
```
Let’s make some examples
```java
//Books
Book htdp = new Book("HtDP", "MF", 2001, 60);
Book lpp = new Book("LPP", "STX", 1942, 25);
Book ll = new Book("LL", "FF", 1986, 10);
 
// lists of Books
ILoBook mtlist = new MtLoBook();
ILoBook lista = new ConsLoBook(this.lpp, this.mtlist);
ILoBook listb = new ConsLoBook(this.htdp, this.mtlist);
ILoBook listc = new ConsLoBook(this.lpp,
                new ConsLoBook(this.ll, this.listb));
ILoBook listd = new ConsLoBook(this.ll,
                  new ConsLoBook(this.lpp,
                    new ConsLoBook(this.htdp, this.mtlist)));
```

## Basic list computations
## Sorting

