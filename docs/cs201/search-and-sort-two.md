---
layout: default
title: Searching and Sorting II
parent: Data Structures and Algorithms I
nav_order: 3
---

# Searching and Sorting II
In the previous section, we introduced the **binary search** algorithm, which allows us to search a sorted list very quickly. This leads to the natural question: how can we sort a list quickly? In this section, we introduce a simple algorithm to sort Alice's one billion DNA sequences efficiently.

## Preliminaries
Before we can sort the database, we need a way of comparing the size of two elements. In this example, we need to compare two DNA sequences and determine which is 'bigger' and which is 'smaller'. Thankfully, this is easy. We can define the following rules for comparing two DNA sequences *X* and *Y*.
1. For the individual nucleotides *A*, *T*, *G*, and *C*, we define the order *A*<*C*<*G*<*T*.
2. If sequence *X* is longer than sequence *Y*, *X*>*Y*.
3. If sequence *X* and sequence *Y* are of the same length, check the first character of *X* and *Y*, denoted *x* and *y* respectively. If *x>y*, then *X*>*Y*.
4. If *x* and *y* are the same, then we repeat the process with the second character of *X* and *Y* (and so forth until the end of the sequences).

We can also write this in Python:

```python
def X_is_bigger_than_Y(X, Y):
    if len(X) > len(Y):
        return True
    elif len(X) < len(Y):
        return False
    else:
        if X[0] > Y[0]:
            return True
        elif X[0] < Y[0]:
            return False
        else:
            return X_is_bigger_than_Y(X[1:], Y[1:])
```

## Insertion Sort
Now that we have a way of comparing two sequences, we can proceed to sort Alice's database.