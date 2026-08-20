---
layout: default
title: Package & Environment Management
parent: CS101 Software Engineering
nav_order: 4
---

# Package & Environment Management

One of the most persistent failure modes in computational biology is **dependency drift**: an analysis written with pandas 1.2 and NumPy 1.20 silently fails, crashes, or produces altered numerical results when executed on another machine running newer versions. Scientific code cannot be considered correct or reproducible unless its execution environment is explicitly isolated, version-pinned, and cryptographically locked.

---

## 1. The Environment Isolation Architecture

By default, executing `pip install <package>` installs libraries directly into the operating system's global Python environment. When different research projects require conflicting versions of shared dependencies, global installation triggers catastrophic version collisions.

```
+---------------------------------------------------------------------------------------------------+
|                                     Global Operating System                                       |
|                                                                                                   |
|  +-------------------------------------+                 +-------------------------------------+  |
|  |     Project A: Single-Cell RNA      |                 |     Project B: Structural Docking   |  |
|  |                                     |                 |                                     |  |
|  |  Isolated Virtual Environment (.venv|                 |  Isolated Conda Environment (docking|  |
|  |  +-------------------------------+  |                 |  +-------------------------------+  |  |
|  |  | Python 3.11 Interpreter       |  |                 |  | Python 3.10 Interpreter       |  |  |
|  |  | - site-packages/              |  |                 |  | - site-packages/ (PyRosetta)  |  |  |
|  |  |   - scanpy==1.9.3             |  |                 |  | - C/C++ Shared Libs (libstdc++)|  |
|  |  |   - numpy==1.24.4             |  |                 |  | - Compiled tools (openbabel)  |  |  |
|  |  +-------------------------------+  |                 |  +-------------------------------+  |  |
|  |                  |                  |                 |                  |                  |  |
|  |                  v                  |                 |                  v                  |  |
|  |     Declared via pyproject.toml     |                 |    Declared via environment.yml     |  |
|  |        Locked via uv.lock           |                 |        Locked via conda-lock        |  |
|  +-------------------------------------+                 +-------------------------------------+  |
+---------------------------------------------------------------------------------------------------+
```

An **Isolated Environment** is a self-contained directory tree housing an independent Python binary and a dedicated `site-packages/` directory. When an environment is activated, the shell's `$PATH` variable is prepended with the environment's `bin/` directory, redirecting all package lookups away from global system paths.

---

## 2. Tooling Landscape: `uv` and `conda`

Computational biology workflows depend on two primary classes of software: pure/wheel Python libraries, and non-Python compiled command-line binaries (e.g., C/C++ bioinformatics tools like `samtools` or `bwa`).

### A. Next-Generation Python Packaging: `uv`
Written in Rust, `uv` is an extremely fast package manager that replaces `pip`, `pip-tools`, `virtualenv`, and `poetry` in a single tool.

* **Speed**: Resolves and installs dependency trees 10–100× faster than legacy `pip`.
* **Deterministic Lockfiles**: Generates cross-platform `uv.lock` files containing exact pinned versions and cryptographic checksums for every transitive dependency.
* **Standard Python Project Workflow**:
  ```bash
  # Initialize a new project with a standard pyproject.toml
  uv init my-genomic-project
  cd my-genomic-project

  # Add dependencies and automatically generate/update uv.lock
  uv add biopython pandas scipy

  # Execute a script inside the isolated environment without manual activation
  uv run python src/pipeline.py
  ```

### B. System & Binary Package Management: `conda` / `mamba`
Python-only package managers cannot install non-Python system libraries or compiled C/Fortran command-line binaries. **Conda** (accelerated via `mamba` or `pixi`) manages pre-compiled binary packages across independent channels such as `bioconda` and `conda-forge`.

```bash
# Create an environment specifying Python version and compiled bioinformatics binaries
mamba create -n alignment_env -c bioconda -c conda-forge python=3.11 bwa samtools bedtools

# Activate environment
conda activate alignment_env
```

---

## 3. Package Management Decision Framework

| Criterion / Feature | Legacy `pip` + `venv` | Modern `uv` | Conda / Mamba (`bioconda`) |
| :--- | :--- | :--- | :--- |
| **Primary Domain** | Python-only packages | Python-only applications & libraries | Cross-language & compiled binaries |
| **Resolution Speed** | Slow ($O(N)$ network backtracking) | Ultra-fast (milliseconds, Rust solver) | Moderate to slow (large SAT dependency graphs) |
| **Non-Python Binaries** | ❌ Cannot install (`samtools`, `bwa`) | ❌ Python packages and wheels only | ✅ Installs arbitrary C/C++/R binaries |
| **Deterministic Locking**| ⚠️ Manual `pip freeze` (unresolved) | ✅ Native `uv.lock` with hash validation | ⚠️ Requires `conda-lock` plugin |
| **Best Used For** | Basic legacy scripts | **All standard Python-centric workflows** | **Bioinformatics pipelines requiring CLI tools** |

---

> **Summary**: Managing software environments requires strict isolation and version locking. Use `uv` for lightning-fast, reproducible Python workflows with deterministic lockfiles, and use `conda`/`mamba` when pipelines require pre-compiled non-Python bioinformatics binaries.
