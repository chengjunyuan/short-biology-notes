---
layout: default
title: Hardware & Memory Bottlenecks
parent: CS100 Computer Fundamentals
nav_order: 1
---

# Hardware Architecture & Memory Bottlenecks

In wet-lab biology, experiments are constrained by pipette volumes, incubator capacities, and reaction kinetics. In computational biology, pipelines are constrained by **processors**, **memory (RAM)**, and **storage bandwidth**.

---

## 1. Processors: CPU vs. GPU vs. TPU

### The Central Processing Unit (CPU)
* **Design**: CPUs have a small number of very powerful, flexible cores (e.g., 8 to 128 cores on modern compute nodes).
* **Strength**: Executing complex, branching logic, conditional statements, and general algorithms sequentially or in moderate parallelism.
* **Bioinformatics Context**: Most classical bioinformatics algorithms—such as sequence aligners (`bwa`, `bowtie2`, `minimap2`), genome assemblers (`SPAdes`, `Flye`), and variant callers (`GATK`)—run on CPUs because sequence graph traversal and dynamic programming involve heavy branching logic.

### Graphics Processing Units (GPUs) & Tensor Processing Units (TPUs)
* **Design**: Thousands of smaller, simpler cores running identical operations in lockstep across massive matrices (*SIMD*: Single Instruction, Multiple Data).
* **Strength**: Dense matrix multiplications and tensor operations.
* **Why the distinction matters**:
  * Tools like **AlphaFold** (protein structure prediction) and **deep learning basecallers** (e.g., Oxford Nanopore `Guppy`/`Dorado`) require GPUs because they compute large neural network weights.
  * Attempting to run classical aligners or graph traversals on a GPU rarely gives speedups unless the algorithm has been explicitly redesigned for dense vector parallelism.

---

## 2. The Memory Hierarchy: Why RAM is the Real Bottleneck

A common frustration when first running bioinformatics software is the dreaded **Out of Memory (OOM) Killer**: your script runs for 4 hours and suddenly terminates with `Killed: 9`.

```
+-----------------------------------------------------------+
| Registers & Cache (L1/L2/L3) -> Extremely Fast (ns), Tiny |
+-----------------------------------------------------------+
                              |
+-----------------------------------------------------------+
| Main Memory (RAM)            -> Fast (10-100 ns), Gigabytes|
+-----------------------------------------------------------+
                              |
+-----------------------------------------------------------+
| NVMe SSD / Storage           -> Slower (Microseconds), TBs |
+-----------------------------------------------------------+
                              |
+-----------------------------------------------------------+
| Network Attached / HDD       -> Slowest (Milliseconds), PBs|
+-----------------------------------------------------------+
```

### Why RAM Exhaustion Happens
1. **Volatile vs. Persistent Storage**: RAM is the fast workspace your CPU reads from directly. Disk (SSD/HDD) is persistent storage. A CPU cannot directly execute calculations on data sitting on disk—it must first be loaded into RAM.
2. **Decompressing Data**: A compressed FASTQ file (`.fastq.gz`) might only take 5 GB on disk, but when loaded, uncompressed, and indexed into a graph or hash table in memory, it can easily expand to 30–60+ GB of RAM.
3. **Genome Assembly & Indexing**: Constructing a de Bruijn graph or suffix index for a large genome requires keeping billions of k-mer nodes in RAM simultaneously. If your machine has 16 GB of RAM and the graph requires 32 GB, the OS will either crash your job or use **swap space** (virtual memory on disk), slowing execution down by orders of magnitude.

---

## 3. Estimating Computational Footprint

Before launching a computation, calculate the theoretical lower bound of memory required:

* A human genome is approximately $3 \times 10^9$ base pairs.
* If stored as plain ASCII text (1 byte per character), the raw sequence is $\approx 3\text{ GB}$.
* In 2-bit encoded binary representation ($A=00, C=01, G=10, T=11$), the raw genome sequence can fit into $\approx 750\text{ MB}$.
* However, positional indices (like the FM-index or suffix array) and annotation metadata multiply this base requirement by $4\times$ to $10\times$.

> **Takeaway**: When planning bioinformatics experiments, always verify both the **disk footprint** (for storing raw/intermediate files) and the **peak memory requirement (RAM)** of your indexing and alignment tools.
