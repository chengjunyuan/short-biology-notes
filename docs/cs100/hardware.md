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

<!-- Core Triad Architecture Schematic -->
<svg viewBox="0 0 780 250" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-blue" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#2563eb" />
    </marker>
    <marker id="arrow-green" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
  </defs>

  <!-- Background Container -->
  <rect width="780" height="250" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Storage Node -->
  <g transform="translate(25, 30)">
    <rect width="215" height="135" rx="8" fill="#ffffff" stroke="#94a3b8" stroke-width="1.5"/>
    <rect width="215" height="30" rx="8" fill="#f1f5f9" stroke="#94a3b8" stroke-width="1.5"/>
    <text x="107" y="20" text-anchor="middle" font-size="13" font-weight="700" fill="#334155">Storage (NVMe / SSD)</text>
    <text x="16" y="55" font-size="12" fill="#475569">• Non-volatile (Persistent)</text>
    <text x="16" y="76" font-size="12" fill="#475569">• High capacity (Terabytes)</text>
    <text x="16" y="97" font-size="12" fill="#64748b">• Latency: 25 – 100 µs</text>
    <rect x="14" y="108" width="187" height="20" rx="4" fill="#fee2e2"/>
    <text x="107" y="122" text-anchor="middle" font-size="11" font-weight="600" fill="#991b1b">Persistent & Slowest Tier</text>
  </g>

  <!-- Bus 1: Storage to RAM -->
  <path d="M 240 95 L 285 95" stroke="#2563eb" stroke-width="2.5" stroke-dasharray="6,3" marker-end="url(#arrow-blue)"/>
  <text x="262" y="80" text-anchor="middle" font-size="11" font-weight="600" fill="#2563eb">PCIe Bus</text>
  <text x="262" y="115" text-anchor="middle" font-size="10" fill="#64748b">Load Data</text>

  <!-- RAM Node -->
  <g transform="translate(285, 30)">
    <rect width="215" height="135" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="2"/>
    <rect width="215" height="30" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="2"/>
    <text x="107" y="20" text-anchor="middle" font-size="13" font-weight="700" fill="#1d4ed8">Main Memory (RAM)</text>
    <text x="16" y="55" font-size="12" fill="#1e293b">• Volatile Workspace</text>
    <text x="16" y="76" font-size="12" fill="#1e293b">• Direct OS & Binary Buffer</text>
    <text x="16" y="97" font-size="12" fill="#64748b">• Latency: 50 – 100 ns</text>
    <rect x="14" y="108" width="187" height="20" rx="4" fill="#dbeafe"/>
    <text x="107" y="122" text-anchor="middle" font-size="11" font-weight="600" fill="#1e40af">Speed Bridge & Staging Area</text>
  </g>

  <!-- Bus 2: RAM to CPU -->
  <path d="M 500 85 L 535 85" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-green)"/>
  <path d="M 540 105 L 505 105" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-green)"/>
  <text x="510" y="72" text-anchor="middle" font-size="11" font-weight="600" fill="#059669">Memory Bus</text>
  <text x="510" y="125" text-anchor="middle" font-size="10" fill="#64748b">DDR Channels</text>

  <!-- CPU Node -->
  <g transform="translate(540, 30)">
    <rect width="215" height="135" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="2"/>
    <rect width="215" height="30" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="2"/>
    <text x="107" y="20" text-anchor="middle" font-size="13" font-weight="700" fill="#047857">Processor (CPU / GPU)</text>
    <text x="16" y="55" font-size="12" fill="#064e3b">• Arithmetic Logic Unit (ALU)</text>
    <text x="16" y="76" font-size="12" fill="#064e3b">• Registers & L1/L2 Caches</text>
    <text x="16" y="97" font-size="12" fill="#64748b">• Latency: &lt; 1 ns (Cycles)</text>
    <rect x="14" y="108" width="187" height="20" rx="4" fill="#d1fae5"/>
    <text x="107" y="122" text-anchor="middle" font-size="11" font-weight="600" fill="#065f46">Active Computation Engine</text>
  </g>

  <!-- Bottom Workflow Summary Banner -->
  <g transform="translate(25, 185)">
    <rect width="730" height="42" rx="6" fill="#ffffff" stroke="#cbd5e1" stroke-width="1"/>
    <text x="365" y="26" text-anchor="middle" font-size="12" fill="#334155">
      <tspan font-weight="700" fill="#0f172a">Execution Data Flow: </tspan>
      Data/Binaries stored on SSD ➔ Copied to RAM at runtime ➔ Fetched into CPU registers for execution
    </text>
  </g>
</svg>


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

<!-- CPU Socket, Cores, Threads, and Cache Topology Schematic -->
<svg viewBox="0 0 780 320" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-slate" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#64748b" />
    </marker>
  </defs>

  <!-- CPU Socket Outer Package -->
  <rect width="780" height="320" rx="10" fill="#f8fafc" stroke="#64748b" stroke-width="2"/>
  <rect width="780" height="32" rx="10" fill="#334155"/>
  <text x="20" y="21" font-size="13" font-weight="700" fill="#ffffff">Single CPU Socket (Physical Silicon Die Package)</text>

  <!-- Physical Core 0 Container -->
  <g transform="translate(25, 48)">
    <rect width="350" height="175" rx="8" fill="#ffffff" stroke="#cbd5e1" stroke-width="1.5"/>
    <rect width="350" height="26" rx="8" fill="#e2e8f0" stroke="#cbd5e1" stroke-width="1.5"/>
    <text x="175" y="17" text-anchor="middle" font-size="12" font-weight="700" fill="#1e293b">Physical Core 0</text>
    
    <!-- Private Core Resources -->
    <rect x="15" y="36" width="95" height="42" rx="5" fill="#f1f5f9" stroke="#94a3b8"/>
    <text x="62" y="54" text-anchor="middle" font-size="11" font-weight="600" fill="#0f172a">ALU &amp; FPU</text>
    <text x="62" y="68" text-anchor="middle" font-size="9" fill="#64748b">Math Units</text>

    <rect x="120" y="36" width="100" height="42" rx="5" fill="#f1f5f9" stroke="#94a3b8"/>
    <text x="170" y="54" text-anchor="middle" font-size="11" font-weight="600" fill="#0f172a">Registers</text>
    <text x="170" y="68" text-anchor="middle" font-size="9" fill="#64748b">&lt; 1 ns Access</text>

    <rect x="230" y="36" width="105" height="42" rx="5" fill="#f1f5f9" stroke="#94a3b8"/>
    <text x="282" y="54" text-anchor="middle" font-size="11" font-weight="600" fill="#0f172a">L1/L2 Cache</text>
    <text x="282" y="68" text-anchor="middle" font-size="9" fill="#64748b">Private Cache</text>

    <!-- SMT Logical Threads -->
    <rect x="15" y="90" width="155" height="68" rx="6" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="92" y="112" text-anchor="middle" font-size="11" font-weight="700" fill="#1d4ed8">Logical Thread 0</text>
    <text x="92" y="128" text-anchor="middle" font-size="10" fill="#475569">Architectural State</text>
    <text x="92" y="144" text-anchor="middle" font-size="9" fill="#2563eb">Active Pipeline</text>

    <rect x="180" y="90" width="155" height="68" rx="6" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="257" y="112" text-anchor="middle" font-size="11" font-weight="700" fill="#1d4ed8">Logical Thread 1</text>
    <text x="257" y="128" text-anchor="middle" font-size="10" fill="#475569">Architectural State</text>
    <text x="257" y="144" text-anchor="middle" font-size="9" fill="#2563eb">SMT / Hyper-Thread</text>
  </g>

  <!-- Physical Core 1 Container -->
  <g transform="translate(405, 48)">
    <rect width="350" height="175" rx="8" fill="#ffffff" stroke="#cbd5e1" stroke-width="1.5"/>
    <rect width="350" height="26" rx="8" fill="#e2e8f0" stroke="#cbd5e1" stroke-width="1.5"/>
    <text x="175" y="17" text-anchor="middle" font-size="12" font-weight="700" fill="#1e293b">Physical Core 1</text>
    
    <!-- Private Core Resources -->
    <rect x="15" y="36" width="95" height="42" rx="5" fill="#f1f5f9" stroke="#94a3b8"/>
    <text x="62" y="54" text-anchor="middle" font-size="11" font-weight="600" fill="#0f172a">ALU &amp; FPU</text>
    <text x="62" y="68" text-anchor="middle" font-size="9" fill="#64748b">Math Units</text>

    <rect x="120" y="36" width="100" height="42" rx="5" fill="#f1f5f9" stroke="#94a3b8"/>
    <text x="170" y="54" text-anchor="middle" font-size="11" font-weight="600" fill="#0f172a">Registers</text>
    <text x="170" y="68" text-anchor="middle" font-size="9" fill="#64748b">&lt; 1 ns Access</text>

    <rect x="230" y="36" width="105" height="42" rx="5" fill="#f1f5f9" stroke="#94a3b8"/>
    <text x="282" y="54" text-anchor="middle" font-size="11" font-weight="600" fill="#0f172a">L1/L2 Cache</text>
    <text x="282" y="68" text-anchor="middle" font-size="9" fill="#64748b">Private Cache</text>

    <!-- SMT Logical Threads -->
    <rect x="15" y="90" width="155" height="68" rx="6" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="92" y="112" text-anchor="middle" font-size="11" font-weight="700" fill="#1d4ed8">Logical Thread 0</text>
    <text x="92" y="128" text-anchor="middle" font-size="10" fill="#475569">Architectural State</text>
    <text x="92" y="144" text-anchor="middle" font-size="9" fill="#2563eb">Active Pipeline</text>

    <rect x="180" y="90" width="155" height="68" rx="6" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="257" y="112" text-anchor="middle" font-size="11" font-weight="700" fill="#1d4ed8">Logical Thread 1</text>
    <text x="257" y="128" text-anchor="middle" font-size="10" fill="#475569">Architectural State</text>
    <text x="257" y="144" text-anchor="middle" font-size="9" fill="#2563eb">SMT / Hyper-Thread</text>
  </g>

  <!-- Bus lines from cores down to L3 -->
  <path d="M 200 223 L 200 236" stroke="#64748b" stroke-width="2" marker-end="url(#arrow-slate)"/>
  <path d="M 580 223 L 580 236" stroke="#64748b" stroke-width="2" marker-end="url(#arrow-slate)"/>

  <!-- Shared L3 Cache Unit -->
  <g transform="translate(25, 240)">
    <rect width="730" height="62" rx="8" fill="#fef3c7" stroke="#f59e0b" stroke-width="1.5"/>
    <text x="365" y="26" text-anchor="middle" font-size="13" font-weight="700" fill="#92400e">Shared L3 Cache Pool (32 MB – 256 MB on-die)</text>
    <text x="365" y="46" text-anchor="middle" font-size="11" fill="#78350f">Unified cache accessible by all physical cores • Connects directly to System Memory Bus (RAM)</text>
  </g>
</svg>


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

<!-- CPU vs GPU Architecture Comparison Schematic -->
<svg viewBox="0 0 780 290" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <!-- Background Canvas -->
  <rect width="780" height="290" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Left: CPU Panel -->
  <g transform="translate(20, 20)">
    <rect width="360" height="250" rx="8" fill="#ffffff" stroke="#cbd5e1" stroke-width="1.5"/>
    <rect width="360" height="32" rx="8" fill="#eff6ff" stroke="#cbd5e1" stroke-width="1.5"/>
    <text x="180" y="21" text-anchor="middle" font-size="13" font-weight="700" fill="#1e40af">CPU: Few Powerful Cores (MIMD)</text>

    <!-- 4 Big Cores -->
    <g transform="translate(15, 45)">
      <rect x="0" y="0" width="75" height="70" rx="5" fill="#f1f5f9" stroke="#3b82f6" stroke-width="1.5"/>
      <text x="37" y="32" text-anchor="middle" font-size="11" font-weight="700" fill="#1e293b">Core 0</text>
      <text x="37" y="50" text-anchor="middle" font-size="9" fill="#64748b">ALU + Regs</text>

      <rect x="85" y="0" width="75" height="70" rx="5" fill="#f1f5f9" stroke="#3b82f6" stroke-width="1.5"/>
      <text x="122" y="32" text-anchor="middle" font-size="11" font-weight="700" fill="#1e293b">Core 1</text>
      <text x="122" y="50" text-anchor="middle" font-size="9" fill="#64748b">ALU + Regs</text>

      <rect x="170" y="0" width="75" height="70" rx="5" fill="#f1f5f9" stroke="#3b82f6" stroke-width="1.5"/>
      <text x="207" y="32" text-anchor="middle" font-size="11" font-weight="700" fill="#1e293b">Core 2</text>
      <text x="207" y="50" text-anchor="middle" font-size="9" fill="#64748b">ALU + Regs</text>

      <rect x="255" y="0" width="75" height="70" rx="5" fill="#f1f5f9" stroke="#3b82f6" stroke-width="1.5"/>
      <text x="292" y="32" text-anchor="middle" font-size="11" font-weight="700" fill="#1e293b">Core 3</text>
      <text x="292" y="50" text-anchor="middle" font-size="9" fill="#64748b">ALU + Regs</text>
    </g>

    <!-- Large Cache & Control Unit -->
    <rect x="15" y="125" width="330" height="42" rx="5" fill="#f8fafc" stroke="#94a3b8"/>
    <text x="180" y="145" text-anchor="middle" font-size="11" font-weight="600" fill="#334155">Large L3 Cache &amp; Out-of-Order Control Logic</text>
    <text x="180" y="159" text-anchor="middle" font-size="9" fill="#64748b">Branch Predictors • Hardware Prefetchers • MMU</text>

    <!-- Strengths pill -->
    <rect x="15" y="178" width="330" height="58" rx="6" fill="#f0fdf4" stroke="#86efac"/>
    <text x="180" y="198" text-anchor="middle" font-size="11" font-weight="700" fill="#166534">Strengths: Low-Latency Branching</text>
    <text x="180" y="215" text-anchor="middle" font-size="10" fill="#15803d">Sequential logic, irregular algorithms, pointer traversal,</text>
    <text x="180" y="228" text-anchor="middle" font-size="10" fill="#15803d">general OS operations, single-thread clock speed.</text>
  </g>

  <!-- Right: GPU Panel -->
  <g transform="translate(400, 20)">
    <rect width="360" height="250" rx="8" fill="#ffffff" stroke="#cbd5e1" stroke-width="1.5"/>
    <rect width="360" height="32" rx="8" fill="#ecfdf5" stroke="#cbd5e1" stroke-width="1.5"/>
    <text x="180" y="21" text-anchor="middle" font-size="13" font-weight="700" fill="#065f46">GPU: Thousands of Stream Cores (SIMD/SIMT)</text>

    <!-- Dense Core Grid (32 mini cores) -->
    <g transform="translate(15, 45)">
      <!-- Row 1 -->
      <rect x="0" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="42" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="84" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="126" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="168" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="210" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="252" y="0" width="35" height="14" rx="2" fill="#10b981"/><rect x="294" y="0" width="35" height="14" rx="2" fill="#10b981"/>
      <!-- Row 2 -->
      <rect x="0" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="42" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="84" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="126" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="168" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="210" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="252" y="18" width="35" height="14" rx="2" fill="#10b981"/><rect x="294" y="18" width="35" height="14" rx="2" fill="#10b981"/>
      <!-- Row 3 -->
      <rect x="0" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="42" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="84" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="126" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="168" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="210" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="252" y="36" width="35" height="14" rx="2" fill="#10b981"/><rect x="294" y="36" width="35" height="14" rx="2" fill="#10b981"/>
      <!-- Row 4 -->
      <rect x="0" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="42" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="84" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="126" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="168" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="210" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="252" y="54" width="35" height="14" rx="2" fill="#10b981"/><rect x="294" y="54" width="35" height="14" rx="2" fill="#10b981"/>
    </g>

    <!-- Small Cache & Ultra-Wide VRAM Bus -->
    <rect x="15" y="125" width="330" height="42" rx="5" fill="#f8fafc" stroke="#94a3b8"/>
    <text x="180" y="145" text-anchor="middle" font-size="11" font-weight="600" fill="#334155">Compact Cache &amp; High-Bandwidth VRAM Controller</text>
    <text x="180" y="159" text-anchor="middle" font-size="9" fill="#64748b">GDDR6 / HBM3 Memory Bus (&gt; 2,000 GB/s throughput)</text>

    <!-- Strengths pill -->
    <rect x="15" y="178" width="330" height="58" rx="6" fill="#f0fdf4" stroke="#86efac"/>
    <text x="180" y="198" text-anchor="middle" font-size="11" font-weight="700" fill="#166534">Strengths: Massive Data Parallelism</text>
    <text x="180" y="215" text-anchor="middle" font-size="10" fill="#15803d">Dense matrix multiplication, deep learning tensors,</text>
    <text x="180" y="228" text-anchor="middle" font-size="10" fill="#15803d">molecular dynamics, cryo-EM voxel reconstruction.</text>
  </g>
</svg>


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

<!-- Memory & Storage Hierarchy Tiered Pyramid Schematic -->
<svg viewBox="0 0 780 340" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-up" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 9 L 5 0 L 10 9 z" fill="#dc2626" />
    </marker>
    <marker id="arrow-down" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 5 10 L 10 1 z" fill="#2563eb" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="340" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Left Speed Arrow Indicator -->
  <path d="M 45 285 L 45 35" stroke="#dc2626" stroke-width="2.5" marker-end="url(#arrow-up)"/>
  <text x="35" y="160" text-anchor="middle" font-size="11" font-weight="700" fill="#dc2626" transform="rotate(-90 35 160)">SPEED &amp; COST / GB INCREASES</text>

  <!-- Right Capacity Arrow Indicator -->
  <path d="M 735 35 L 735 285" stroke="#2563eb" stroke-width="2.5" marker-end="url(#arrow-down)"/>
  <text x="748" y="160" text-anchor="middle" font-size="11" font-weight="700" fill="#2563eb" transform="rotate(90 748 160)">CAPACITY &amp; LATENCY INCREASES</text>

  <!-- Tier 1: CPU Registers (Narrowest, Top) -->
  <g transform="translate(180, 20)">
    <rect width="420" height="48" rx="6" fill="#fee2e2" stroke="#ef4444" stroke-width="1.5"/>
    <text x="210" y="22" text-anchor="middle" font-size="12" font-weight="700" fill="#991b1b">CPU Registers</text>
    <text x="210" y="38" text-anchor="middle" font-size="10" fill="#7f1d1d">&lt; 1 ns latency • ~few KB • 1 clock cycle (Scaled: 1 sec)</text>
  </g>

  <!-- Tier 2: CPU Caches (L1, L2, L3) -->
  <g transform="translate(150, 76)">
    <rect width="480" height="48" rx="6" fill="#ffedd5" stroke="#f97316" stroke-width="1.5"/>
    <text x="240" y="22" text-anchor="middle" font-size="12" font-weight="700" fill="#9a3412">CPU Caches (L1, L2, L3 on-die)</text>
    <text x="240" y="38" text-anchor="middle" font-size="10" fill="#7c2d12">1 – 15 ns latency • 32 KB – 256 MB • SRAM (Scaled: ~15 sec)</text>
  </g>

  <!-- Tier 3: Main Memory (RAM & VRAM) -->
  <g transform="translate(120, 132)">
    <rect width="540" height="48" rx="6" fill="#fef3c7" stroke="#f59e0b" stroke-width="1.5"/>
    <text x="270" y="22" text-anchor="middle" font-size="12" font-weight="700" fill="#92400e">Main Memory: System RAM &amp; GPU VRAM</text>
    <text x="270" y="38" text-anchor="middle" font-size="10" fill="#78350f">50 – 100 ns latency • 16 GB – 2 TB • DDR4/DDR5 / HBM3 (Scaled: ~1.5 min)</text>
  </g>

  <!-- Tier 4: NVMe Solid State Drives -->
  <g transform="translate(90, 188)">
    <rect width="600" height="48" rx="6" fill="#e0f2fe" stroke="#0ea5e9" stroke-width="1.5"/>
    <text x="300" y="22" text-anchor="middle" font-size="12" font-weight="700" fill="#075985">NVMe PCIe Solid-State Drives (SSDs)</text>
    <text x="300" y="38" text-anchor="middle" font-size="10" fill="#0c4a6e">25 – 100 µs latency • 1 – 30+ TB • NAND Flash (Scaled: 1 – 2 days)</text>
  </g>

  <!-- Tier 5: HDDs & Network Storage (Widest, Bottom) -->
  <g transform="translate(60, 244)">
    <rect width="660" height="48" rx="6" fill="#f1f5f9" stroke="#64748b" stroke-width="1.5"/>
    <text x="330" y="22" text-anchor="middle" font-size="12" font-weight="700" fill="#334155">Rotational Hard Drives (HDDs) &amp; Network Attached Storage (NFS / Lustre)</text>
    <text x="330" y="38" text-anchor="middle" font-size="10" fill="#475569">5 – 10+ ms latency • Petabytes • Magnetic Disks / Cold Storage (Scaled: 2 – 4 months)</text>
  </g>

  <!-- Bottom Summary Callout -->
  <text x="390" y="322" text-anchor="middle" font-size="11" fill="#64748b">
    Accessing data on spinning disk vs. CPU registers is physically equivalent to waiting 4 months vs. 1 second.
  </text>
</svg>


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

