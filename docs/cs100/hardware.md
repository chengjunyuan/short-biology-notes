---
layout: default
title: Hardware Fundamentals
parent: CS100 Computer Fundamentals
nav_order: 1
---

# Hardware Fundamentals

At its core, **computation** is the process of taking input data, transforming it through a sequence of arithmetic and logical operations, and producing an output result. 

Every computing platform—from a laptop to a high-performance compute node—implements this process using three primary hardware components: **Processors (CPU/GPU)**, **Main Memory (RAM)**, and **Persistent Storage (SSD/HDD)**. Understanding how these components interact enables informed hardware selection, effective compute resource allocation on clusters, and accurate diagnosis of performance bottlenecks.

---

## 1. The Core Triad: Storage, Memory, and Processors

```
+------------------+         High-Speed Bus        +--------------------+
|  Storage (SSD)   | ----------------------------> | Main Memory (RAM)  |
|  - Non-volatile  |   Loaded into working memory  | - Volatile         |
|  - High capacity |                               | - Low latency      |
+------------------+                               +--------------------+
                                                             |
                                                   Fast Bus  | Instruction &
                                                   Fetch     | Data Stream
                                                             v
                                                   +--------------------+
                                                   |  Processor (CPU)   |
                                                   |  - Execution Unit  |
                                                   |  - Ultra-fast      |
                                                   +--------------------+
```

### Persistent Storage (SSD / HDD)
* **Role**: The persistent repository for operating systems, installed software, and datasets.
* **Characteristics**: **Non-volatile** (data persists when powered off) and high-capacity, but with the slowest read/write speeds in the system.

### Processors (CPU / GPU)
* **Role**: The active execution units that read program instructions and execute mathematical and logical calculations on data.
* **Characteristics**: Operates at gigahertz clock frequencies (billions of operations per second), operating in fractions of a nanosecond.

### The Necessity of Main Memory (RAM)
Because processors operate in fractions of a nanosecond, reading operands directly from an SSD for every calculation would stall the processor for thousands of idle cycles per instruction.

**Random Access Memory (RAM)** bridges this speed gap:
* **Volatile Working Workspace**: Provides read/write access times measured in tens of nanoseconds, holding active application binaries and runtime data.
* **Program Execution Lifecycle**: When launching a program or loading a dataset, the operating system copies data from persistent storage into RAM. The processor reads from and writes to RAM directly during execution.
* **Random Access**: Any memory address in RAM can be accessed in constant time $O(1)$, regardless of physical placement.

---

## 2. Processors: CPUs, Cores, Threads, and Key Specifications

When purchasing a system, evaluating a workstation, or requesting CPU resources on a shared cluster (e.g., `#SBATCH --cpus-per-task=16`), processor performance is governed by several core metrics.

```
+-----------------------------------------------------------------------+
| Single CPU Socket                                                     |
|                                                                       |
|  +---------------------------------+ +------------------------------+ |
|  | Physical Core 0                 | | Physical Core 1              | |
|  |  [Regs] [L1/L2 Cache] [ALU]    | |  [Regs] [L1/L2 Cache] [ALU] | |
|  |  +------------+  +------------+ | |  +------------+  +---------+ | |
|  |  | Thread 0   |  | Thread 1   | | |  | Thread 0   |  | Thread 1| | |
|  |  +------------+  +------------+ | |  +------------+  +---------+ | |
|  +---------------------------------+ +------------------------------+ |
|                                                                       |
|  +-----------------------------------------------------------------+  |
|  | Shared L3 Cache (e.g., 32 MB - 256 MB)                          |  |
|  +-----------------------------------------------------------------+  |
+-----------------------------------------------------------------------+
```

### Physical Cores vs. Logical Threads
* **Physical Core**: An independent physical processing unit containing its own Arithmetic Logic Unit (ALU), Floating Point Unit (FPU), registers, and L1/L2 caches. An 8-core CPU can execute 8 physical instruction streams simultaneously.
* **Logical Thread (SMT / Hyper-Threading)**: Duplicates architectural state registers on a single physical core. When one thread stalls waiting for memory, the core instantly switches execution to the second thread.
* **Inspecting Resources**: On personal machines and servers, tools like `os.cpu_count()` in Python, `nproc`, `lscpu`, or `top` report total **logical threads** (e.g., an 8-core CPU with SMT presents 16 logical CPUs).

### What Makes a "Good" CPU? Key Evaluation Metrics
1. **Clock Speed (Frequency in GHz)**:
   * *Base vs. Boost Clock* (e.g., 3.5 GHz base / 5.2 GHz boost). Measures how many clock cycles a single core executes per second.
   * *Impact*: Dictates single-threaded execution speed for sequential algorithms that cannot be parallelized.
2. **Core / Thread Count**:
   * *Scale*: 8–16 cores on modern laptops/desktops; 32–128 cores on enterprise server nodes.
   * *Impact*: Dictates throughput for parallelizable workflows (e.g., multi-threaded sequence aligners, parallel simulations, distributed pipelines).
3. **Cache Size (L3 Cache in MB)**:
   * Large shared on-die caches (e.g., 32–256 MB) reduce the frequency of expensive RAM access penalties.
4. **Memory Channels and Bandwidth**:
   * Consumer desktop CPUs typically support **Dual-Channel** RAM; server CPUs (e.g., AMD EPYC, Intel Xeon) support **8-Channel** memory, providing massive memory bandwidth to prevent multicore starvation.

---

## 3. Specialized Acceleration: CPUs vs. GPUs vs. TPUs

When configuring workloads or requesting cluster accelerators (e.g., `#SBATCH --gpus=1`), understanding the fundamental architectural differences between CPUs and GPUs determines whether a workload will actually benefit from acceleration.

```
CPU: Few Powerful Cores                    GPU: Thousands of Simple Cores
(Optimized for Low-Latency Branching)       (Optimized for Massive Data Parallelism)

+-------+ +-------+ +-------+ +-------+    [][][][][][][][][][][][][][][][][][][]
| Core  | | Core  | | Core  | | Core  |    [][][][][][][][][][][][][][][][][][][]
|   1   | |   2   | |   3   | |   4   |    [][][][][][][][][][][][][][][][][][][]
+-------+ +-------+ +-------+ +-------+    [][][][][][][][][][][][][][][][][][][]
| Large Cache, Branch Predictor, MMU  |    [][][][][][][][][][][][][][][][][][][]
+-------------------------------------+    +------------------------------------+
                                           | Small Cache, High-Bandwidth VRAM   |
                                           +------------------------------------+
```

### Architectural Comparison

| Dimension | Central Processing Unit (CPU) | Graphics Processing Unit (GPU) | Tensor Processing Unit (TPU) |
| :--- | :--- | :--- | :--- |
| **Core Architecture** | Few large, complex cores (8–128) | Thousands of smaller SIMD cores (2,000–16,000+) | Dedicated systolic array matrix units |
| **Optimization Goal** | Low latency, complex control logic | High throughput, massive data parallelism | High-throughput matrix multiplication |
| **Execution Model** | MIMD (Multiple Instruction, Multiple Data) | SIMD / SIMT (Single Instruction, Multiple Threads) | Hardware-level tensor contraction |
| **Working Memory** | System RAM (32 GB – 2 TB+) | On-board discrete VRAM (8 GB – 80 GB) | High-Bandwidth Memory (HBM) |
| **Primary Workloads** | General computing, sequence alignment (`bwa`, `minimap2`), graph traversals, data parsing | Deep learning (AlphaFold, basecalling), cryo-EM reconstruction, molecular dynamics | Large-scale deep learning model training |

### Why GPUs Do NOT Accelerate All Workloads
A common misconception is that adding GPUs will automatically speed up any computational job. In practice, many workloads show zero performance gain—or even run slower—when assigned GPUs due to three factors:

1. **Software Support & GPU Kernels**: A standard command-line tool, C/C++ binary, or standard Python/R script compiles exclusively for CPU instruction sets (x86/ARM). Unless software is explicitly written to target GPU APIs (such as **NVIDIA CUDA**, **OpenCL**, **PyTorch**, or **JAX**), the GPU sits entirely idle (0% utilization) while the CPU does all the work.
2. **Algorithm Structure & Branch Divergence**: GPUs achieve speedups only when thousands of threads execute the *exact same instruction* across different data elements in lockstep (SIMD). Algorithms with heavy conditional branching (`if`/`else`), pointer chasing, or irregular graph traversal (e.g., Smith-Waterman sequence alignment, de Bruijn graph assembly) force GPU threads to serialize, negating parallel advantages.
3. **Host-to-Device Memory Transfer Overhead**: Data must be transferred from System RAM across the PCIe bus into GPU on-board memory (VRAM) before computation, and results copied back. If the computation is not mathematically dense enough, PCIe transfer latency outweighs any processing speedup.

### What Makes a "Good" GPU? Key Evaluation Metrics
1. **Video RAM (VRAM Capacity in GB)**:
   * *Scale*: 8–24 GB on consumer GPUs (e.g., RTX 4080/4090); 40–80+ GB on datacenter GPUs (e.g., NVIDIA A100, H100).
   * *Significance*: VRAM is a **hard constraint**. If a neural network model, batch size, or 3D cryo-EM volume exceeds physical VRAM, the job terminates immediately with a `CUDA Out of Memory` error.
2. **Memory Bandwidth (GB/s or TB/s)**:
   * Determines how fast data can be streamed from on-card VRAM into GPU tensor cores (e.g., >2,000 GB/s on modern datacenter GPUs vs. ~50–100 GB/s for system RAM).
3. **Tensor Cores & Precision Support (FP32, FP16, BF16, INT8)**:
   * Dedicated hardware units for mixed-precision matrix multiplication, accelerating deep learning calculations by multiples compared to standard FP32 floating-point cores.

---

## 4. The Memory & Storage Hierarchy

Hardware balances **speed**, **capacity**, and **cost** across hierarchical tiers:

```
+----------------------------------------------------------------------+
| CPU Registers (< 1 ns latency, ~KB)                                  |  Fastest, Smallest,
|   L1 / L2 / L3 CPU Caches (1 - 15 ns latency, ~MB)                   |  Highest Cost / GB
+----------------------------------------------------------------------+
| Main Memory: RAM / GPU VRAM (50 - 100 ns latency, 16 GB - 2 TB)      |
+----------------------------------------------------------------------+
| NVMe Solid State Drive (10 - 100 µs latency, 1 - 30+ TB)             |  Slowest, Largest,
|   Hard Disk Drive / Network Storage (1 - 10 ms latency, Petabytes)   |  Lowest Cost / GB
+----------------------------------------------------------------------+
```

### Latency Scale Comparison (Human-Scaled Intuition)
Scaling access times to a human scale (where 1 CPU cycle $\approx$ 1 second) illustrates why memory location dictates execution speed:

| Hierarchy Level | Actual Hardware Latency | Scaled Human Equivalent |
| :--- | :--- | :--- |
| **CPU Register / L1 Cache** | $0.5 - 1\text{ ns}$ | 1 second |
| **L3 Cache** | $10 - 15\text{ ns}$ | 15 seconds |
| **Main Memory (RAM)** | $50 - 100\text{ ns}$ | 1.5 minutes |
| **NVMe Solid-State Drive** | $25 - 100\ \mu\text{s}$ | 1 to 2 days |
| **Rotational Hard Drive (HDD)** | $5 - 10\text{ ms}$ | 2 to 4 months |

### Storage & RAM Evaluation Metrics
* **RAM**: Evaluated primarily by **Capacity (GB/TB)** (determines the maximum dataset size held in working memory) and **Transfer Rate** (e.g., DDR4-3200 vs. DDR5-5600 MT/s).
* **Storage (SSD vs. HDD)**:
  * **NVMe PCIe SSDs**: Transfer speeds of 3,500–7,000+ MB/s with high Input/Output Operations Per Second (IOPS). Essential for high-throughput pipeline scratch directories.
  * **SATA SSDs**: Capped at ~500 MB/s due to legacy SATA interface limits.
  * **Spinning HDDs**: Slow sequential transfers (~150–250 MB/s) and very poor random access, used primarily for archival and cold storage.

---

## 5. Demystifying Job Allocations & Failure Modes

When submitting jobs on institutional clusters (or provisioning cloud compute instances), job parameters map directly to physical hardware boundaries:

```bash
# Example SLURM Job Submission Header
#SBATCH --cpus-per-task=16   # 16 logical CPU worker threads allocated
#SBATCH --mem=64G            # 64 GB of physical System RAM allocated
#SBATCH --gpus=1             # 1 dedicated discrete GPU accelerator assigned
```

### What These Resource Parameters Mean
* **`--cpus-per-task`**: Assigns specific CPU execution threads to your process. Setting this value higher only speeds up software that includes explicit multi-threading parameters (e.g., `samtools sort -@ 16` or `blastn -num_threads 16`).
* **`--mem`**: Defines the physical RAM ceiling for your job. If your application's active data structures expand beyond this limit, the OS kernel will terminate the process.
* **`--gpus`**: Exposes dedicated GPU hardware to your execution environment. Only effective for software built with GPU-accelerated backend libraries.

### System Failure Modes: OOM and Swap
1. **Out of Memory (OOM) Killer (`Killed: 9` / Exit Code 137)**:
   * When total memory requests exceed allocated physical RAM, the Linux kernel activates the **OOM Killer** to protect system stability. It sends signal 9 (`SIGKILL`), abruptly aborting the process without saving intermediate state.
2. **CUDA Out of Memory (`RuntimeError: CUDA out of memory`)**:
   * Occurs when a deep learning model, batch size, or tensor allocation exceeds the physical VRAM capacity of the GPU. Unlike CPU memory, GPUs cannot easily spill over to disk without massive performance stalls.
3. **Swap Thrashing**:
   * If a system is configured to use disk storage as overflow memory (swap space), exceeding RAM causes the OS to constantly page memory blocks between RAM and disk. Execution speeds collapse by orders of magnitude, making jobs appear permanently hung.

> **Summary**: Effective computational performance requires matching algorithms to hardware strengths: high-clock CPUs for sequential logic, multicore threads for parallel data chunks, and GPUs with ample VRAM for dense tensor operations. Aligning job resource requests with physical hardware prevents both resource waste and out-of-memory terminations.

<div class="page-nav">
  <a href="index.html" class="page-nav-prev">&larr; CS100 Overview</a>
  <a href="operating-systems.html" class="page-nav-next">Next: Operating Systems &rarr;</a>
</div>

