---
layout: default
title: Multivariable Optimization & The Hessian
parent: MA201 Calculus & Optimization
nav_order: 3
---

# Multivariable Optimization & The Hessian

In single-variable calculus, we find critical points where $f'(x) = 0$ and check the sign of $f''(x)$.

In multivariable optimization ($f: \mathbb{R}^d \rightarrow \mathbb{R}$), critical points occur where the gradient vector is zero ($\nabla f = \mathbf{0}$). To classify whether that point is a **local minimum**, a **local maximum**, or a **saddle point**, we analyze the matrix of second partial derivatives: the **Hessian Matrix**.

---

## 1. Finding Critical Points in Higher Dimensions

A point $\mathbf{x}^* = (x_1^*, \dots, x_d^*)$ is a critical point if and only if all partial derivatives are simultaneously zero:

$$\nabla f(\mathbf{x}^*) = \mathbf{0} \iff \begin{cases} \frac{\partial f}{\partial x_1} = 0 \\ \frac{\partial f}{\partial x_2} = 0 \\ \vdots \\ \frac{\partial f}{\partial x_d} = 0 \end{cases}$$

---

## 2. The Hessian Matrix ($H$)

The **Hessian Matrix** is the $d \times d$ square matrix of all second-order partial derivatives:

$$H = \nabla^2 f(\mathbf{x}) = \begin{bmatrix} \frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_1 \partial x_d} \\ \frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} & \cdots & \frac{\partial^2 f}{\partial x_2 \partial x_d} \\ \vdots & \vdots & \ddots & \vdots \\ \frac{\partial^2 f}{\partial x_d \partial x_1} & \frac{\partial^2 f}{\partial x_d \partial x_2} & \cdots & \frac{\partial^2 f}{\partial x_d^2} \end{bmatrix}$$

*(By Schwarz’s Theorem, if second derivatives are continuous, mixed partials are equal: $\frac{\partial^2 f}{\partial x_i \partial x_j} = \frac{\partial^2 f}{\partial x_j \partial x_i}$. Thus, the Hessian is always a **symmetric matrix**!)*

---

## 3. Second Derivative Test: Positive Definiteness (PSD / PD)

How do we determine if the multivariable curvature curves upwards in every direction? We evaluate whether the Hessian matrix is **Positive Definite**:

```
Local Minimum (Bowl):          Local Maximum (Dome):         Saddle Point (Pringles Chip):
Hessian is Positive Definite    Hessian is Negative Definite   Hessian has Mixed Eigenvalues
      (All λ > 0)                    (All λ < 0)               (Some λ > 0, Some λ < 0)
```

### Definitions & Classification:
1. **Positive Definite ($H \succ 0$)**:
   * For all non-zero vectors $\mathbf{z}$, $\mathbf{z}^T H \mathbf{z} > 0$ (all eigenvalues $\lambda_i > 0$).
   * **Conclusion**: $\mathbf{x}^*$ is a strict **Local Minimum**.
2. **Positive Semi-Definite ($H \succeq 0$)**:
   * For all $\mathbf{z}$, $\mathbf{z}^T H \mathbf{z} \ge 0$ (all eigenvalues $\lambda_i \ge 0$).
   * **Conclusion**: $\mathbf{x}^*$ is a **Local Minimum** (or flat valley).
3. **Negative Definite ($H \prec 0$)**:
   * All eigenvalues $\lambda_i < 0$.
   * **Conclusion**: $\mathbf{x}^*$ is a strict **Local Maximum**.
4. **Indefinite (Mixed Signs)**:
   * Some eigenvalues $\lambda_i > 0$ and some $\lambda_j < 0$.
   * **Conclusion**: $\mathbf{x}^*$ is a **Saddle Point** (minimum along one trajectory, maximum along another).

### Quick 2D Determinant Criterion ($d = 2$):
For $H = \begin{bmatrix} f_{xx} & f_{xy} \\ f_{xy} & f_{yy} \end{bmatrix}$:
* $\det(H) = f_{xx} f_{yy} - (f_{xy})^2$.
* If $\det(H) > 0$ and $f_{xx} > 0 \implies$ **Local Minimum**.
* If $\det(H) > 0$ and $f_{xx} < 0 \implies$ **Local Maximum**.
* If $\det(H) < 0 \implies$ **Saddle Point**.

---

## 4. Iterative Optimization: Gradient Descent

When analytical equations cannot be solved by hand (e.g. fitting non-linear biological neural networks), we use iterative numerical algorithms like **Gradient Descent**:

$$\mathbf{x}_{t+1} = \mathbf{x}_t - \eta \nabla f(\mathbf{x}_t)$$

Where $\eta > 0$ is the **learning rate** (step size). By taking steps in the negative gradient direction ($-\nabla f$), the algorithm descends the surface until it converges to a local minimum ($\nabla f \approx \mathbf{0}$).

> **Takeaway**: Multivariable critical points have $\nabla f = \mathbf{0}$. If the Hessian matrix $H$ at that point is Positive Definite (all eigenvalues positive), the point is guaranteed to be a local minimum.
