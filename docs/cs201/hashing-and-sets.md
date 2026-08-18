---
layout: default
title: Hashing, Sets & Bloom Filters
parent: CS201 Data Structures & Algorithms I
nav_order: 5
---

# Hashing, Sets & Bloom Filters

When searching a database, $O(\log N)$ binary search is fast, but **$O(1)$ constant time lookup** is the gold standard.

Hash tables and probabilistic data structures allow instant indexing of biological entities—from looking up codon translation tables to tracking hundreds of millions of $k$-mers.

---

## 1. Hash Tables & Hash Functions

A **Hash Table** maps a key (e.g., a gene name `"BRCA1"` or $k$-mer `"ACGTGCTA"`) to an array index using a mathematical **Hash Function** $h(k)$.

```
Key ("ATGC") ──> [ Hash Function h(k) ] ──> Index 4 ──> Array[4] = GeneAnnotationObject
```

### Properties of a Good Hash Function
1. **Deterministic**: Same input key must always yield the exact same integer hash.
2. **Uniform Distribution**: Keys are distributed evenly across table buckets to minimize collisions.
3. **Fast Computation**: Calculates in $O(L)$ time for key of length $L$.

---

## 2. Collision Resolution Strategies

Because the space of possible DNA sequences ($4^K$) is vast compared to table size $M$, two distinct keys will inevitably produce the same hash index ($h(k_1) = h(k_2)$).

```
1. Separate Chaining:           2. Open Addressing (Linear Probing):
   Bucket [0] ──> null             Bucket [0] -> [ Key A ]
   Bucket [1] ──> [ A ] -> [ B ]   Bucket [1] -> [ Key B (collided from 0) ]
   Bucket [2] ──> [ C ]            Bucket [2] -> [ Key C ]
```

* **Separate Chaining**: Each bucket contains a linked list or small dynamic array of collided records.
* **Open Addressing (Linear/Quadratic Probing)**: If bucket $h(k)$ is occupied, probe subsequent slots in the table until an empty slot is found.

### Load Factor ($\alpha$) & Dynamic Resizing
$$\alpha = \frac{N \text{ (number of stored items)}}{M \text{ (number of buckets)}}$$
* When $\alpha > 0.75$, lookup performance degrades due to long collision chains.
* **Dynamic Resizing**: The table allocates a new underlying array of double capacity ($2M$) and rehashes all elements.
* **Amortized Analysis**: While a resize takes $O(N)$ time, it happens infrequently enough that the **amortized cost per insertion remains $O(1)$**.

---

## 3. Bloom Filters: Space-Efficient Probabilistic Sets

In high-throughput sequencing experiments, you might need to test whether billions of incoming short reads contain $k$-mers already seen in a reference genome dataset. Storing all $k$-mers in a standard hash table could require 64 GB of RAM.

A **Bloom Filter** is a space-efficient probabilistic data structure that tests set membership using a tiny fraction of that memory.

```
Incoming k-mer ──> [ h1 ] ──> Bit 3  ──┐
               ──> [ h2 ] ──> Bit 7  ──┼──> Check Bit Array: [ 0, 0, 0, 1, 0, 0, 0, 1, 0 ]
               ──> [ h3 ] ──> Bit 1  ──┘                       All bits 1? -> "Probably Yes"
                                                               Any bit 0?  -> "Definitely No"
```

### The Bloom Filter Guarantee
* **False Negatives are IMPOSSIBLE**: If the Bloom filter returns `False`, the sequence is **100% guaranteed** not to be in the set.
* **False Positives are POSSIBLE**: If all hashed bits are `1`, it is possible that other elements set those bits coincidentally. (The false positive rate $\epsilon$ can be tuned mathematically by adjusting array bit size $M$ and number of hash functions $K$).

### Biological Impact
Aligners and error-correction tools (like `BFC`, `Lighter`, `SGA`) use Bloom filters to rapidly discard novel sequencing errors or filter non-target reads directly in memory before expensive disk writes.

> **Takeaway**: Hash tables provide expected $O(1)$ lookup via amortized resizing and collision management. For multi-gigabyte sequencing datasets, Bloom filters offer near-zero-memory membership checks with zero false negatives.
