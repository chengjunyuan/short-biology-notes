---
title: Home
layout: home
nav_order: 1
---

# Computational Biology Bridging Notes

Welcome! This repository contains a curated set of bridging notes designed to transition students and researchers from a **Life Sciences** background into **Computational Biology and Bioinformatics**.

Traditional computer science and mathematics curricula can often feel abstract or disconnected from biological problems. These notes emphasize **the rationale behind computational decisions**—why we choose specific hardware, environments, algorithms, mathematical tools, and engineering practices when processing large-scale genomic, transcriptomic, and structural data.

---

## Curriculum Structure

### Foundational Modules
* [**CS100: Computer Fundamentals**](docs/cs100/) — Hardware architectures, memory constraints, OS differences (POSIX/Linux vs. Windows), remote server navigation (SSH, tmux), and HPC workflows.
* [**CS101: Programming Fundamentals**](docs/cs101/) — Mental models for program execution, Python data collections, robust biological file parsing (FASTA/TSV), and modern environment management (`uv`, `conda`).
* [**CS211: Software Engineering & OOP**](docs/cs211/) — High-level Object-Oriented design, Git & GitHub workflows, clean code standards, automated testing with `pytest`, and building reproducible bioinformatic pipelines with CI/CD.

### Algorithms Core
* [**CS201: Data Structures and Algorithms I**](docs/cs201/) — Comprehensive coverage of asymptotic analysis, sorting & searching, linear structures, priority queues & heaps, trees & tries, hashing & Bloom filters, graph algorithms, and dynamic programming.
* **CS202: Data Structures and Algorithms II** *(Upcoming)* — Advanced algorithmic techniques (amortized analysis, network flow, string matching algorithms, de Bruijn graphs).

### Mathematics Foundations
* [**MA101: Discrete Mathematics & Notation**](docs/ma101/) — The symbolic language of mathematics: summations ($\sum$), products ($\prod$), set-builder notation ($\in, \cup, \setminus$), and the Pigeonhole Principle.
* [**MA201: Calculus & Optimization**](docs/ma201/) — Single- and multivariable calculus, the gradient vector ($\nabla f$), critical points, the Hessian matrix ($H$), Positive Semi-Definiteness (PSD), and Maximum Likelihood Estimation.
* [**MA211: Linear Algebra**](docs/ma211/) — Matrix operations, Gaussian elimination ($A\mathbf{x} = \mathbf{b}$), determinants ($\det(A)$), matrix inverses, span/basis/dimension, and eigenvectors/eigenvalues powering PCA in genomics.
* **MA221: Probability** *(Upcoming)* — Random variables, distributions, expectation, Markov chains.
* **MA231: Statistics** *(Upcoming)* — Estimation, hypothesis testing, likelihood, Bayesian inference.

### Applied Electives
* **CS311: Classical Machine Learning** *(Upcoming)*
* **CS301: Applied Algorithms in Genomics** *(Upcoming)*
* **CS302: Applied Algorithms in Structural Bioinformatics** *(Upcoming)*
