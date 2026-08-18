---
layout: default
title: CS100 Computer Fundamentals
nav_order: 2
has_children: true
---

# CS100: Computer Fundamentals

Welcome to **CS100: Computer Fundamentals**! 

When analyzing biological data—whether assembling a 3-gigabase mammalian genome, running molecular dynamics simulations, or querying billions of short sequencing reads—computation is constrained by physical hardware and operating system realities.

The goal of this course is not to memorize trivia, but to develop an intuition for **why computation behaves the way it does** and how to work effectively in remote, high-performance environments.

---

## Course Modules

1. [**Hardware Architecture & Memory Bottlenecks**](hardware.md)
   * *Rationale*: Why biological computations often crash due to out-of-memory (OOM) errors rather than slow CPUs, and when GPUs/TPUs actually help vs. when they are unnecessary.
2. [**Operating Systems & Command-Line Implications**](operating-systems.md)
   * *Rationale*: Why the bioinformatics world runs overwhelmingly on Linux/POSIX, and how differences in file paths, case sensitivity, and line endings (`CRLF` vs. `LF`) cause silent pipeline failures.
3. [**Working with Remote Servers & HPC**](servers-and-hpc.md)
   * *Rationale*: Why biological workloads cannot run on personal laptops, how SSH and terminal multiplexers (`tmux`) keep computations alive, and how job schedulers (SLURM) allocate shared cluster resources.
