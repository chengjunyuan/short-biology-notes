---
layout: default
title: Trees, Balanced BSTs, Tries & Disjoint Sets
parent: CS201 Data Structures & Algorithms I
nav_order: 4
---

# Trees, Balanced BSTs, Tries & Disjoint Sets

Linear structures force a compromise: arrays allow $O(1)$ random access but $O(N)$ insertion/deletion in the middle; linked lists allow fast node insertion but $O(N)$ search.

**Tree structures** break this dilemma, enabling dynamic search, insertion, and prefix matching in logarithmic or length-bounded time.

---

## 1. Binary Search Trees (BST) & The Need for Balance

### The BST Invariant
For every node $X$:
* All keys in the left subtree are $< X$.
* All keys in the right subtree are $> X$.

### The Degenerate Tree Problem
If elements are inserted in already-sorted order (`"A"`, `"C"`, `"G"`, `"T"`), an unbalanced BST degenerates into a linear linked list:

```
Unbalanced (Degenerate):        Balanced (AVL Tree):
       ( A )                            ( C )
         \                             /     \
         ( C )                      ( A )   ( G )
           \                                   \
           ( G )                               ( T )
             \
             ( T )
Search: O(N) worst case             Search: O(log N) guaranteed!
```

### Self-Balancing Trees: AVL Trees
* **Balance Factor**: $BF(\text{node}) = \text{height}(\text{left}) - \text{height}(\text{right}) \in \{-1, 0, 1\}$.
* When an insertion or deletion causes $|BF| > 1$, the tree performs $O(1)$ **Tree Rotations** (Left, Right, Left-Right, Right-Left) to restore height balance.
* **Guarantee**: Height is strictly bounded by $O(\log N)$, guaranteeing $O(\log N)$ search, insertion, and deletion always.

---

## 2. Tries (Prefix Trees) for Sequence Indexing

A **Trie** is a specialized tree designed for string retrieval where edges represent individual characters.

```
                  ( root )
                 /   |    \
               [A]  [C]   [G]
              /       \
            [T]       [G]
           /            \
         [G]            [T]
       (ATG)           (CGT)
```

### Why use a Trie over a Hash Map for DNA Sequences?
1. **Prefix Search**: Find all reads sharing an identical 15-bp adapter prefix in $O(L)$ time (where $L$ is sequence length), regardless of how many billions of reads exist in the database ($N$).
2. **Space Compression**: Sequences sharing common prefixes share the same parent nodes in memory.
3. **Worst-Case Bound**: Exact match lookup depends solely on sequence length $O(L)$, completely independent of database size $N$.

---

## 3. Disjoint Set Union (Union-Find) for Sequence Clustering

In computational biology, we frequently need to cluster biological sequences, protein interaction clusters, or connected components.

**The Problem**: Given $N$ sequence clusters, efficiently perform two operations:
1. `union(x, y)`: Merge the cluster containing sequence $x$ with the cluster containing sequence $y$.
2. `find(x)`: Identify the canonical representative ID of the cluster containing $x$.

```python
class UnionFind:
    def __init__(self, size: int):
        self.parent = list(range(size))
        self.rank = [0] * size

    def find(self, x: int) -> int:
        # Path Compression: Flattens tree directly to root on lookup!
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x: int, y: int) -> bool:
        root_x = self.find(x)
        root_y = self.find(y)
        if root_x == root_y:
            return False  # Already in same cluster

        # Union by Rank: Attach smaller tree under deeper tree
        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
        return True
```

### Complexity
With **Path Compression** and **Union by Rank**, any sequence of $M$ operations on $N$ elements executes in **$O(M \cdot \alpha(N))$** time, where $\alpha(N)$ is the Inverse Ackermann function ($\alpha(N) < 5$ for all physical values in the known universe—effectively constant time per operation!).

> **Takeaway**: Self-balancing trees (AVL) guarantee $O(\log N)$ dynamic searching, Tries provide length-bounded $O(L)$ prefix lookups for sequencing reads, and Union-Find clusters biological sequences in near-instantaneous $O(\alpha(N))$ time.
