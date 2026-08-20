---
layout: default
title: CS100 Computer Fundamentals
nav_order: 2
has_children: true
---

# CS100: Computer Fundamentals

Computational workflows operate within the constraints of physical hardware and operating system architectures. Understanding the distinction between volatile memory and persistent storage (RAM vs. SSD), the operational characteristics of processors (CPUs vs. GPUs), and the differences across computing form factors—from personal laptops to high-performance server racks—is essential for designing reliable, scalable pipelines.

This course establishes the foundational hardware components, operating system conventions, and remote computing environments necessary for all subsequent computational biology work.

---

## Topics

1. [**Hardware Fundamentals**](hardware.html)
   * The core computation triad: Processors (CPU/GPU), Memory (RAM), and Storage (SSD/HDD)
   * Processors, physical cores, and logical threads (SMT/Hyper-Threading)
   * CPU vs. GPU acceleration realities, VRAM constraints, and hardware evaluation metrics
   * Job resource parameters (`--cpus`, `--mem`, `--gpus`) and failure modes (OOM, swap thrashing)
2. [**Operating Systems & Command-Line Environments**](operating-systems.html)
   * OS architecture, kernel vs. user space, and deployment formats (ISOs, VMs, Containers, WSL2)
   * OS landscape (Linux, macOS/Darwin, Windows NT) and binary compatibility (ELF vs. PE)
   * Terminal emulators vs. shells (`cmd.exe`, PowerShell, Anaconda Prompt, WSL2)
   * Command resolution (`ls`, `pwd`, shell built-ins vs. `$PATH`) and cross-platform pipeline traps
3. [**Working with Remote Servers & HPC**](servers-and-hpc.html)
   * Physical server reality, networking (fiber vs. Wi-Fi), and laptop vs. server decision framework
   * Remote access mechanics via SSH, cryptographic key pairs, and remote IDEs
   * Session persistence and preventing `SIGHUP` terminations with `tmux`
   * Shell environments (`$PATH`, `.bashrc`, `.zshrc`) and batch scheduling with SLURM/PBS

<div class="page-nav">
  <div></div>
  <a href="hardware.html" class="page-nav-next">Next: Hardware Fundamentals &rarr;</a>
</div>
