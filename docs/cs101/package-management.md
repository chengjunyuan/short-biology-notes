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

<!-- Environment Isolation Architecture Schematic -->
<svg viewBox="0 0 780 290" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <!-- Background Canvas: Global OS -->
  <rect width="780" height="290" rx="10" fill="#f8fafc" stroke="#64748b" stroke-width="1.5"/>
  <rect width="780" height="28" rx="10" fill="#334155"/>
  <text x="20" y="19" font-size="12" font-weight="700" fill="#ffffff">Global Operating System &amp; System Python Environment</text>
  <text x="760" y="19" text-anchor="end" font-size="10" font-weight="600" fill="#fca5a5">⚠️ Avoid global pip install</text>

  <!-- Left: Project A (.venv / uv) -->
  <g transform="translate(25, 42)">
    <rect width="350" height="232" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="1.5"/>
    <rect width="350" height="26" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="175" y="18" text-anchor="middle" font-size="12" font-weight="700" fill="#1d4ed8">Project A: Single-Cell RNA (<tspan font-family="monospace">uv</tspan>)</text>

    <!-- Inner Env Details -->
    <g transform="translate(15, 34)">
      <rect width="320" height="110" rx="5" fill="#f8fafc" stroke="#cbd5e1"/>
      <text x="12" y="20" font-size="10" font-weight="700" fill="#1e293b">Isolated Virtual Environment (<tspan font-family="monospace" fill="#2563eb">.venv/</tspan>):</text>
      <text x="12" y="38" font-size="10" fill="#334155">• Python 3.11 Interpreter</text>
      <text x="12" y="56" font-size="10" fill="#334155">• <tspan font-family="monospace">site-packages/</tspan>:</text>
      <text x="24" y="74" font-size="9" font-family="monospace" fill="#0369a1">- scanpy==1.9.3, numpy==1.24.4</text>
      <text x="24" y="90" font-size="9" font-family="monospace" fill="#0369a1">- scipy==1.11.2, matplotlib==3.7.1</text>

      <!-- Manifest & Lockfile -->
      <g transform="translate(0, 116)">
        <rect width="320" height="34" rx="4" fill="#eff6ff" stroke="#93c5fd"/>
        <text x="160" y="15" text-anchor="middle" font-size="10" font-weight="600" fill="#1e40af">Declared: <tspan font-family="monospace">pyproject.toml</tspan></text>
        <text x="160" y="28" text-anchor="middle" font-size="9" font-weight="700" fill="#2563eb">Locked &amp; Hashed: <tspan font-family="monospace">uv.lock</tspan></text>
      </g>

      <!-- Bottom badge -->
      <rect x="0" y="156" width="320" height="28" rx="4" fill="#dbeafe"/>
      <text x="160" y="174" text-anchor="middle" font-size="10" font-weight="700" fill="#1e40af">Ultra-Fast Rust Dependency Solver</text>
    </g>
  </g>

  <!-- Right: Project B (Conda / Mamba) -->
  <g transform="translate(405, 42)">
    <rect width="350" height="232" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="1.5"/>
    <rect width="350" height="26" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
    <text x="175" y="18" text-anchor="middle" font-size="12" font-weight="700" fill="#047857">Project B: Structural Docking (<tspan font-family="monospace">conda</tspan>)</text>

    <!-- Inner Env Details -->
    <g transform="translate(15, 34)">
      <rect width="320" height="110" rx="5" fill="#f8fafc" stroke="#cbd5e1"/>
      <text x="12" y="20" font-size="10" font-weight="700" fill="#1e293b">Isolated Conda Environment (<tspan font-family="monospace" fill="#059669">docking_env/</tspan>):</text>
      <text x="12" y="38" font-size="10" fill="#334155">• Python 3.10 Interpreter</text>
      <text x="12" y="56" font-size="10" fill="#334155">• Compiled C/C++ Bioinformatics Tools:</text>
      <text x="24" y="74" font-size="9" font-family="monospace" fill="#047857">- bwa==0.7.17, samtools==1.19</text>
      <text x="24" y="90" font-size="9" font-family="monospace" fill="#047857">- pyrosetta==2023.15, libstdc++</text>

      <!-- Manifest & Lockfile -->
      <g transform="translate(0, 116)">
        <rect width="320" height="34" rx="4" fill="#ecfdf5" stroke="#86efac"/>
        <text x="160" y="15" text-anchor="middle" font-size="10" font-weight="600" fill="#166534">Declared: <tspan font-family="monospace">environment.yml</tspan></text>
        <text x="160" y="28" text-anchor="middle" font-size="9" font-weight="700" fill="#059669">Locked: <tspan font-family="monospace">conda-lock.yml</tspan></text>
      </g>

      <!-- Bottom badge -->
      <rect x="0" y="156" width="320" height="28" rx="4" fill="#d1fae5"/>
      <text x="160" y="174" text-anchor="middle" font-size="10" font-weight="700" fill="#065f46">Manages Non-Python System Binaries</text>
    </g>
  </g>
</svg>

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
