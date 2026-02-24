---
id: L13_Abstracting_over_behaviour
aliases:
  - L13_Abstracting_over_behaviour
tags: []
---

# L13_Abstracting_over_behaviour

Filtering example.
We start off by adding methods to our `ILoXyz`.
Adding more filter methods resulted in duplicated code.
Only difference is the code was the filter predicate. We want to *pass this predicate function to a single method* which abstracts the duplicated code and calls the provided filter.
    In Java we dont have **higher order functions**. What we need to do is *extract* the filter methods from our Interface into their **own Class**.
Then we add an interface for these classes - `IFilterPredicate`
And finally we call the method from our new interface in our `ILoXyz`.
> Now adding a filter is just adding a class that implements the `IFilterPredicate`, and use it when calling.
> These filter classes are just one function/method inside classes with nothing more to it. So they are called *function objects*

<!-- TODO: add codes at each step -->
