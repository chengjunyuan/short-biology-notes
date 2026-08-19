---
layout: default
title: Determinants & Matrix Inverses
parent: MA211 Linear Algebra
nav_order: 3
---

# Determinants & Matrix Inverses

For numbers, the multiplicative inverse of $5$ is $5^{-1} = \frac{1}{5}$, allowing us to solve $5x = 20 \implies x = 5^{-1}(20) = 4$.

How do we invert a matrix $A$ to solve $A\mathbf{x} = \mathbf{b}$? And how do we determine whether an inverse even exists?

---

## 1. The Matrix Inverse ($A^{-1}$)

For a square matrix $A \in \mathbb{R}^{n \times n}$, its **Inverse** $A^{-1}$ satisfies:

$$A A^{-1} = A^{-1} A = I_n$$

Where $I_n$ is the $n \times n$ **Identity Matrix** (ones along the main diagonal, zeros everywhere else).

If $A^{-1}$ exists, solving $A\mathbf{x} = \mathbf{b}$ is immediate:
$$A^{-1}(A\mathbf{x}) = A^{-1}\mathbf{b} \implies I\mathbf{x} = A^{-1}\mathbf{b} \implies \mathbf{x} = A^{-1}\mathbf{b}$$

A matrix with an inverse is called **Invertible** (or **Non-Singular**). If no inverse exists, it is called **Singular**.

---

## 2. The Determinant ($\det(A)$ or $|A|$)

The **Determinant** is a scalar number that measures the geometric scaling factor of the linear transformation described by matrix $A$.

```
Geometric Interpretation of Determinant in 2D:
   y ▲                     y ▲
     │  [ 1, 0 ]             │        Transform by A
     │  [ 0, 1 ]             │      ─────────────────>    Area = |det(A)|
     │  Area = 1.0           │
     └──────────► x          └──────────► x
```

* **Area / Volume Scaling**: $\det(A)$ is the factor by which areas (in 2D) or volumes (in 3D) expand or shrink under transformation $A$.
* **Invertibility Criterion**:
  $$\text{Matrix } A \text{ is invertible if and only if } \det(A) \ne 0$$
  *(If $\det(A) = 0$, the transformation collapses space into a flat line or plane—destroying information and making inversion impossible).*

---

## 3. Calculating Determinants

### A. $2 \times 2$ Matrix Formula:
For $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$:

$$\det(A) = ad - bc$$

The inverse is:
$$A^{-1} = \frac{1}{ad - bc} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

### B. $3 \times 3$ Matrix (Cofactor Expansion):
For $A = \begin{bmatrix} a & b & c \\ d & e & f \\ g & h & i \end{bmatrix}$:

$$\det(A) = a \begin{vmatrix} e & f \\ h & i \end{vmatrix} - b \begin{vmatrix} d & f \\ g & i \end{vmatrix} + c \begin{vmatrix} d & e \\ g & h \end{vmatrix}$$

$$= a(ei - fh) - b(di - fg) + c(dh - eg)$$

---

## 4. Key Properties of Determinants

1. **Multiplication**: $\det(AB) = \det(A) \cdot \det(B)$
2. **Inverse**: $\det(A^{-1}) = \frac{1}{\det(A)}$
3. **Transpose**: $\det(A^T) = \det(A)$
4. **Triangular Matrices**: The determinant of an upper or lower triangular matrix is simply the product of its diagonal entries!

> **Takeaway**: $\det(A) \ne 0$ guarantees that a linear system has a unique solution and that $A^{-1}$ exists.
