---
layout: default
title: Summation & Product Notation
parent: MA101 Discrete Mathematics & Notation
nav_order: 1
---

# Summation ($\sum$) & Product ($\prod$) Notation

In bioinformatics papers, loops across genomic positions, read depths, or likelihood scores are almost always written using Greek capital letters **Sigma ($\sum$)** for sums and **Pi ($\prod$)** for products.

---

## 1. Summation Notation ($\sum$)

The summation symbol $\sum$ is simply a mathematical `for` loop that adds numbers together.

$$\sum_{i=1}^{n} x_i = x_1 + x_2 + x_3 + \dots + x_n$$

### Anatomy of a Sum
* **Index Variable ($i$)**: The loop counter (often named $i, j, k$).
* **Lower Limit ($1$)**: The starting integer value of the index.
* **Upper Limit ($n$)**: The ending integer value of the index.
* **Summand ($x_i$)**: The expression evaluated at each step.

### Python Code Equivalent
```python
# Summation: sum_{i=1}^{n} x_i
total = 0
for i in range(1, n + 1):
    total += x[i]
```

### Useful Algebraic Rules
1. **Factoring out constants**: $\sum_{i=1}^{n} c \cdot x_i = c \cdot \sum_{i=1}^{n} x_i$
2. **Splitting additions**: $\sum_{i=1}^{n} (x_i + y_i) = \sum_{i=1}^{n} x_i + \sum_{i=1}^{n} y_i$
3. **Sum of a constant**: $\sum_{i=1}^{n} c = c \cdot n$

---

## 2. Product Notation ($\prod$)

The product symbol $\prod$ is a mathematical `for` loop that multiplies terms together.

$$\prod_{i=1}^{n} x_i = x_1 \cdot x_2 \cdot x_3 \cdots x_n$$

### Biological Example: Joint Probability of a DNA Read
If sequencing errors are independent across $L$ positions, the probability of observing a specific sequence read $S = s_1 s_2 \dots s_L$ given quality scores $P(s_i)$ is:

$$P(S) = \prod_{i=1}^{L} P(s_i) = P(s_1) \cdot P(s_2) \cdots P(s_L)$$

### Python Code Equivalent
```python
# Product: prod_{i=1}^{L} P(s_i)
joint_prob = 1.0
for i in range(1, L + 1):
    joint_prob *= P[i]
```

> **Why Logarithms Turn Products into Sums**:
> Multiplying 150 small probabilities (e.g., $0.001^{150}$) results in **floating-point underflow** in Python (`0.0`).
> Taking the natural logarithm ($\ln$) converts the product into a computationally stable sum:
> $$\ln\left(\prod_{i=1}^{L} P(s_i)\right) = \sum_{i=1}^{L} \ln P(s_i)$$

---

## 3. Nested / Double Sums ($\sum\sum$)

When processing 2D matrices (such as pairwise sequence alignment scores or gene-by-sample expression matrices):

$$\sum_{i=1}^{m} \sum_{j=1}^{n} M_{i,j}$$

This represents nested loops: for each row $i$ from $1$ to $m$, sum across all columns $j$ from $1$ to $n$.

```python
grand_total = 0.0
for i in range(1, m + 1):
    for j in range(1, n + 1):
        grand_total += M[i][j]
```

> **Takeaway**: $\sum$ is a cumulative addition loop, and $\prod$ is a cumulative multiplication loop. Taking the logarithm of a product transforms it into a numerically stable sum.
