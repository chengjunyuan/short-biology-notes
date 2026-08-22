---
title: Home
layout: home
nav_order: 1
---

# Computational Biology Bridging Notes

This repository is a short guide for those transitioning from a life sciences background into computational biology and bioinformatics.

The focus is on the concepts, conventions, and tools used in day-to-day work. These notes are not meant to turn you into an expert; rather, the goal is to get you up to speed with the fundamentals, understand what is happening, and navigate computational environments with confidence.

---

## Curriculum Structure

### Foundational Computing
* [**CS100: Computer Fundamentals**](docs/cs100/) *(Complete)* — Physical hardware constraints (CPU/GPU, RAM, storage), memory exhaustion (OOM), operating system architecture (Linux/POSIX vs. Windows NT), remote terminal navigation (SSH, `tmux`), and HPC cluster job scheduling (SLURM/PBS).
* [**CS101: Software Engineering**](docs/cs101/) *(Complete)* — Development environments, data processing pipelines, clean code standards, modularity, object-oriented programming (OOP), isolated package management (`uv`, `conda`), collaborative Git workflows, automated testing (`pytest`), CI/CD pipelines, and reproducibility.

### Algorithms & Data Structures
* [**CS201: Data Structures and Algorithms I**](docs/cs201/) *(In Progress / WIP)* — Comprehensive coverage of asymptotic complexity ($O, \Omega, \Theta$), sorting and searching, linear data structures, heaps and priority queues, binary search trees, tries, hash tables, Bloom filters, graph algorithms, and dynamic programming.
* **CS202: Data Structures and Algorithms II** *(Upcoming / Not Started)* — Advanced algorithm design based on Kleinberg & Tardos (Ch. 1–7, 11–13): Gale-Shapley stable matching, rigorous greedy proofs (exchange arguments, clustering, Huffman), divide-and-conquer (FFT, closest pair), advanced DP (RNA secondary structure, linear-space Hirschberg alignment), network flow (Ford-Fulkerson, Max-Flow Min-Cut, bipartite matching), approximation algorithms (metric $k$-center, set cover, LP rounding), local search (Metropolis, simulated annealing), and randomized algorithms (Karger's min-cut, MAX 3-SAT, universal hashing, Chernoff bounds).

### Mathematical Foundations
* [**MA101: Discrete Mathematics & Notation**](docs/ma101/) *(In Progress / WIP)* — Decoding the formal mathematical notation of computational literature: summations ($\sum$), products ($\prod$), set-builder notation ($\in, \cup, \cap, \setminus$), propositional logic, and the Pigeonhole Principle in sequence alignment heuristics.
* [**MA201: Calculus & Optimization**](docs/ma201/) *(In Progress / WIP)* — Single- and multivariable calculus, the gradient vector ($\nabla f$), directional derivatives, critical point classification via the Hessian matrix ($H$), Positive Semi-Definiteness (PSD), and Maximum Likelihood Estimation (MLE).
* [**MA211: Linear Algebra**](docs/ma211/) *(In Progress / WIP)* — Matrix representation of high-dimensional biological data, systems of linear equations ($A\mathbf{x} = \mathbf{b}$), Gaussian elimination, geometric determinants, vector span and basis, and eigendecomposition powering Principal Component Analysis (PCA).
* **MA221: Probability** *(Upcoming / Not Started)* — Random variables, discrete and continuous probability distributions, expectation, variance, and Markov processes.
* **MA231: Statistics** *(Upcoming / Not Started)* — Statistical estimation, hypothesis testing, likelihood ratio tests, and Bayesian inference.

### Applied Computational Biology
* **CS301: Applied Algorithms in Genomics** *(Upcoming / Not Started)* — Burrows-Wheeler Transform (BWT), FM-index, Hidden Markov Models (HMMs), variant calling, and genome assembly.
* **CS302: Applied Algorithms in Structural Bioinformatics** *(Upcoming / Not Started)* — Macromolecular coordinate systems, energy force fields, molecular dynamics simulations, molecular docking, and structural prediction architectures.
* **CS311: Classical Machine Learning** *(Upcoming / Not Started)* — Supervised and unsupervised statistical learning, feature selection, support vector machines, decision trees, random forests, and dimensionality reduction.
