---
layout: default
title: Searching and Sorting I
parent: Data Structures and Algorithms I
nav_order: 2
---

# Searching and Sorting I
We consider the following motivating example for this entire section. Alice is a marine biologist who has harvested a large number of waterborne bacteria and sequenced their DNA. She has obtained one billion (1,000,000,000) unique 30 bp DNA sequences. Alice would like to publish the data she has collected as a public database for other scientists to use, so that they can search for their sequences of interest. How can Alice best store her data?

## Storing the Data 'as-is' is Inefficient
The simplest option for Alice is store the data 'as-is'. That is, she just needs to upload her gigantic `.fasta` file and let the other scientists search for their sequences on their own. Unfortunately, this is really slow. In fact, the average number of sequences checked will be 

## Binary Search
We hereby introduce our first algorithm: **binary search**. To describe any algorithm, we need the following: the algorithm's input, the algorithm's output, how it works (i.e., the algorithm itself), and the algorithm's performance (how it scales with respect to input size).

- Input(s): A sorted list *ls* and a value *v* to search.
- Output: A boolean value (True if *v* is in *S* and False otherwise)
- Time Complexity: *O(logn)*
- Space Complexity: *O(1)*

Let's first examine the intuition behind the algorithm.

1. Given the input list *ls*, we want to check if our value *v* is smaller than, equals to, or bigger than the middle element of *ls* (which we denote as *m*).
2. If *v* is equal to *m*, we are done and our algorithm can return True immediately.
3. However, if *v* is smaller than *m*, we know that we can ignore the second half of *ls*. This is because *ls* is a sorted list: everything after *m* is going to be larger than *m*. Hence, if *v* is in *ls*, it must be in the first half of *ls*.
4. We can now replace *ls* with just the first half of *ls* and repeat the three steps above.
5. If we go through the entire first half of the original input list *ls* and cannot find our value *v*, this means that *v* is smaller than the smallest value in *ls*, i.e., *v* is not in the list. In this case, we return False.
6. A similar procedure applies if *v* is larger than *m* in Step 3.

We can write this algorithm in Python:

```python
def binary_search(ls, v):
    low = 0
    high = len(ls) - 1
    while low <= high:
        mid = low + ((high - low) // 2)
        if ls[mid] == v:
            return True
        elif ls[mid] < v:
            low = mid + 1
        else:
            high = mid - 1
    return False
```

### Worked Examples
To better understand the Python implementation of the algorithm, we can consider a few worked examples.

#### Example 1: *ls*=[0,1,2,3,4,5,6,7,8,9,10], *v=3*
1. We start by setting `low=0` and `high=10`.
2. We enter the `while` loop next, and first set `mid=5` using the value of `low` and `high` given in Step 1.
3. The value of `ls[mid]` is `ls[5]=5`. Since `5>3`, the `else` block gets executed and we set `high=4`.
4. The `while` loop repeats, and now `mid=2`, using the new value of `high` from Step 3.
5. The value of `ls[mid]` is now `ls[2]=2`. Since `2<3`, the `elif` block gets executed and we set `low=3`.
6. The `while` loop repeats, and now `mid=3`, using the new value of `low` from Step 5.
7. The value of `ls[mid]` is `ls[3]=3`. Since `3=3`, the `if` block gets executed and the function returns `True`. This means that *v=3* is an element of *ls*.

### A Minor Extension
Our current algorithm only returns a True or False value depending on whether our value *v* is in the input list *ls*. We can slightly improve the algorithm to return the index of *v* if it is in *ls*, and to return some other default value otherwise. By convention, the default value is -1. That is, if *v* is not in *ls*, our `binary_search` function should return -1. Here is the modified implementation: the only lines changed are the two `return` statements.

```python
def binary_search(ls, v):
    low = 0
    high = len(ls) - 1
    while low <= high:
        mid = low + ((high - low) // 2)
        if ls[mid] == v:
            return mid # this line was changed
        elif ls[mid] < v:
            low = mid + 1
        else:
            high = mid - 1
    return -1 # this line was changed
```

## Conclusion
To summarize:
1. Searching an unsorted list is quite inefficient, while searching a sorted list is quite efficient.
2. The binary search algorithm allows us to search a sorted list using *O(logn)* steps.
3. We have an incentive to sort our data before storing it to allow for easy retrieval.
