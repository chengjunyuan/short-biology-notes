---
layout: default
title: Software Development Lifecycle & Reproducibility
parent: CS101 Software Engineering
nav_order: 6
---

# Software Development Lifecycle & Reproducibility

In experimental life sciences, standard operating protocols require positive controls, negative controls, and validated assay steps to ensure findings are sound. In computational biology, the **Software Development Lifecycle (SDLC)** is the engineering counterpart to laboratory quality control. Applying structured development practices prevents the silent computational errors, undocumented parameter changes, and unreproducible pipelines that undermine scientific publications.

---

## 1. The Scientific SDLC Architecture

The scientific software development lifecycle is an iterative engineering loop designed to transform research hypotheses into resilient, long-term computational assets.

```
+---------------------------------------------------------------------------------------------------+
|                              THE SCIENTIFIC SDLC ENGINEERING CYCLE                                |
|                                                                                                   |
|  +---------------------------+       Write Code       +------------------------------------+      |
|  | 1. REQUIREMENTS & DESIGN  | ---------------------> |        2. IMPLEMENTATION           |      |
|  | - Define biological scope |                        | - Modular, typed Python functions  |      |
|  | - Mathematical algorithms |                        | - Standard project layout (src/)   |      |
|  +---------------------------+                        +------------------------------------+      |
|               ^                                                         |                         |
|               |                                                         v Run Local Tests         |
|  +---------------------------+                        +------------------------------------+      |
|  | 5. DEPLOY & REPRODUCE     |                        |         3. AUTOMATED TESTING       |      |
|  | - Singularity / Docker    |                        | - pytest unit & edge case tests    |      |
|  | - Zenodo DOI snapshot     |                        | - Negative & synthetic controls    |      |
|  +---------------------------+                        +------------------------------------+      |
|               ^                                                         |                         |
|               | Pass Code Review & Tests                                v Push / Pull Request     |
|               +---------------------------------------------------------+                         |
|                                4. CONTINUOUS INTEGRATION (CI)                                     |
|                                - GitHub Actions automated runner                                  |
|                                - Multi-OS environment matrix testing                              |
+---------------------------------------------------------------------------------------------------+
```

---

## 2. Core Pillars of the Scientific SDLC

### A. Automated Unit Testing with `pytest`
Bioinformatics pipelines are prone to **silent logical failures**: code that finishes with exit code `0` but produces mathematically incorrect variant calls or misaligned genomic coordinates.

Automated unit tests act as computational control assays:

```python
# src/bio_utils.py
def calculate_gc_fraction(sequence: str) -> float:
    """Calculates GC content fraction (0.0 to 1.0) of a nucleotide sequence."""
    if not sequence:
        return 0.0
    seq_upper = sequence.upper()
    gc_count = seq_upper.count("G") + seq_upper.count("C")
    return gc_count / len(seq_upper)

# tests/test_bio_utils.py
import pytest
from bio_utils import calculate_gc_fraction

def test_gc_fraction_standard():
    assert calculate_gc_fraction("ATGC") == pytest.approx(0.5)
    assert calculate_gc_fraction("GGCC") == pytest.approx(1.0)

def test_gc_fraction_empty_string():
    # Negative control: ensure zero-division error is gracefully handled
    assert calculate_gc_fraction("") == 0.0

def test_gc_fraction_case_insensitivity():
    # Defensive check: handles lowercase inputs correctly
    assert calculate_gc_fraction("atgc") == pytest.approx(0.5)
```

### B. Continuous Integration (CI) with GitHub Actions
Continuous Integration automatically executes your test suite inside isolated cloud virtual machines on every `git push` or Pull Request:

```yaml
# .github/workflows/ci.yml
name: Pipeline CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v2
      - run: uv venv && uv pip install -e . pytest
      - run: uv run pytest tests/
```

If a code change introduces a regression or breaks a test, the CI job fails immediately with a red indicator (`✖`), blocking faulty code from merging into production.

### C. Production Logging vs. `print()`
Avoid relying on unformatted `print()` statements in production pipelines. Python's built-in `logging` module provides structured, timestamped logs with granular severity levels (`DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`):

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s"
)
logger = logging.getLogger("alignment_pipeline")

logger.info("Ingesting FASTQ records from sample_01.fastq...")
logger.warning("Low quality score (Q < 15) observed at cycle 142.")
```

### D. Containerization: Docker & HPC Singularity (Apptainer)
To guarantee that a computational pipeline produces identical results ten years after publication, package the entire operating system, compiled bioinformatics binaries (`samtools`, `bwa`), and Python libraries into an immutable container image.
* **Docker**: Industry standard for local development and cloud deployments.
* **Singularity / Apptainer**: Designed specifically for multi-user High-Performance Computing (HPC) clusters where users do not have root administrative permissions.

---

## 3. Engineering Paradigm Comparison

| Development Dimension | Ad-Hoc Scripting Paradigm | Production Scientific SDLC Paradigm |
| :--- | :--- | :--- |
| **Verification Method** | Eyeballing a single output file | Automated `pytest` suites covering edge cases |
| **Regression Detection**| Discovered months later during paper review | Caught instantly by Continuous Integration (CI) |
| **Execution Logging** | Sparse `print()` statements to terminal | Timestamped, level-filtered structured logging |
| **Portability** | "Works on my laptop" | Deterministic container image (Singularity/Docker)|
| **Longevity** | Broken when dependencies update | Runnable indefinitely via pinned lockfiles |

---

> **Summary**: The Software Development Lifecycle provides the quality control framework for scientific computing. Combining automated `pytest` suites, GitHub Actions CI, structured logging, and containerization ensures research software is verifiable, robust against regression, and permanently reproducible.
