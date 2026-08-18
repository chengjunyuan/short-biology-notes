---
layout: default
title: CS201 Data Structures & Algorithms I
nav_order: 4
has_children: true
---

# CS201: Data Structures & Algorithms I

Welcome to **CS201: Data Structures & Algorithms I**!

High-throughput DNA sequencing generates billions of short nucleotide reads in a single instrument run. While raw compute power continues to scale, algorithmic efficiency is the difference between a pipeline taking 30 minutes vs. 30 days.

Data Structures and Algorithms (DSA) is the foundation of computational biology. This course is modeled on classical rigorous algorithms curricula (such as NUS CS2040S), with every concept motivated by the **computational demands of biological data processing**.

---

## Course Modules

1. [**Algorithm Analysis & Asymptotic Complexity**](algorithm-analysis.md)
   * *Rationale*: How to rigorously predict execution time and memory scaling as genome size or sequencing depth grows ($O$, $\Omega$, $\Theta$).
2. [**Searching & Sorting Algorithms**](searching-and-sorting.md)
   * *Rationale*: Why sorting unlocks logarithmic $O(\log N)$ search, when to use comparison sorts (Merge/Quick) vs. linear-time non-comparison sorts (Radix Sort on fixed-length reads).
3. [**Linear Structures, Heaps & Priority Queues**](linear-structures-and-heaps.md)
   * *Rationale*: Dynamic arrays vs. linked lists (cache locality vs. pointer chasing), and using min/max heaps for streaming $k$-way merges of massive coordinate-sorted BAM files.
4. [**Trees, Balanced BSTs, Tries & Disjoint Sets**](trees-and-tries.md)
   * *Rationale*: Why trees overcome array/linked-list trade-offs, how AVL balance guarantees $O(\log N)$ worst-case bounds, how Tries index sequence prefixes, and how Union-Find clusters biological sequences.
5. [**Hashing, Sets & Bloom Filters**](hashing-and-sets.md)
   * *Rationale*: How hash functions achieve $O(1)$ expected lookup, amortized resizing costs, and how probabilistic Bloom filters filter novel $k$-mers in low memory.
6. [**Graph Algorithms & Network Biology**](graphs.md)
   * *Rationale*: Modeling biological networks (metabolic pathways, gene regulatory networks, assembly graphs), BFS/DFS traversals, topological sorting for workflow execution, and shortest path algorithms (Dijkstra, Bellman-Ford, MST).
7. [**Dynamic Programming & Sequence Alignment**](dynamic-programming.md)
   * *Rationale*: Solving problems with optimal substructure and overlapping subproblems without exponential recursion; the core foundation behind Smith-Waterman and Needleman-Wunsch sequence alignment.