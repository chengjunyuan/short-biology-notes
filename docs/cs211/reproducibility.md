---
layout: default
title: Project Structure & Reproducibility
parent: CS211 Software Engineering & OOP
nav_order: 5
---

# Project Structure & Reproducibility

A computational analysis is only useful to the scientific community if other researchers can install, run, and replicate the results without manual debugging.

---

## 1. Standard Python Project Structure (`src` Layout)

A clean project directory separates source code, tests, documentation, and raw data:

```
my-bio-pipeline/
├── data/                  # Raw and processed data (often gitignored)
├── docs/                  # Documentation / markdown notes
├── src/                   # Core Python package source code
│   └── my_pipeline/
│       ├── __init__.py
│       ├── cli.py         # Command-line interface entrypoint
│       └── parser.py      # Core business logic
├── tests/                 # Automated test suites
│   ├── test_cli.py
│   └── test_parser.py
├── .gitignore             # Ignore data/, .venv/, cache/
├── pyproject.toml         # Build system & dependency specifications
└── README.md              # Project overview and usage instructions
```

* **The Rationale for `src/`**: Putting source code under `src/` prevents Python from accidentally importing local uninstalled modules during testing, ensuring tests run against the true installed package.

---

## 2. Structured Logging vs. `print()`

In production scripts, avoid relying on bare `print()` statements:
* `print()` outputs everything to stdout without timestamps, severity levels, or source module context.
* Python’s **`logging`** module allows categorizing messages by severity (`DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`), writing simultaneously to terminal and log files, and disabling debug output in production without editing the code.

```python
import logging

logging.basicConfig(level=logging.INFO, format="%(asctime)s [%(levelname)s] %(message)s")
logging.info("Starting genome indexing...")
logging.warning("Non-standard nucleotide 'N' detected at position 1042.")
```

---

## 3. Containerization Overview: Docker & Singularity

Even with pinned Python dependencies, system-level C libraries and OS kernel differences can cause pipeline discrepancies across machines.
* **Docker / Singularity (Apptainer)** package your entire operating system environment, compiled binaries (`samtools`, `bedtools`), and Python dependencies into an immutable image.
* Singularity is specifically designed for multi-user HPC clusters where users do not have root daemon access.

> **Takeaway**: Organize projects with standard `src/` layouts, use formal logging instead of `print()`, and bundle pipelines into containers when sharing complex multi-tool workflows.
