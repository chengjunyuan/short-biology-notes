---
layout: default
title: Collections & Data Representation
parent: CS101 Programming Fundamentals
nav_order: 2
---

# Collections & Data Representation in Python

Choosing the right collection type determines whether your biological script completes in 2 seconds or 2 hours, and whether it uses 100 MB or 10 GB of RAM.

---

## 1. The Core Collection Types

### A. Lists (`list`) — Ordered, Mutable Sequences
* **Characteristics**: Preserves insertion order, allows duplicates, elements can be modified in place.
* **When to use**: Storing ordered series of biological records (e.g., list of sequencing reads in the order they were sequenced).
* **Limitation**: Checking if an element exists in a list (`if x in my_list:`) takes $O(N)$ linear time. If `my_list` contains 1,000,000 items, Python must check every single item one by one.

### B. Tuples (`tuple`) — Ordered, Immutable Records
* **Characteristics**: Fixed length and content once created. Cannot append or mutate.
* **Why use over Lists?**:
  * Lower memory footprint than lists.
  * Guarantees that genomic coordinates (e.g., `("chr1", 100050, 100200)`) cannot be accidentally modified downstream.
  * Can be used as dictionary keys because they are immutable and hashable.

### C. Dictionaries (`dict`) — Key-Value Hash Maps
* **Characteristics**: Fast lookups ($O(1)$ constant time on average) mapping unique keys to values.
* **When to use**:
  * Genetic codon tables (`{"AUG": "Met", "UGG": "Trp", ...}`).
  * Gene expression matrices mapping Gene ID $\rightarrow$ Expression level (`{"TP53": 12.4, "EGFR": 3.1}`).
  * Frequency tables of k-mers.

### D. Sets (`set`) — Unordered Collections of Unique Elements
* **Characteristics**: Contains only distinct items. Supports mathematical set operations (union, intersection, difference). Lookup is $O(1)$.
* **When to use**:
  * Identifying the set of unique k-mers present in a sequencing sample.
  * Finding the intersection of differentially expressed genes between two experimental treatments:
    ```python
    treatment_a_genes = {"BRCA1", "TP53", "MYC", "EGFR"}
    treatment_b_genes = {"TP53", "KRAS", "EGFR", "CDK4"}
    
    shared_genes = treatment_a_genes & treatment_b_genes
    # Result: {'TP53', 'EGFR'} in O(min(len(A), len(B))) time!
    ```

---

## 2. Mutability vs. Immutability & Memory References

A common source of unexpected bugs in Python is passing mutable objects (like lists and dictionaries) between functions:

```python
def process_genes(gene_list: list[str]):
    gene_list.append("CONTROL_GENE")  # Mutates the original list in place!

original_genes = ["TP53", "BRCA1"]
process_genes(original_genes)
print(original_genes)  # ['TP53', 'BRCA1', 'CONTROL_GENE']
```

In Python, variables do not hold the actual data directly; they hold **references** (pointers in memory) to the underlying data object. If you pass a list to a function, any modifications mutate the original data. If immutability is desired, use `tuple` or create an explicit copy.

---

## 3. Decision Matrix: Which Collection Should I Use?

```
Do you need key-value associations?
├── Yes ──> Use dict (e.g., Codon -> Amino Acid)
└── No
    ├── Do you only care about uniqueness and fast membership checks?
    │   └── Yes ──> Use set (e.g., Filtered Gene IDs, Unique K-mers)
    └── Do you need an ordered sequence?
        ├── Needs to be modified / appended over time? ──> Use list (e.g., parsed records)
        └── Fixed record that shouldn't change? ─────────> Use tuple (e.g., (chrom, start, end))
```
