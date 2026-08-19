---
layout: default
title: Span, Basis & Dimension
parent: MA211 Linear Algebra
nav_order: 4
---

# Span, Basis & Dimension

In a single-cell experiment measuring $20,000$ genes, the data lives in a $20,000$-dimensional space. But do we really need $20,000$ independent coordinate axes to describe cellular differences?

Biological pathways are co-regulated: if a cell activates cell division, hundreds of mitotic genes turn on together.

Vector space concepts—**Linear Independence**, **Span**, and **Basis**—provide the mathematical framework for finding the true minimal number of dimensions needed to describe biological data.

---

## 1. Linear Combinations & Span

Given a set of vectors $\{\mathbf{v}_1, \mathbf{v}_2, \dots, \mathbf{v}_k\} \subset \mathbb{R}^d$, a **Linear Combination** is any vector formed by multiplying each vector by a scalar constant and adding them:

$$\mathbf{w} = c_1 \mathbf{v}_1 + c_2 \mathbf{v}_2 + \dots + c_k \mathbf{v}_k \quad (c_i \in \mathbb{R})$$

The **Span** of a set of vectors is the entire subspace of all possible linear combinations that can be reached:

$$\operatorname{Span}(\mathbf{v}_1, \dots, \mathbf{v}_k) = \{ c_1 \mathbf{v}_1 + \dots + c_k \mathbf{v}_k \mid c_i \in \mathbb{R} \}$$

* Two non-parallel vectors in 3D space span a **2D flat plane**.
* Adding a third vector that already lies on that plane adds **no new spatial directions** to the span.

---

## 2. Linear Independence

A set of vectors $\{\mathbf{v}_1, \dots, \mathbf{v}_k\}$ is **Linearly Independent** if no vector in the set can be written as a linear combination of the others.

```
Linearly Independent (2D Plane):        Linearly Dependent (Redundant):
          v2 ▲                                    v2 ▲
             │                                       │   / v3 (Redundant!)
             │                                       │  /
             └──────────► v1                         └─/────────► v1
```

### Formal Test:
The only solution to:
$$c_1 \mathbf{v}_1 + c_2 \mathbf{v}_2 + \dots + c_k \mathbf{v}_k = \mathbf{0}$$
must be the trivial solution $c_1 = c_2 = \dots = c_k = 0$.

If non-zero scalars exist that satisfy the equation, the vectors are **Linearly Dependent** (redundant).

---

## 3. Basis & Dimension

A **Basis** for a vector space $V$ is a set of vectors that satisfies two criteria:
1. They are **Linearly Independent** (no redundant vectors).
2. They **Span** $V$ (they reach every possible point in the space).

The **Dimension** ($\dim V$) is simply the number of vectors in any basis of $V$.

* The standard basis for $\mathbb{R}^3$ is $\mathbf{e}_1 = \begin{bmatrix} 1 \\ 0 \\ 0 \end{bmatrix}, \mathbf{e}_2 = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}, \mathbf{e}_3 = \begin{bmatrix} 0 \\ 0 \\ 1 \end{bmatrix}$.
* Any point in $\mathbb{R}^3$ can be uniquely represented as coordinates along these basis vectors.

> **Takeaway**: A basis is an efficient, non-redundant coordinate system. In computational biology, finding a low-dimensional basis allows compressing 20,000 gene dimensions into a few meaningful biological programs.
