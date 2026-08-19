---
layout: default
title: Single-Variable Differentiation & Extremum Points
parent: MA201 Calculus & Optimization
nav_order: 1
---

# Single-Variable Differentiation & Extremum Points

The derivative $\frac{df}{dx}$ or $f'(x)$ measures the **instantaneous rate of change** of a function $f(x)$ with respect to $x$.

---

## 1. Core Differentiation Rules

In practice, you do not calculate derivatives using limit definitions; you apply standard differentiation rules:

| Rule Name | Function $f(x)$ | Derivative $f'(x)$ | Example |
| :--- | :--- | :--- | :--- |
| **Power Rule** | $x^n$ | $n x^{n-1}$ | $\frac{d}{dx}(x^3) = 3x^2$ |
| **Exponential** | $e^{ax}$ | $a e^{ax}$ | $\frac{d}{dx}(e^{-2x}) = -2e^{-2x}$ |
| **Natural Logarithm** | $\ln(x)$ | $\frac{1}{x}$ | $\frac{d}{dx}(\ln x) = \frac{1}{x}$ |
| **Sum / Difference** | $u(x) \pm v(x)$ | $u'(x) \pm v'(x)$ | $\frac{d}{dx}(x^2 + 5x) = 2x + 5$ |
| **Product Rule** | $u(x) \cdot v(x)$ | $u'(x)v(x) + u(x)v'(x)$ | $\frac{d}{dx}(x e^x) = e^x + x e^x$ |
| **Quotient Rule** | $\frac{u(x)}{v(x)}$ | $\frac{u'(x)v(x) - u(x)v'(x)}{[v(x)]^2}$ | $\frac{d}{dx}\left(\frac{x}{1+x}\right) = \frac{1}{(1+x)^2}$ |
| **Chain Rule (Composite)** | $g(h(x))$ | $g'(h(x)) \cdot h'(x)$ | $\frac{d}{dx}(\ln(x^2 + 1)) = \frac{2x}{x^2 + 1}$ |

---

## 2. Solving for Local Maxima and Minima

At the peak of a hill (local maximum) or the bottom of a valley (local minimum), the slope of the tangent line is completely flat:

$$f'(x) = 0$$

```
Local Maximum: f'(x) = 0, f''(x) < 0 (Concave Down: ∩)
          ▲
        /   \
       /     \
───────────────────────────
       \     /
        \   /
          ▼
Local Minimum: f'(x) = 0, f''(x) > 0 (Concave Up: ∪)
```

### The Optimization Algorithm:
1. **Find Critical Points**: Take the first derivative $f'(x)$, set $f'(x) = 0$, and solve for $x^*$.
2. **Second Derivative Test ($f''(x)$)**:
   * If $f''(x^*) > 0 \implies x^*$ is a **Local Minimum** (curving upwards).
   * If $f''(x^*) < 0 \implies x^*$ is a **Local Maximum** (curving downwards).
   * If $f''(x^*) = 0 \implies$ Inconclusive (inflection point or higher-order test needed).

---

## 3. Worked Example: Maximum Likelihood Estimation (MLE) of a Coin/Mutation Probability

Suppose you sequence $N$ independent DNA fragments and observe $k$ mutated reads and $N - k$ wildtype reads. The probability (likelihood) of observing this data as a function of the unknown mutation rate $p \in (0, 1)$ is:

$$L(p) = p^k (1 - p)^{N - k}$$

To find the value of $p$ that **maximizes** the likelihood:

1. **Take the log-likelihood** (logarithms simplify products to sums without changing the maximum location):
   $$\ell(p) = \ln L(p) = k \ln(p) + (N - k) \ln(1 - p)$$
2. **Differentiate with respect to $p$ and set to zero**:
   $$\frac{d\ell}{dp} = \frac{k}{p} - \frac{N - k}{1 - p} = 0$$
3. **Solve for $p^*$**:
   $$\frac{k}{p} = \frac{N - k}{1 - p} \implies k(1 - p) = p(N - k) \implies k = pN \implies p^* = \frac{k}{N}$$
4. **Second derivative test**:
   $$\frac{d^2\ell}{dp^2} = -\frac{k}{p^2} - \frac{N - k}{(1 - p)^2} < 0 \quad \text{(Guaranteed strictly negative } \implies \text{Global Maximum!)}$$

The optimal MLE estimate is simply the observed mutation proportion $\frac{k}{N}$.

> **Takeaway**: Critical points occur where the first derivative is zero ($f'(x) = 0$). The sign of the second derivative confirms whether the point is a maximum or minimum.
