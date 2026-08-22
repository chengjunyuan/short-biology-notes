---
layout: default
title: Software Development Lifecycle & Reproducibility
parent: CS101 Software Engineering
nav_order: 6
---

# Software Development Lifecycle & Reproducibility

The **Software Development Lifecycle (SDLC)** is a framework for managing software development. It is commonly used by many software engineering teams and it is worth taking a look at to better understand how software engineers think.

---

## 1. The SDLC Architecture
Briefly speaking, SDLC starts by setting out the requirements for our software (e.g., must be compatible with Linux and Windows, must use Python 3.11, etc.) and designing the high-level architecture. This is followed by implementation (i.e., writing the code) and testing the code. Once the code has been verified to work, we deploy it online for others to use (typically onto GitHub) and continue maintaining it (i.e., fixing bugs and responding to user requests).

<!-- SDLC Engineering Cycle Schematic -->
<svg viewBox="0 0 780 300" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-blue-sdlc" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#2563eb" />
    </marker>
    <marker id="arrow-green-sdlc" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
    <marker id="arrow-amber-sdlc" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#d97706" />
    </marker>
    <marker id="arrow-purple-sdlc" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#7c3aed" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="300" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Stage 1: Requirements & Design -->
  <g transform="translate(25, 20)">
    <rect width="220" height="110" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="1.5"/>
    <rect width="220" height="24" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="110" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#1d4ed8">1. Requirements &amp; Design</text>
    <text x="12" y="44" font-size="10" fill="#334155">• Define biological scope</text>
    <text x="12" y="62" font-size="10" fill="#334155">• Formulate mathematical models</text>
    <text x="12" y="80" font-size="10" fill="#334155">• Define schemas &amp; data contracts</text>
    <text x="12" y="98" font-size="9" fill="#64748b">• Identify algorithmic bounds</text>
  </g>

  <!-- Arrow 1 -> 2 -->
  <path d="M 245 75 L 275 75" stroke="#2563eb" stroke-width="2" marker-end="url(#arrow-blue-sdlc)"/>
  <text x="260" y="65" text-anchor="middle" font-size="8" font-weight="600" fill="#2563eb">Code</text>

  <!-- Stage 2: Implementation -->
  <g transform="translate(280, 20)">
    <rect width="220" height="110" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="1.5"/>
    <rect width="220" height="24" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
    <text x="110" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#047857">2. Implementation</text>
    <text x="12" y="44" font-size="10" fill="#334155">• Modular Python packages (<tspan font-family="monospace">src/</tspan>)</text>
    <text x="12" y="62" font-size="10" fill="#334155">• Static type hinting (<tspan font-family="monospace">mypy</tspan>)</text>
    <text x="12" y="80" font-size="10" fill="#334155">• Linter &amp; style standards (<tspan font-family="monospace">ruff</tspan>)</text>
    <text x="12" y="98" font-size="9" fill="#059669">• Pin dependencies with <tspan font-family="monospace">uv.lock</tspan></text>
  </g>

  <!-- Arrow 2 -> 3 -->
  <path d="M 500 75 L 530 75" stroke="#059669" stroke-width="2" marker-end="url(#arrow-green-sdlc)"/>
  <text x="515" y="65" text-anchor="middle" font-size="8" font-weight="600" fill="#059669">Test</text>

  <!-- Stage 3: Automated Testing -->
  <g transform="translate(535, 20)">
    <rect width="220" height="110" rx="8" fill="#ffffff" stroke="#f59e0b" stroke-width="1.5"/>
    <rect width="220" height="24" rx="8" fill="#fef3c7" stroke="#f59e0b" stroke-width="1.5"/>
    <text x="110" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#92400e">3. Automated Testing</text>
    <text x="12" y="44" font-size="10" fill="#334155">• Unit &amp; regression tests (<tspan font-family="monospace">pytest</tspan>)</text>
    <text x="12" y="62" font-size="10" fill="#334155">• Negative controls &amp; empty inputs</text>
    <text x="12" y="80" font-size="10" fill="#334155">• Synthetic benchmark datasets</text>
    <text x="12" y="98" font-size="9" fill="#b45309">• Defends against silent bugs</text>
  </g>

  <!-- Arrow 3 -> 4 Downward -->
  <path d="M 645 130 L 645 155" stroke="#d97706" stroke-width="2" marker-end="url(#arrow-amber-sdlc)"/>
  <text x="655" y="146" font-size="8" font-weight="600" fill="#d97706">Push / Pull Request</text>

  <!-- Stage 4: Continuous Integration (CI) -->
  <g transform="translate(280, 160)">
    <rect width="475" height="118" rx="8" fill="#ffffff" stroke="#8b5cf6" stroke-width="1.5"/>
    <rect width="475" height="24" rx="8" fill="#f5f3ff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="237" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#6d28d9">4. Continuous Integration (GitHub Actions CI Runner)</text>
    
    <g transform="translate(15, 32)">
      <rect width="215" height="72" rx="5" fill="#f5f3ff" stroke="#c4b5fd"/>
      <text x="12" y="18" font-size="10" font-weight="700" fill="#5b21b6">Automated Cloud Verification:</text>
      <text x="12" y="34" font-size="9" fill="#334155">• Multi-OS runner matrix</text>
      <text x="12" y="48" font-size="9" fill="#334155">• Runs <tspan font-family="monospace">ruff check</tspan> &amp; <tspan font-family="monospace">mypy</tspan></text>
      <text x="12" y="62" font-size="9" fill="#334155">• Executes entire <tspan font-family="monospace">pytest</tspan> suite</text>

      <rect x="230" y="0" width="215" height="72" rx="5" fill="#ecfdf5" stroke="#86efac"/>
      <text x="242" y="18" font-size="10" font-weight="700" fill="#166534">Merge Quality Gate:</text>
      <text x="242" y="34" font-size="9" fill="#15803d">✓ Test suite 100% green</text>
      <text x="242" y="48" font-size="9" fill="#15803d">✓ Peer code review passed</text>
      <text x="242" y="62" font-size="9" font-weight="600" fill="#047857">✖ Blocks broken commits</text>
    </g>
  </g>

  <!-- Arrow 4 -> 5 Leftward -->
  <path d="M 280 220 L 250 220" stroke="#7c3aed" stroke-width="2" marker-end="url(#arrow-purple-sdlc)"/>
  <text x="265" y="210" text-anchor="middle" font-size="8" font-weight="600" fill="#7c3aed">Pass</text>

  <!-- Stage 5: Deploy & Reproduce -->
  <g transform="translate(25, 160)">
    <rect width="220" height="118" rx="8" fill="#ffffff" stroke="#059669" stroke-width="1.5"/>
    <rect width="220" height="24" rx="8" fill="#ecfdf5" stroke="#059669" stroke-width="1.5"/>
    <text x="110" y="17" text-anchor="middle" font-size="11" font-weight="700" fill="#047857">5. Deploy &amp; Reproduce</text>
    <text x="12" y="44" font-size="10" fill="#334155">• Immutable Containers (Docker)</text>
    <text x="12" y="62" font-size="10" fill="#334155">• Multi-user HPC Singularity</text>
    <text x="12" y="80" font-size="10" fill="#334155">• Zenodo DOI Archival Snapshot</text>
    <text x="12" y="98" font-size="9" font-weight="600" fill="#059669">Permanent 10-year reproducibility</text>
  </g>

  <!-- Arrow 5 -> 1 Upward Feedback Loop -->
  <path d="M 135 160 L 135 135" stroke="#059669" stroke-width="2" marker-end="url(#arrow-green-sdlc)"/>
  <text x="150" y="148" font-size="8" font-weight="600" fill="#059669">Iterate</text>
</svg>

---

## 2. Core Pillars of the SDLC

### A. Automated Unit Testing with `pytest`
When we add new features or scripts to a code base, we always run the risk of breaking an existing piece of code. A **test case** is an input for our code to process and the intended output. A **unit test** is a test case for the smallest functional unit of our code, which is typically an individual function or an individual class (also known as a unit). It is good practice to run automated unit tests every time we update our code to ensure that previously working features continue to work. The example below tests the `calculate_gc_fraction` function using the `pytest` module in Python.

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
Continuous Integration is a useful feature on GitHub that automatically executes your test suite inside isolated cloud virtual machines on every `git push` or Pull Request:

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

### C. Containerization: Docker & HPC Singularity (Apptainer)
To guarantee that a computational pipeline produces identical results ten years after publication, package the entire operating system, compiled bioinformatics binaries (`samtools`, `bwa`), and Python libraries into an immutable container image.
* **Docker**: Industry standard for local development and cloud deployments.
* **Singularity / Apptainer**: Designed specifically for multi-user High-Performance Computing (HPC) clusters where users do not have root administrative permissions.

---

## 3. Engineering Style Comparison

| Development Dimension | Ad-Hoc Scripting | Production SDLC |
| :--- | :--- | :--- |
| **Verification Method** | Eyeballing a single output file | Automated `pytest` suites covering edge cases |
| **Regression Detection**| Discovered months later during paper review | Caught instantly by Continuous Integration (CI) |
| **Portability** | "Works on my laptop" | Deterministic container image (Singularity/Docker)|
| **Longevity** | Broken when dependencies update | Runnable indefinitely via pinned lockfiles |

---

> **Summary**: The Software Development Lifecycle provides the quality control framework for scientific computing. Combining automated `pytest` suites, GitHub Actions CI, structured logging, and containerization ensures research software is verifiable, robust against regression, and permanently reproducible.
