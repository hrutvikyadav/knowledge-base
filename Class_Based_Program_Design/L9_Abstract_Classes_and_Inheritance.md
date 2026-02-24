---
id: L9_Abstract_Classes_and_Inheritance
aliases:
  - L9_Abstract_Classes_and_Inheritance
tags: []
---

# L9_Abstract_Classes_and_Inheritance

Here we talk about removing code duplication(caused by multiple classes implementing an interface for ex.) with Abstract Classes and Inheritance.
In the concerned classes, it is likely that there will be *some duplication* and *some uniqueness*
- we take the duplicated **part** and put it in another class and call it *`abstract`* class (because it is only part of the impl and is ==not complete==). This abstract class is not meant to be instantiated (it is not complete), only serves the purpose of code reuse.
    This is because, again, in java there is no such thing as an abstract or a helper function, instead we represent these ideas with classes.
- then the unique part is left to be *extended* by the respective class that inherits the abstract class(gets the common data), `implements` the unique parts itself and thus satisfies i.e. ==completes== the *abstract interface*.
    The extending/inheriting class then calls the constructor of the superclass.
- for code that is duplicated across multiple classes but is also different in one or more classes, we *override* (use `@Override`) that code in the inheriting class.

> [!tip]
> If an abstract class is implementing some methods from an interface and leaving out the remaining ones to be implemented by the extending classes, it is a good idea to add the method signatures to the abstract class and mark them as `abstract`. Makes it easier for readers of the code to figure out what is going on.

After doing this kind of implementation you might wonder do we even need the interface we started with. And the answer is probably yes.
If we need to do something different from what is in the abstract class(i.e. differs in implementation), then we have to use our interface. Otherwise, we are stuck with the abstract class and end up overriding a lot of things. Whereas just not extending the abstract class and simply implementing the interface would have been better.

