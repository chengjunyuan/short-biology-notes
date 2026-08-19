---
layout: default
title: Matrices, Vectors & Basic Operations
parent: MA211 Linear Algebra
nav_order: 1
---

# Matrices, Vectors & Basic Operations

In computational biology, data is naturally organized into tables of numbers. In mathematics, these tables are called **Matrices**, and individual rows or columns are called **Vectors**.

---

## 1. Matrix Representation of Biological Data

A matrix $A \in \mathbb{R}^{m \times n}$ has $m$ rows and $n$ columns.

$$X = \begin{bmatrix} x_{1,1} & x_{1,2} & \cdots & x_{1,n} \\ x_{2,1} & x_{2,2} & \cdots & x_{2,n} \\ \vdots & \vdots & \ddots & \vdots \\ x_{m,1} & x_{m,2} & \cdots & x_{m,n} \end{bmatrix}$$

* In a **Gene Expression Matrix**:
  * Rows ($i = 1 \dots m$): Individual biological samples or single cells.
  * Columns ($j = 1 \dots n$): Specific genes (e.g., *TP53*, *EGFR*, *BRCA1*).
  * Entry $x_{i,j}$: The measured expression count of gene $j$ in sample $i$.

---

## 2. Core Matrix Operations

### A. Matrix Addition & Subtraction
Matrices of the **same dimensions** ($m \times n$) are added element-by-element:
$$(A + B)_{i,j} = A_{i,j} + B_{i,j}$$

### B. Scalar Multiplication
Multiplying a matrix by a single constant scalar $c \in \mathbb{R}$ scales every entry:
$$(c A)_{i,j} = c \cdot A_{i,j}$$
*(e.g., scaling raw sequencing counts by library size normalization factors).*

### C. Matrix Multiplication ($AB$)
Matrix multiplication is **not** element-wise. If $A$ is $m \times k$ and $B$ is $k \times n$, their product $C = AB$ is an $m \times n$ matrix where each entry is the **dot product** of row $i$ of $A$ and column $j$ of $B$:

$$C_{i,j} = \sum_{r=1}^{k} A_{i,r} B_{r,j}$$

```
Row i of A:    [ a1, a2, a3 ]  x  Column j of B: [ b1 ]   ──>  Ci,j = a1*b1 + a2*b2 + a3*b3
                                                 [ b2 ]
                                                 [ b3 ]
```

* **Important Rule**: $AB \ne BA$ (Matrix multiplication is **non-commutative**!).
* **Dimension Matching**: The inner dimensions must match: $(m \times \mathbf{k}) \times (\mathbf{k} \times n) \rightarrow (m \times n)$.

### D. The Matrix Transpose ($A^T$)
The **Transpose** flips a matrix over its diagonal, turning rows into columns and columns into rows:
$$(A^T)_{i,j} = A_{j,i}$$
* If $A$ is $m \times n$, then $A^T$ is $n \times m$.
* Useful property: $(AB)^T = B^T A^T$.

> **Takeaway**: Matrices structure high-dimensional multi-gene experiments. Matrix multiplication $AB$ applies linear transformations, projecting samples across feature spaces.
