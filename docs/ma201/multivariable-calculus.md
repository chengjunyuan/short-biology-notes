---
layout: default
title: Multivariable Calculus & The Gradient Vector
parent: MA201 Calculus & Optimization
nav_order: 2
---

# Multivariable Calculus & The Gradient Vector

Most real biological models involve multiple variables: predicting protein stability from temperature, pH, and salt concentration ($f(T, \text{pH}, S)$), or fitting machine learning loss functions with thousands of parameter weights $\mathbf{w} \in \mathbb{R}^d$.

Multivariable calculus extends differentiation to functions of multiple inputs.

---

## 1. Partial Derivatives ($\frac{\partial f}{\partial x_i}$)

When differentiating a function $f(x, y)$ with respect to one variable (say $x$), we treat all other variables ($y$) as constant numbers:

### Example:
Let $f(x, y) = 3x^2 y + 2x + y^3$.

* **Partial derivative with respect to $x$**:
  $$\frac{\partial f}{\partial x} = \frac{\partial}{\partial x}(3x^2 y + 2x + y^3) = 6xy + 2$$
  *(Notice $y^3$ becomes $0$ because $y$ is treated as a constant!)*
* **Partial derivative with respect to $y$**:
  $$\frac{\partial f}{\partial y} = \frac{\partial}{\partial y}(3x^2 y + 2x + y^3) = 3x^2 + 3y^2$$

---

## 2. The Gradient Vector ($\nabla f$)

For a function $f(x_1, x_2, \dots, x_d)$, the **Gradient** (denoted $\nabla f$ or "del $f$") is the vector of all its first-order partial derivatives:

$$\nabla f(\mathbf{x}) = \begin{bmatrix} \frac{\partial f}{\partial x_1} \\ \frac{\partial f}{\partial x_2} \\ \vdots \\ \frac{\partial f}{\partial x_d} \end{bmatrix}$$

```
                ▲ ∇f (Steepest Ascent)
               /
      Contour / (Level Curves)
     ────────/────────
            /
           /  -∇f (Steepest Descent)
          ▼
```

### The Fundamental Geometric Properties of the Gradient:
1. **Direction of Maximum Increase**: $\nabla f(\mathbf{x})$ points in the direction of the steepest ascent on the multi-dimensional surface.
2. **Magnitude is the Steepest Rate**: $\|\nabla f(\mathbf{x})\|$ is the instantaneous rate of increase in that direction.
3. **Orthogonal to Contour Lines**: The gradient vector is always perpendicular to the level curves (contours of equal value) of the function.

---

## 3. Directional Derivatives

If you want to know the rate of change of $f(\mathbf{x})$ in any arbitrary direction specified by a unit vector $\mathbf{u}$ ($\|\mathbf{u}\| = 1$), the **Directional Derivative** $D_{\mathbf{u}} f$ is simply the dot product with the gradient:

$$D_{\mathbf{u}} f(\mathbf{x}) = \nabla f(\mathbf{x}) \cdot \mathbf{u}$$

> **Takeaway**: Partial derivatives isolate the slope along one coordinate axis. The gradient vector $\nabla f$ combines all partial derivatives to point in the direction of steepest increase.
