---
id: L6_Accumulators
aliases:
  - L6_Accumulators
tags: []
---

# L6_Accumulators

Usually when designing methods for [[L2_Data_definitions_unions#Self-referencial Unions|Self referencial unions]] we will need to write helper methods.
A good clue for this is when we want to process information(fields) of both the object(this) and the self-reference; as we won't be able to access fields on interfaces (they don't exist).
This is where we end up passing information(with helper methods) on to the self-reference and thus *accumulate* data/result as we recurse.
> Helpers withour accumulators can also exist

As we proceed through lessons we will enounter more complex accumulators than this.

