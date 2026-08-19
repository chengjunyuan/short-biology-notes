---
layout: default
title: Systems of Linear Equations & Row Reduction
parent: MA211 Linear Algebra
nav_order: 2
---

# Systems of Linear Equations & Row Reduction

Many computational biology questions can be formulated as a system of simultaneous linear constraints—such as balancing chemical reaction flux in metabolic pathways (Flux Balance Analysis) or fitting linear regression models.

---

## 1. Systems in Matrix Form: $A\mathbf{x} = \mathbf{b}$

Consider a system of linear equations:
$$\begin{aligned} 2x_1 + 3x_2 - x_3 &= 8 \\ 4x_1 + x_2 + 2x_3 &= 7 \\ -2x_1 + 2x_2 + x_3 &= 3 \end{aligned}$$

We can write this compactly in matrix-vector notation:

$$\underbrace{\begin{bmatrix} 2 & 3 & -1 \\ 4 & 1 & 2 \\ -2 & 2 & 1 \end{bmatrix}}_{A \text{ (Coefficient Matrix)}} \underbrace{\begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix}}_{\mathbf{x} \text{ (Unknowns)}} = \underbrace{\begin{bmatrix} 8 \\ 7 \\ 3 \end{bmatrix}}_{\mathbf{b} \text{ (Output Target)}}$$

---

## 2. Augmented Matrices & Elementary Row Operations

To solve for $\mathbf{x}$, we construct an **Augmented Matrix** $[A \mid \mathbf{b}]$:

$$[A \mid \mathbf{b}] = \left[\begin{array}{ccc|c} 2 & 3 & -1 & 8 \\ 4 & 1 & 2 & 7 \\ -2 & 2 & 1 & 3 \end{array}\right]$$

We perform three allowable **Elementary Row Operations** (which do not change the solution set):
1. **Row Swap**: Swap the position of two rows ($R_i \leftrightarrow R_j$).
2. **Scalar Scaling**: Multiply a row by a non-zero constant ($R_i \leftarrow c R_i$).
3. **Row Addition**: Add a multiple of one row to another ($R_j \leftarrow R_j + c R_i$).

---

## 3. Gaussian Elimination & Row Echelon Form (REF)

**Gaussian Elimination** is the systematic algorithmic procedure for using row operations to eliminate lower entries, creating an upper-triangular staircase (**Row Echelon Form**):

```
Original Augmented Matrix:          Row Echelon Form (REF):
   [ a11  a12  a13 | b1 ]              [ 1   *   *  | * ]
   [ a21  a22  a23 | b2 ]   ───>       [ 0   1   *  | * ]  ──> Back-Substitution solves x3, then x2, then x1!
   [ a31  a32  a33 | b3 ]              [ 0   0   1  | * ]
```

### The Three Possible Solution Outcomes:
1. **Unique Solution**: Every column has a leading pivot (e.g., identity matrix on the left).
2. **Infinitely Many Solutions**: One or more columns lack a leading pivot (free variables exist).
3. **No Solution (Inconsistent)**: Row reduction produces a contradictory row like $\left[\begin{array}{ccc|c} 0 & 0 & 0 & 5 \end{array}\right]$ (meaning $0 = 5$).

> **Takeaway**: Any linear system $A\mathbf{x} = \mathbf{b}$ can be solved algorithmically via Gaussian elimination by converting the augmented matrix into Row Echelon Form.
