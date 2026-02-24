---
id: L_10_Customizing_constructors_for_correctness_and_convenience
aliases:
  - L_10_Customizing_constructors_for_correctness_and_convenience
tags: []
---

# L_10_Customizing_constructors_for_correctness_and_convenience
Reasons for customized constructors, designs for custom constructors.

For common customized behaviour that we observe in use of our code, we can overload constructor params so that the caller can only pass in what is absolutely necessary.
    Also, when writing convenience constructors(usually in abstract classes), remember you can reuse the existing constructor with `this()` to prevent code duplication when needed.

This is also good place to discuss validating the args passed to constructor.
A rudimentary approach to this is validate with some conditions and if check fails, `throw` an `Exception` which will crash with a message.
Later we will see how to actually handle Exceptions and try to recover from the illegal state and continue execution.

If you have a method that is not doing anything specific with `this` class, it is common practice to extract such methods into a Utils helper class
> The motivation is that we don't really want to have methods in a class and not in the interface which it implements.


