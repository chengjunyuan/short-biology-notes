---
layout: default
title: Sets, Number Systems & Set-Builder Notation
parent: MA101 Discrete Mathematics & Notation
nav_order: 2
---

# Sets, Number Systems & Set-Builder Notation

A **Set** is an unordered collection of distinct objects. Set notation provides a concise, unambiguous way to describe filtered cohorts of genes, coordinate ranges, and mathematical domains.

---

## 1. Standard Number Sets

In mathematical descriptions, variable types are specified using blackboard bold letters:

* **$\mathbb{N}$ (Natural Numbers)**: Non-negative integers $\{0, 1, 2, 3, \dots\}$ (or sometimes starting at $1$).
* **$\mathbb{Z}$ (Integers)**: Whole numbers $\{\dots, -2, -1, 0, 1, 2, \dots\}$ (from German *Zahlen*).
* **$\mathbb{Q}$ (Rational Numbers)**: Any fraction $\frac{a}{b}$ where $a, b \in \mathbb{Z}$ and $b \ne 0$ (from *Quotient*).
* **$\mathbb{R}$ (Real Numbers)**: All continuous numbers along the real number line, including irrationals like $\pi, e, \sqrt{2}$.
* **$\mathbb{R}^d$ ($d$-Dimensional Real Space)**: Vectors of $d$ continuous real numbers (e.g., an expression profile of $d$ genes $\mathbf{x} \in \mathbb{R}^d$).

---

## 2. Set Membership & Set-Builder Notation

### Basic Membership Symbols
* $x \in S$: "$x$ is an element of set $S$".
* $x \notin S$: "$x$ is not an element of set $S$".
* $\emptyset$ or $\{\}$: The **Empty Set** (contains zero elements).
* $A \subseteq B$: "$A$ is a subset of $B$" (every element in $A$ also belongs to $B$).

### Set-Builder Notation
Set-builder notation defines a set by specifying a domain and a filtering condition:

$$\{ \text{variable} \in \text{Domain} \mid \text{Condition} \}$$

*(The vertical bar $\mid$ or colon $:$ reads as "such that".)*

#### Examples:
1. **Mathematical**:
   $$S = \{ x \in \mathbb{R} \mid x > 0 \}$$
   *(The set of all positive real numbers).*
2. **Biological / Computational**:
   $$G_{\text{sig}} = \{ g \in \text{Genes} \mid p\text{-val}(g) < 0.05 \text{ and } |\log_2\text{FC}(g)| \ge 1.5 \}$$
   *(The set of significantly differentially expressed genes).*

---

## 3. Set Operations

```
Union (A ∪ B):             Intersection (A ∩ B):      Set Difference (A \ B):
  [  ( A   *   B )  ]        [    (   *   )    ]        [  ( *     )   B  ]
 (Everything in A or B)     (Elements in BOTH A & B)     (Elements in A but NOT B)
```

| Operation | Symbol | Meaning | Python Equivalent |
| :--- | :--- | :--- | :--- |
| **Union** | $A \cup B$ | Elements in $A$, $B$, or both | `A \| B` |
| **Intersection** | $A \cap B$ | Elements in both $A$ and $B$ | `A & B` |
| **Set Difference (Setminus)** | $A \setminus B$ | Elements in $A$ that are not in $B$ | `A - B` |
| **Complement** | $A^c$ or $\bar{A}$ | Elements in universe $U$ not in $A$ ($U \setminus A$) | `U - A` |

### Indexed Big Unions ($\bigcup$) and Big Intersections ($\bigcap$)
Just as $\sum$ aggregates additions, $\bigcup$ and $\bigcap$ aggregate set operations across multiple sets:

* **Big Union**: $\bigcup_{i=1}^{k} S_i = S_1 \cup S_2 \cup \dots \cup S_k$ (the set of all elements present in *at least one* sample).
* **Big Intersection**: $\bigcap_{i=1}^{k} S_i = S_1 \cap S_2 \cap \dots \cap S_k$ (the core set of elements shared across *all* $k$ samples).

> **Takeaway**: Set-builder notation is the mathematical equivalent of a Python list/set comprehension (`{x for x in domain if condition}`).
