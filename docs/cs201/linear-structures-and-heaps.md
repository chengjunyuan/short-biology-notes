---
layout: default
title: Linear Structures, Heaps & Priority Queues
parent: CS201 Data Structures & Algorithms I
nav_order: 3
---

# Linear Structures, Heaps & Priority Queues

How data is laid out in physical RAM dictates memory cache hits and execution speed. Understanding dynamic arrays, linked lists, stacks, queues, and binary heaps allows you to build efficient data processing pipelines.

---

## 1. Dynamic Arrays vs. Linked Lists

```
Dynamic Array:   [ Item 0 ][ Item 1 ][ Item 2 ][ Item 3 ]   <-- Contiguous block in RAM
                                                                (Cache-friendly O(1) index)

Linked List:     [ Data | *next ] ──> [ Data | *next ] ──> [ Data | null ]  <-- Scattered in RAM
                                                                              (Pointer chasing)
```

### Direct Comparison
* **Dynamic Arrays (e.g., Python `list`, C++ `vector`)**:
  * **Contiguous Memory**: Elements sit right next to each other in RAM.
  * **Random Access**: $O(1)$ constant time lookup by index (e.g., `arr[42]`).
  * **Cache Locality**: The CPU pre-fetches contiguous blocks into ultra-fast L1/L2 cache, making array traversal remarkably fast.
  * **Amortized Append**: Appending to the end is $O(1)$ amortized (doubles array capacity when full).
* **Linked Lists**:
  * **Scattered Nodes**: Each node holds data plus memory pointers (`*next`, `*prev`).
  * **Lookup**: $O(N)$ sequential traversal required.
  * **Cache Misses**: Every node jump is a memory pointer dereference across non-contiguous RAM.
  * **Advantage**: $O(1)$ insertion/deletion *only if* you already hold a pointer to the target node.

---

## 2. Stacks & Queues

* **Stack (LIFO: Last-In, First-Out)**:
  * Operations: `push(x)` and `pop()` in $O(1)$ time.
  * **Biological Use Case**: Validating balanced base pairings in RNA secondary structure notation (dot-bracket syntax: `(((...)))`).
* **Queue (FIFO: First-In, First-Out)**:
  * Operations: `enqueue(x)` and `dequeue()` in $O(1)$ time.
  * **Biological Use Case**: Breadth-First Search (BFS) for exploring biological interaction networks level-by-level; buffer queues for asynchronous sequencing read processing.

---

## 3. Binary Heaps & Priority Queues

A **Priority Queue** is an abstract data structure where each element has an associated priority, and the element with the highest (or lowest) priority is always served first.

### Why not use a sorted list or unsorted list?
* Unsorted List: $O(1)$ insert, but $O(N)$ to find and extract the minimum.
* Sorted List: $O(1)$ extract minimum, but $O(N)$ to insert a new element in sorted position.
* **Binary Heap**: Achieves **$O(\log N)$ insertion** and **$O(\log N)$ extraction** with $O(1)$ peek!

```
         [ Min-Heap ]
             ( 2 )
            /     \
         ( 5 )   ( 8 )
        /    \
      (12)  (15)
  (Every parent is <= its children)
```

### Complete Binary Tree Array Representation
A binary heap can be stored inside a single flat array without storing node pointers:
* For an element at index $i$:
  * Left Child: $2i + 1$
  * Right Child: $2i + 2$
  * Parent: $\lfloor (i - 1) / 2 \rfloor$

---

## 4. Biological Application: $K$-Way Merge of Massive Sorted Files

In bioinformatics, BAM/CRAM alignment files from multiple sequencing lanes are sorted by genomic coordinates.

**The Problem**: Suppose you have $K = 50$ distinct sorted files (each containing $10^7$ reads). How do you merge them into a single sorted master file without loading all 50 files into memory at once?

```
File 1: [ chr1:100, chr1:250, ... ] ──┐
File 2: [ chr1:105, chr1:180, ... ] ──┼──> [ Min-Heap (Size K=50) ] ──> Output Master Stream
File 3: [ chr1:110, chr1:190, ... ] ──┘        Pop min -> Write
                                               Read next record from same file
```

1. Initialize a Min-Heap of size $K$ containing the first record from each of the $K$ files.
2. `extract_min()` from the heap (takes $O(\log K)$ time) and write it to the master output.
3. Read the next record from the file that produced the extracted item, and `insert()` it into the heap ($O(\log K)$).
4. **Total Merging Complexity**: $O(N \log K)$ time and only $O(K)$ memory, where $N$ is total reads across all files!

> **Takeaway**: Dynamic arrays dominate linear storage due to CPU cache locality. Binary heaps provide efficient $O(\log N)$ priority retrieval, enabling streaming multi-file merges on massive genomic datasets.
