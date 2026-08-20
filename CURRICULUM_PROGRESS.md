# Computational Biology Bridging Program — Progress & Handover Note

*Last updated: 2026-08-18*

This document tracks the design philosophy, completed courses, and roadmap for the **Life Sciences to Computational Biology Bridging Curriculum**.

---

## 1. Core Pedagogical Philosophy

1. **Problem-First & Rationale-First ("Why over How")**:
   Every topic answers *why* specific tools, hardware architectures, or mathematical concepts exist by addressing concrete practitioner dilemmas:
   * *CS100*: Why RAM is required as a low-latency intermediate workspace; why CPUs handle branching logic while GPUs accelerate dense tensor operations; why scientific software standardizes on POSIX/Linux; why `tmux` survives network drops (`SIGHUP`); how SLURM/PBS allocates hardware.
   * *CS101*: Why streaming file I/O is mandatory over memory slurping, why `uv` and virtual environments prevent dependency drift.
   * *CS211*: Why OOP bundles state + behavior, why CI/CD automated tests act as assay controls against silent computational bugs.
   * *CS201*: Why binary search and trees bypass linear scans, why $K$-way merges using heaps enable streaming BAM file sorting.
   * *MA101*: Why notation like $\sum$, $\prod$, and set-builder notation decodes computational papers; why Pigeonhole Principle explains seed-and-extend heuristics in aligners.
   * *MA201*: Why derivatives enable Maximum Likelihood Estimation; why the Hessian matrix determines local optima.
   * *MA211*: Why eigendecomposition and PCA reduce 20,000-gene spaces into 2D single-cell clusters.

2. **Tone, Terminology & Structure Standards**:
   * **Tone**: Formal, direct, academic, and rigorous (no conversational filler, exclamation points, or meta-commentary).
   * **Landing Pages**: Use `## Topics` header; provide concise bulleted lists under topic links without "Rationale:" tags.
   * **Technical Vocabulary**: Define computing jargon (*bare-metal*, *hypervisor*, *daemon*, *node*, *subsystem*) upon first mention with concrete examples.
   * **Failure Modes**: Provide exact error signatures (e.g. `Killed: 9`, `CUDA out of memory`, `/bin/bash^M: bad interpreter`, `SIGHUP`) and remediation commands.

3. **LaTeX / KaTeX Formatting Convention**:
   * All matrix (`bmatrix`, `vmatrix`), alignment (`aligned`), and piecewise (`cases`) environments inside Markdown `$$...$$` blocks must be kept on **single lines** (or without line-broken `&` tokens) to ensure seamless rendering across KaTeX, Antigravity, and Jekyll.

---

## 2. Completed Modules & Directory Structure

### Foundational Courses
* **`docs/cs100/` — CS100: Computer Fundamentals**
  * `index.md` — Overview & formal topics overview
  * `hardware.md` — CPU-RAM-SSD triad, cores/threads (SMT), CPU vs GPU acceleration realities, memory/storage hierarchy, hardware evaluation metrics, SLURM allocations, OOM/swap failure modes
  * `operating-systems.md` — Kernel vs user space, deployment formats (bare-metal, VMs, containers, WSL2), Unix vs Windows NT divergence, terminals vs shells (`cmd`, PowerShell, Anaconda Prompt, PuTTY, WSL2), command resolution (`ls`, `pwd`), POSIX stream pipes, cross-platform pitfalls (`LF`/`CRLF`, paths, case sensitivity)
  * `servers-and-hpc.md` — Physical server architecture (racks, wired fiber, data transfer), laptop vs server decision matrix, SSH under the hood (keys vs passwords, remote IDEs), `tmux` persistence against `SIGHUP`, `$PATH` & `.bashrc`/`.zshrc` dotfiles, shared cluster node specialization (login vs compute nodes) and SLURM/PBS job scheduling
* **`docs/cs101/` — CS101: Software Engineering**
  * `index.md` — Overview & formal topics roadmap
  * `pipeline-and-ide.md` — IDE architecture (VS Code, LSP, debuggers, remote SSH) and the Ingest $\rightarrow$ Process $\rightarrow$ Emit execution triad
  * `clean-code-and-functions.md` — Readability, PEP 8 naming conventions, function decomposition, docstrings, type annotations (`mypy`), and linters (`ruff`)
  * `oop-fundamentals.md` — Objects as custom data types, class blueprints vs. instances, encapsulation, validation, and `@dataclass`
  * `package-management.md` — Dependency drift, virtual environment isolation, fast Python package management with `uv`, and binary environments with `conda`/`mamba`
  * `version-control.md` — Git DAG mental model, feature branching, GitHub Pull Requests, peer code review, and merge conflict resolution
  * `sdlc-and-reproducibility.md` — Scientific SDLC cycle, automated testing with `pytest`, GitHub Actions CI, structured logging, and containerization (Docker/Singularity)

### Algorithms Core
* **`docs/cs201/` — CS201: Data Structures & Algorithms I (CS2040S-aligned)**
  * `index.md` — Overview & syllabus
  * `algorithm-analysis.md` — Asymptotic complexity ($O, \Omega, \Theta$), scaling to $10^9$ reads, space complexity
  * `searching-and-sorting.md` — Binary search, Merge/Quick sort, Radix sort ($O(N \cdot L)$ on fixed-length reads)
  * `linear-structures-and-heaps.md` — Dynamic arrays vs. linked lists, stacks, queues, heaps for $K$-way BAM merge
  * `trees-and-tries.md` — BST degeneracies, AVL rotations, Tries for prefix matching, Union-Find for sequence clustering
  * `hashing-and-sets.md` — Hash tables, amortized resizing, Bloom filters for low-memory $k$-mer filtering
  * `graphs.md` — Adjacency lists, BFS/DFS, DAGs & topological sort in pipelines, Dijkstra, MST
  * `dynamic-programming.md` — Memoization vs. tabulation, LCS, foundation for Needleman-Wunsch & Smith-Waterman

### Mathematics Foundations
* **`docs/ma101/` — MA101: Discrete Mathematics & Notation**
  * `index.md` — Overview & decoder of mathematical language
  * `sums-and-products.md` — $\sum$ and $\prod$ notation, Python loops, log-likelihood conversions
  * `sets-and-logic.md` — Number systems ($\mathbb{N}, \mathbb{Z}, \mathbb{Q}, \mathbb{R}$), set-builder notation, operations ($\cup, \cap, \setminus, A^c$)
  * `pigeonhole-principle.md` — Principle statement, hash collisions, seed-and-extend speedup in aligners
* **`docs/ma201/` — MA201: Calculus & Optimization**
  * `index.md` — Overview & role of calculus in model optimization
  * `single-variable-calculus.md` — Derivative rules, critical points ($f'=0$), second derivative test, MLE worked example
  * `multivariable-calculus.md` — Partial derivatives, gradient vector $\nabla f$, directional derivatives
  * `optimization-and-hessian.md` — Critical points ($\nabla f = \mathbf{0}$), Hessian matrix, PSD tests, Gradient Descent
* **`docs/ma211/` — MA211: Linear Algebra**
  * `index.md` — Overview & role of linear algebra in high-dimensional biology
  * `matrices-and-operations.md` — Matrix representation of expression data, $AB$, transpose $A^T$
  * `linear-systems-and-row-reduction.md` — Formulating $A\mathbf{x} = \mathbf{b}$, augmented matrices, Gaussian elimination
  * `determinants-and-inverses.md` — Geometric determinant interpretation, invertibility condition, matrix inverses
  * `span-basis-and-dimension.md` — Linear combinations, span, linear independence, basis, dimension reduction
  * `eigenvalues-and-eigenvectors.md` — Characteristic equation, eigendecomposition, PCA intuition for single-cell clustering

---

## 3. Remaining Roadmap

### Upcoming Compulsory Modules
1. **CS202 Data Structures and Algorithms II** *(CS3230 alignment: Advanced DP, Greedy, Network Flow, NP-completeness, String Algorithms / de Bruijn graphs)*
2. **MA221 Probability** *(Random variables, common distributions, expectation, Markov chains)*
3. **MA231 Statistics** *(Estimation, hypothesis testing, likelihood ratios, Bayesian inference)*

### Upcoming Elective Modules
1. **CS311 Classical Machine Learning** *(Supervised/unsupervised learning, SVMs, decision trees, random forests)*
2. **CS301 Applied Algorithms (Genomics)** *(Burrows-Wheeler transform, FM-index, HMMs, variant calling)*
3. **CS302 Applied Algorithms (Structural Bioinformatics)** *(Protein structure representation, force fields, molecular dynamics, docking, AlphaFold principles)*
