---
layout: default
title: MA211 Linear Algebra
nav_order: 8
has_children: true
---

# MA211: Linear Algebra

Welcome to **MA211: Linear Algebra**!

Biological datasets are multidimensional. A single-cell RNA-sequencing experiment measures the expression of $20,000$ genes across $50,000$ individual cells—forming a $50,000 \times 20,000$ matrix.

Linear Algebra provides the mathematical foundation and toolset for organizing, transforming, reducing, and extracting patterns from high-dimensional biological data.

---

## Course Modules

1. [**Matrices, Vectors & Basic Operations**](matrices-and-operations.html)
   * *Rationale*: How matrices represent multi-sample biological experiments, matrix multiplication ($AB$), and transpose properties.
2. [**Systems of Linear Equations & Row Reduction**](linear-systems-and-row-reduction.html)
   * *Rationale*: Solving $A\mathbf{x} = \mathbf{b}$ using augmented matrices and Gaussian Elimination / Row Echelon Form.
3. [**Determinants & Matrix Inverses**](determinants-and-inverses.html)
   * *Rationale*: Geometric interpretation of determinants ($\det(A)$), invertibility conditions, and computing $A^{-1}$.
4. [**Linear Combinations, Span, Basis & Dimension**](span-basis-and-dimension.html)
   * *Rationale*: Vector spaces, linear independence, and finding minimal coordinate systems (bases) for biological variation.
5. [**Eigenvalues, Eigenvectors & PCA Intuition**](eigenvalues-and-eigenvectors.html)
   * *Rationale*: Finding the principal axes of variation in matrices ($A\mathbf{v} = \lambda \mathbf{v}$) and how this powers Principal Component Analysis (PCA) for clustering single cells.
