---
layout: default
title: Searching & Sorting Algorithms
parent: CS201 Data Structures & Algorithms I
nav_order: 2
---

# Searching & Sorting Algorithms

A fundamental task in genomics is querying whether a specific sequence or genomic variant exists in a database of billions of records.

Why do we spend computational time sorting data? **Because sorting transforms an expensive $O(N)$ linear scan into an instant $O(\log N)$ logarithmic search.**

---

## 1. The Power of Binary Search

### Linear Search vs. Binary Search
* **Linear Search ($O(N)$)**: Checks each element sequentially from index $0$ to $N-1$. If querying $N = 10^9$ DNA sequences, an average query inspects $500,000,000$ elements.
* **Binary Search ($O(\log N)$)**: On a **sorted** array, inspects the middle element $M$. If the query $V < M$, discard the entire upper half; if $V > M$, discard the entire lower half. Repeat recursively.
  $$\log_2(1,000,000,000) \approx 30 \text{ inspections maximum!}$$

```python
def binary_search(sorted_arr: list[str], target: str) -> int:
    low = 0
    high = len(sorted_arr) - 1
    
    while low <= high:
        mid = low + ((high - low) // 2)
        if sorted_arr[mid] == target:
            return mid  # Found target at index mid
        elif sorted_arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
            
    return -1  # Target does not exist in dataset
```

---

## 2. Comparison-Based Sorting Algorithms

Any sorting algorithm that determines order strictly through pairwise comparisons ($A < B$) has a theoretical mathematical lower bound of **$\Omega(N \log N)$** comparisons in the worst/average case.

### A. Quadratic Sorts: Insertion Sort & Selection Sort ($O(N^2)$)
* **Insertion Sort**: Builds the sorted array one item at a time by shifting elements.
  * *When to use*: Excellent for **nearly sorted data** ($O(N)$ best-case time complexity). Frequently used as the base-case cutoff in hybrid algorithms (e.g., Timsort in Python).
* **Selection Sort**: Repeatedly scans the unsorted partition for the minimum element. Always $\Theta(N^2)$ comparisons regardless of initial order.

### B. Divide & Conquer: Merge Sort ($O(N \log N)$)
* **Strategy**:
  1. Recursively split the array into halves until single elements remain ($O(\log N)$ recursion levels).
  2. Merge two sorted subarrays in linear time $O(N)$ using two pointers.
* **Properties**:
  * **Guaranteed Worst-Case**: $O(N \log N)$ time always.
  * **Stable**: Preserves relative order of duplicate elements (vital when sorting multi-tiered genomic tables, e.g., sorting by chromosome, then position).
  * **Trade-off**: Requires $O(N)$ auxiliary memory for merge buffers.

```
       [4, 1, 3, 2]
      /            \
   [4, 1]        [3, 2]     (Divide: O(log N) depth)
   /    \        /    \
  [4]   [1]    [3]    [2]
   \    /        \    /
   [1, 4]        [2, 3]     (Merge: O(N) work per level)
      \            /
       [1, 2, 3, 4]         Total Time: O(N log N)
```

### C. In-Place Partitioning: Quick Sort ($O(N \log N)$ average, $O(N^2)$ worst)
* **Strategy**: Pick a pivot element, partition the array in-place so all elements $\le \text{pivot}$ are on the left and elements $> \text{pivot}$ are on the right. Recursively sort partitions.
* **Properties**:
  * **In-Place**: $O(\log N)$ auxiliary space on the call stack.
  * **Cache-Friendly**: Fast practical constant factors.
  * **Worst-Case**: $O(N^2)$ if pivot selection is poor on already-sorted data (mitigated by randomized pivot selection or median-of-three).

---

## 3. Non-Comparison Sorts: Breaking the $O(N \log N)$ Barrier

If our data has a bounded alphabet (e.g., DNA nucleotides $\{A, C, G, T\}$) or fixed-length sequences (e.g., Illumina short reads of length $L = 150\text{ bp}$), we can sort in **linear time** using non-comparison algorithms.

### Radix Sort ($O(N \cdot L)$)
* **Strategy**: Uses **Counting Sort** as a stable sub-routine, sorting sequences digit-by-digit from Least Significant Digit (LSD) to Most Significant Digit (MSD).
* **Biological Context**:
  * For $N = 10^8$ reads of fixed length $L = 100$, Radix Sort runs in $O(100 \cdot N)$ linear operations, completely bypassing the comparison bound.

---

## 4. Sorting Algorithm Comparison

| Algorithm | Best Time | Average Time | Worst Time | Space | Stable? | Primary Use Case in Bioinfo |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Insertion Sort** | $O(N)$ | $O(N^2)$ | $O(N^2)$ | $O(1)$ | Yes | Small or nearly-sorted coordinate buffers |
| **Merge Sort** | $O(N \log N)$ | $O(N \log N)$ | $O(N \log N)$ | $O(N)$ | Yes | External memory sorting (e.g. `samtools sort`) |
| **Quick Sort** | $O(N \log N)$ | $O(N \log N)$ | $O(N^2)$ | $O(\log N)$ | No | General in-memory fast sorting |
| **Radix Sort** | $O(N \cdot L)$ | $O(N \cdot L)$ | $O(N \cdot L)$ | $O(N + K)$ | Yes | Fixed-length short sequencing reads ($K=4$) |

> **Takeaway**: Comparison sorts top out at $O(N \log N)$, with Merge Sort providing stability and predictable bounds. For fixed-length DNA reads, Radix Sort achieves linear $O(N \cdot L)$ sorting.
