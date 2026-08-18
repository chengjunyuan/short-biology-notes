---
layout: default
title: Dynamic Programming & Sequence Alignment
parent: CS201 Data Structures & Algorithms I
nav_order: 7
---

# Dynamic Programming & Sequence Alignment

Why does a naive recursive algorithm to align two biological sequences take exponential time ($O(2^N)$), while the **Needleman-Wunsch** and **Smith-Waterman** algorithms solve it in polynomial time ($O(N \cdot M)$)?

The answer is **Dynamic Programming (DP)**—an algorithmic paradigm that avoids redundant computation by solving each subproblem once and storing its result.

---

## 1. The Core Philosophy of Dynamic Programming

An algorithmic problem is amenable to DP if it exhibits two properties:

1. **Optimal Substructure**: The optimal solution to the global problem contains within it optimal solutions to its smaller subproblems.
2. **Overlapping Subproblems**: A recursive solution repeatedly recalculates identical subproblems rather than generating fresh ones.

```
                  fib(5)
                /        \
            fib(4)        fib(3)  <-- fib(3) recalculated redundantly!
           /      \       /     \
        fib(3)   fib(2) fib(2)  fib(1)
```

---

## 2. The Two Approaches: Top-Down vs. Bottom-Up

### Approach 1: Memoization (Top-Down)
* Keep the natural recursive tree structure.
* Before computing, check if the state is already cached in a hash table or array. If present, return it immediately; otherwise, compute and cache it.

### Approach 2: Tabulation (Bottom-Up)
* Eliminate recursion entirely.
* Solve the simplest base-case subproblems first and iteratively fill a 1D or 2D matrix (table) from smallest to largest state.

---

## 3. The Longest Common Subsequence (LCS) Problem

Given two nucleotide sequences:
* $X = \text{"ACCGT"}$ ($N = 5$)
* $Y = \text{"AGCT"}$ ($M = 4$)

Find the length of the longest subsequence present in both strings in the same relative order.

### The Recurrence Relation
Let $DP[i][j]$ be the length of the LCS of prefixes $X[0 \dots i-1]$ and $Y[0 \dots j-1]$:

$$DP[i][j] = \begin{cases} 
0 & \text{if } i=0 \text{ or } j=0 \\
DP[i-1][j-1] + 1 & \text{if } X[i-1] == Y[j-1] \quad \text{(Match!)} \\
\max(DP[i-1][j], DP[i][j-1]) & \text{if } X[i-1] \ne Y[j-1] \quad \text{(Mismatch / Gap)}
\end{cases}$$

```python
def longest_common_subsequence(seq1: str, seq2: str) -> int:
    n, m = len(seq1), len(seq2)
    # Initialize (n+1) x (m+1) DP matrix with zeros
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if seq1[i - 1] == seq2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
                
    return dp[n][m]
```

### Complexity
* **Time Complexity**: $O(N \cdot M)$ — every cell in the matrix is evaluated in $O(1)$ operations.
* **Space Complexity**: $O(N \cdot M)$ to store the matrix (or $O(\min(N, M))$ if only keeping the previous row).

---

## 4. Prelude to Biological Sequence Alignment

LCS is the direct mathematical foundation for classical bioinformatics sequence alignment algorithms:
* **Needleman-Wunsch (Global Alignment)**: Uses the LCS 2D grid framework with a scoring matrix (match reward, mismatch penalty, linear gap penalty) to align two homologous sequences end-to-end.
* **Smith-Waterman (Local Alignment)**: Adds a lower bound of $0$ ($DP[i][j] = \max(0, \dots)$) to find high-scoring local alignment domains within divergent sequences.

> **Takeaway**: Dynamic Programming turns exponential brute-force searches into practical $O(N \cdot M)$ matrix computations, making whole-gene and protein sequence alignments computationally feasible.
