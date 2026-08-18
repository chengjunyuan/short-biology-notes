---
layout: default
title: Operating Systems & The Command Line
parent: CS100 Computer Fundamentals
nav_order: 2
---

# Operating Systems & The Command Line

Why does almost every computational biology tool specify: *"Requires Linux or macOS"*? Why do tools frequently fail when run on native Windows?

Understanding the underlying operating system differences helps prevent common, hard-to-diagnose errors when running pipelines.

---

## 1. The Operating System Landscape

* **Linux (Ubuntu, CentOS, Rocky Linux, Debian)**: The universal standard for servers, cloud computing, and high-performance computing (HPC) clusters. Nearly all bioinformatics tools are written and tested first for Linux.
* **macOS**: Built on a Unix-like foundation (Darwin/BSD). Most command-line bioinformatics software compiles and runs natively on macOS.
* **Windows**: Historically built on a different kernel architecture (NT) with different file handling and shell conventions. While Windows Subsystem for Linux (WSL2) allows running Linux inside Windows, running bioinformatic pipelines natively on Windows often produces errors.

---

## 2. Why OS Differences Break Bioinformatics Pipelines

### A. Line Endings: `LF` vs. `CRLF`
* **Unix / Linux / macOS**: Uses Line Feed (`\n` or `LF`, ASCII 10) to represent the end of a text line.
* **Windows**: Uses Carriage Return + Line Feed (`\r\n` or `CRLF`, ASCII 13 + ASCII 10).

**The Rationale & Failure Mode**:
When a script or TSV file created on Windows is sent to a Linux server, programs read `\r` as part of the string itself rather than whitespace.
* A file path like `/data/sample1.fastq\r` will fail with `File Not Found`.
* Bash scripts will error with: `/bin/bash^M: bad interpreter: No such file or directory`.
* *Fix*: Tools like `dos2unix` strip out hidden `\r` characters.

### B. File Paths and Separators
* **Unix / Linux / macOS**: Forward slashes (`/`), e.g., `/home/user/project/data.fa`. Root directory is `/`.
* **Windows**: Backslashes (`\`), e.g., `C:\Users\user\project\data.fa`.
* Hardcoding backslashes in Python scripts or pipeline definitions instantly breaks cross-platform compatibility. (Always use standard libraries like `pathlib.Path` or `os.path.join`).

### C. Case Sensitivity
* **Linux**: Case-sensitive. `sample.fasta`, `Sample.fasta`, and `SAMPLE.FASTA` are three completely distinct files.
* **macOS / Windows**: Often case-insensitive (preserving case in UI, but treating `sample.fa` and `SAMPLE.FA` as the same file on disk).
* Scripts written on macOS that inconsistently capitalize file names will fail when deployed to a Linux cluster.

### D. The POSIX Standard & Tooling Ecosystem
Computational biology relies heavily on chaining tools together: output from an aligner streams directly into a sorter, which streams into a variant caller without saving massive intermediate files to disk.

This philosophy—treating text streams as standard input/output (`stdin`/`stdout`) across modular command-line utilities—is the core design principle of Unix/Linux (the POSIX standard). Windows shells (`cmd.exe`, PowerShell) have different piping semantics and object models.

> **Takeaway**: When working on Windows, develop inside **WSL2 (Windows Subsystem for Linux)**. Ensure all files use Unix line endings (`LF`) and UTF-8 encoding.
