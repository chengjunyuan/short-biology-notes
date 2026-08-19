---
layout: default
title: Eigenvalues, Eigenvectors & PCA Intuition
parent: MA211 Linear Algebra
nav_order: 5
---

# Eigenvalues, Eigenvectors & PCA Intuition

When a matrix $A$ multiplies a vector $\mathbf{x}$, it typically does two things: it **rotates** the vector and **stretches** it.

However, for any matrix $A$, there exist special vectors whose direction is **completely unchanged** by the transformation—they are only stretched or shrunk by a scalar factor.

These special vectors are called **Eigenvectors**, and the stretch factors are called **Eigenvalues**.

---

## 1. The Fundamental Eigenvalue Equation

$$A \mathbf{v} = \lambda \mathbf{v} \quad (\mathbf{v} \ne \mathbf{0})$$

* $A \in \mathbb{R}^{n \times n}$: A square matrix transformation.
* $\mathbf{v}$: An **Eigenvector** (characteristic direction).
* $\lambda \in \mathbb{R}$: The corresponding **Eigenvalue** (scaling factor).

```
Matrix Transformation A acting on generic vs. eigenvector:
Generic vector x:   x ───> Ax (Rotated AND Stretched)
Eigenvector v:      v ───> Av = λv (Pure Stretch along same axis!)
```

---

## 2. Calculating Eigenvalues & Eigenvectors

To find $\lambda$ and $\mathbf{v}$:

1. Rewrite the equation:
   $$A\mathbf{v} - \lambda \mathbf{v} = \mathbf{0} \implies (A - \lambda I)\mathbf{v} = \mathbf{0}$$
2. For a non-zero vector $\mathbf{v}$ to exist, the matrix $(A - \lambda I)$ must be singular (non-invertible):
   $$\det(A - \lambda I) = 0 \quad \text{(The Characteristic Equation)}$$
3. Solve the characteristic polynomial for roots $\lambda_1, \lambda_2, \dots, \lambda_n$.
4. For each $\lambda_i$, substitute back into $(A - \lambda_i I)\mathbf{v} = \mathbf{0}$ and solve for the eigenvector $\mathbf{v}_i$ using Gaussian elimination.

---

## 3. Biological Application: Principal Component Analysis (PCA)

In single-cell genomics, we start with a high-dimensional expression matrix ($50,000\text{ cells} \times 20,000\text{ genes}$). How do we project this into a 2D scatter plot while preserving the most biologically meaningful variation?

```
High-Dimensional Gene Data Cloud:       PCA Eigenvector Projections:
           Gene 2 ▲                                 PC2 ▲ (Second Eigenvector λ2)
                  │   /                             │
                  │  / (Main axis of variance: PC1) │   * * *
                  │ /                               │ * * * * *
                  └────────► Gene 1                 └────────► PC1 (Top Eigenvector λ1)
```

### The Linear Algebra Engine of PCA:
1. Compute the **Covariance Matrix** $C = \frac{1}{N} X^T X$ (which measures how gene expression correlates across cells).
2. Calculate the eigenvalues and eigenvectors of $C$:
   * **Eigenvector with largest $\lambda_1$ ($\text{PC1}$)**: The exact line through $20,000$-dimensional space that captures the **maximum possible biological variance**.
   * **Eigenvector with second largest $\lambda_2$ ($\text{PC2}$)**: The orthogonal direction capturing the next highest variance.
3. Project each cell's $20,000$-gene profile onto $\text{PC1}$ and $\text{PC2}$ to generate the standard 2D PCA plots used to identify cell types (T-cells, B-cells, neurons).

> **Takeaway**: Eigenvectors identify the invariant principal axes of linear transformations. In bioinformatics, eigendecomposition of covariance matrices (PCA) compresses massive gene expression matrices into informative low-dimensional visual landscapes.
