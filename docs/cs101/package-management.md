---
layout: default
title: Packages & Environment Management
parent: CS101 Programming Fundamentals
nav_order: 4
---

# Packages & Environment Management

One of the most frequent reasons a published computational biology paper cannot be replicated is **dependency drift**: code written for Python 3.8 with pandas 1.2 silently fails or gives different numerical results when run with Python 3.12 and pandas 2.2.

To prevent this, computational biologists isolate environments and pin dependencies.

---

## 1. Why Virtual Environments Are Mandatory

By default, installing a package via `pip install <pkg>` installs it into your global system Python directory.
* **The "Dependency Hell" Problem**:
  * Project A requires `biopython==1.78` and `numpy==1.21`.
  * Project B requires `scanpy` which depends on `numpy>=1.24`.
  * Upgrading numpy globally breaks Project A.
* **The Solution**: An **isolated virtual environment** is simply a self-contained directory containing its own Python interpreter and dedicated `site-packages` directory. Each project gets its own environment.

---

## 2. The Tooling Landscape: `pip`, `conda`, and `uv`

### A. Conda / Mamba (Bioconda)
* **Strengths**: Manages non-Python binary dependencies (C/C++ libraries, compiled tools like `samtools`, `bedtools`, `bwa`, and R packages).
* **When to use**: Installing bioinformatics software suites and command-line tools from the `bioconda` and `conda-forge` channels.
* **Weakness**: Environment solving in standard Conda can be slow (mitigated by `mamba` or `pixi`).

### B. Modern Python Packaging: `uv`
* **What it is**: An extremely fast, next-generation Python package and project manager written in Rust.
* **Why it is replacing legacy `pip`/`venv`**:
  * **10–100x Faster**: Resolves and installs complex Python dependency trees in milliseconds.
  * **Unified Tool**: Replaces `pip`, `pip-tools`, `virtualenv`, and `poetry` in a single binary.
  * **Automatic Python version management**: Can download and manage specific Python versions (e.g., `uv python install 3.11`) without needing root privileges.
* **Basic `uv` Workflow**:
  ```bash
  # Create and activate a virtual environment instantly
  uv venv my_project_env
  source my_project_env/bin/activate

  # Install packages at high speed
  uv pip install biopython pandas scipy
  ```

---

## 3. Ensuring Reproducibility: Declaring Dependencies

For peer-reviewed research and collaborative pipelines, your codebase must declare exact versions:

* **`pyproject.toml`**: The modern PEP 621 standard for defining project metadata and required dependencies.
* **Lockfiles (`uv.lock` or `conda-lock.yml`)**: An exact, frozen snapshot of every transitive dependency and its cryptographic hash. When a colleague runs `uv sync`, they get an exact byte-for-byte duplicate of your working environment.

> **Takeaway**: Never install packages directly into your global system Python. Use `conda`/`mamba` when you need compiled bioinformatics binaries (`samtools`, `bwa`), and use `uv` for lightning-fast, reproducible Python-centric projects.
