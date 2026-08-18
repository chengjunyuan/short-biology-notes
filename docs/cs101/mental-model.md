---
layout: default
title: Mental Model of Programming
parent: CS101 Programming Fundamentals
nav_order: 1
---

# Mental Model of Programming

At its core, computer programming is not about memorizing complex syntax; it is about establishing a rigorous mental model of **how data transforms from input to output**.

---

## 1. The Fundamental Pipeline: Input $\rightarrow$ Transformation $\rightarrow$ Output

Every biological analysis program—from a 5-line script to an enterprise sequence aligner—follows this uniform pattern:

```
[ Raw Input ]  ->  [ State & In-Memory Transformations ]  ->  [ Structured Output ]
  (FASTA / TSV)       (Filter reads, calculate GC, count)       (Cleaned file / summary)
```

1. **Input**: Data entering the system (file on disk, user argument, streaming input).
2. **State & Transformations**: Mathematical operations, condition checks, and data reshaping in memory.
3. **Output**: Persisting results back to disk, generating a figure, or sending a stream to another program.

If an unexpected output occurs or a script errors, debugging simply means inspecting where along this transformation pipeline the data state diverged from your mental expectation.

---

## 2. Mapping Biological Concepts to Core Data Types

Python provides built-in primitive types. Choosing the correct type ensures data integrity and prevents silent calculation bugs:

| Biological Entity | Python Type | Example | Rationale |
| :--- | :--- | :--- | :--- |
| **Nucleotide / Peptide Sequence** | `str` | `"ATGCGATCG"` | Text string representing discrete character sequences. Immutable. |
| **Read Count / Read Length** | `int` | `150`, `45281` | Discrete, exact integer counts. |
| **Quality Score (Phred) / $p$-value** | `float` | `0.001`, `38.5` | Continuous numerical values with floating-point precision. |
| **Read Filtering Flag / Match Status** | `bool` | `True`, `False` | Binary state (passed quality threshold or not). |
| **Missing Annotation / Untranslated Region** | `None` | `None` | Represents explicit absence of value (distinct from `0` or `""`). |

---

## 3. Strict Typing vs. Duck Typing in Scientific Code

Python is dynamically typed: a variable holding a string can later hold an integer. However, in scientific workflows, unexpected implicit type conversions are a leading cause of silent bugs (e.g., treating a gene name `"00123"` as integer `123`, dropping leading zeros).

```python
# Clear type annotations clarify intent for human and static analysis tools
def calculate_gc_content(sequence: str) -> float:
    g_count = sequence.count("G")
    c_count = sequence.count("C")
    return (g_count + c_count) / len(sequence)
```

> **Takeaway**: Always think of code as a directional pipeline transforming input data into desired output states using explicit, well-defined data types.
