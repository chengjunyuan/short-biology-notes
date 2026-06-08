---
layout: default
title: Algorithm Analysis
parent: Data Structures and Algorithms I
nav_order: 1
---

# Algorithm Analysis
Before discussing the 'real' algorithms, let's take a moment to understand what it means for an algorithm to be 'efficient'. Generally speaking, our algorithms are measured in two dimensions: the amount of storage space it uses, and the number of operations it performs. A space-efficient algorithm uses the least storage space required, and a time-efficient algorithm performs the fewest number of operations required. We generally quantify the performance of an algorithm in terms of the size of the input data for the algorithm.

As a simple example, consider the following function in Python:

```python
def counter(n):
    for i in range(n):
        print(i)
```

The `counter()` function takes in a single integer `n` and prints stuff out accordingly. We can observe that the number of `print()` operations performed by `counter()` grows linearly as `n` increases. As a slightly more complicated example, consider the function:

```python
def complex_counter(n):
    for i in range(n):
        for j in range(n):
            print(i * j)

    for i in range(n):
        print(i)
```

We observe that given the input `n`, the function `complex_counter()` executes the `print()` function a total of $$n^2+n$$ times.