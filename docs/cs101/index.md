---
layout: default
title: CS101 Programming Fundamentals
nav_order: 3
has_children: true
---

# CS101: Programming Fundamentals

Welcome to **CS101: Programming Fundamentals**!

In computational biology, code is the bridge between raw experimental sequencing data and biological insight. Rather than treating code as a black box or memorizing language syntax, this course develops clear mental models for **how data is represented in memory**, **how files are parsed safely without crashing**, and **how computational environments are managed reproducibly**.

---

## Course Modules

1. [**Mental Model of Programming**](mental-model.html)
   * *Rationale*: Understanding execution as a pure transformation pipeline: $\text{Input} \rightarrow \text{Transformations} \rightarrow \text{Output}$, and mapping biological entities to core data types.
2. [**Collections & Data Representation**](collections.html)
   * *Rationale*: Choosing the right data structure (Lists vs. Tuples vs. Dictionaries vs. Sets) based on lookup speed, uniqueness, and memory mutability.
3. [**Reading, Writing, & Parsing Biological Data**](file-io.html)
   * *Rationale*: Why reading entire gigabyte files into memory causes crashes ("slurping" vs. "streaming"), and how to handle real-world malformed data.
4. [**Packages & Environment Management**](package-management.html)
   * *Rationale*: Why dependency conflicts break scientific pipelines, why virtual environments are mandatory, and why modern tools like `uv` and `conda` are used for reproducible research.
