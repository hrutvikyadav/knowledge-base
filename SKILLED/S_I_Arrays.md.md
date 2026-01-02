---
id: S_I_Arrays
aliases:
  - S_I_Arrays
tags: []
---

# Arrays

## Problems

### 1. Implement an array

### 2. Walk a matrix

You are given a matrix represented by an r x c two-dimensional array of integers. Starting at the root matrix[row = 0][column = 0] and walking from the perimeter of the matrix towards the center, you want to touch each element once in the matrix by traversing it clockwise in spiral order. Return a 1-dimensional array of all the elements from the matrix in the order you visited them.

**Write a function walkMatrix that takes an r X c 2D array and returns a 1D array of all the elements in the matrix printed in clockwise order.**

> [!note]
Do not consider the result array when calculating your `space complexity`.

```js
// Input
const matrix = [
  [0, 1, 2, 3],
  [11, 12, 13, 4],
  [10, 15, 14, 5],
  [9, 8, 7, 6],
];

// Output: walkMatrix(matrix)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];
```

#### Breakdown

#### ADT
#htdd
[[SKILLED_Matrix_walk_2025-09-25 15.19.05.excalidraw]]

#### Validations
1. You can do this in linear time O(n) and only need to visit each cell once.
2. You may be tempted to use an additional data structure which would add an additional linear O(n) space. Based on the board dimensions and location, you can actually solve this with constant space O(1).
3. Ensure you handle your indexes well and don't have off-by-one when tracking or let them bleed into cells you have already visited.
4. Does your solution handle any board dimension r x c? The input can be a square, vertical rectangle, or horizontal rectangle.
5. Does your solution handle an empty matrix [[]] 0 x 0 input?
6. Does your solution handle a 1 x c and r x 1 input correctly without repeating values?

#### Big O Complexity
This question can be solved in linear time O(n) and constant space O(1).

We have to walk through the matrix items once, so we know it can't be any better than linear time, and since we don't add any additional data structures (it asks for an array as the return type, so we won't count that against our space), we know we can solve it in constant space if we track our visited values with variables instead of an additional data structure.

#### Question Variations
There are a few different ways to test how you think through a matrix problem. Read through these and make sure you feel comfortable based on what you just learned.

What if we inverted the problem and provided an input integer n and want to build an n x n square matrix where the values are 1 - n printed clockwise from matrix[0][0] to the center.
Walking a matrix clockwise is only one way to do it. Think through how you would handle other paths such as walking vertically column-by-column or in a zig-zag diagonally.
What if this was a 3-D+ array? It would be harder to visualize, but the solution builds off of what we just did.
How would you rotate a matrix (or image) 90°?
This can also be solved recursively.

#### Learning Outcomes
This question or variations of it are common in interviews. It is the ultimate question to understand array indexing. If you are comfortable with this problem and its variations, you are on a great path to navigating arrays during an interview.

While it's not the most challenging problem in terms of creating a complex algorithm, it does test if a developer can transform their logic into code.

We learned how to navigate through multi-dimensional array indexes
We thought through edge cases and off-by-one traps
We distilled our problem down to a repetitive pattern of simple logic
We codified our logic

### 3. Product Array
Given an array of integers, write a function buildProductArray that returns an array where each item is the product of all the items in the input array except for the item at that index.

#### Constraints:
- Solve this without using division
- You can create a results array, and it won't count against your space complexity
- Memory may be a concern though, so try to limit your use of additional data structures

```ts
// Input
const input = [1, 2, 3, 4, 5];

// Output: buildProductArray(input)
// [2*3*4*5, 1*3*4*5, 1*2*4*5, 1*2*3*5, 1*2*3*4]
[120, 60, 40, 30, 24];
```

### 4. 
