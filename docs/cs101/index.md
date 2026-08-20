---
layout: default
title: CS101 Software Engineering
nav_order: 3
has_children: true
---

# CS101: Software Engineering

Scientific discovery in computational biology depends on software systems that are robust, testable, and reproducible. Writing code for biological data analysis requires moving beyond one-off interactive commands to engineering modular, maintainable pipelines that can reliably process large-scale datasets, withstand changing upstream dependencies, and support multi-researcher collaboration.

This course establishes the software engineering principles required for computational biology: configuring integrated development environments, structuring data processing pipelines, writing modular and object-oriented code, managing isolated environments, collaborating via version control, and executing projects through the software development lifecycle.

---

## Topics

1. [**Development Environments & The Pipeline Paradigm**](pipeline-and-ide.html)
   * The core programmatic mental model: Data Ingestion $\rightarrow$ State Transformation $\rightarrow$ Results Presentation
   * Modern IDE architecture: VS Code navigation, type hinting, debuggers, and remote SSH
   * Exploratory prototyping, rapid iteration, and artifact generation with Jupyter Notebooks
2. [**Clean Code, Modularity, & Functions**](clean-code-and-functions.html)
   * Code readability as a prerequisite for scientific correctness; PEP 8 naming conventions
   * Functional decomposition: scope isolation, single-responsibility principle, and DRY
   * Type annotations (`mypy`), docstring specifications, and automated linting/formatting (`ruff`)
3. [**Object-Oriented Programming Fundamentals**](oop-fundamentals.html)
   * Objects as custom user-defined data types combining state (attributes) and behavior (methods)
   * Classes as blueprints vs. object instances for biological entities (`SequenceRecord`, `Variant`)
   * Encapsulation, invariant validation, and defensive data modeling
4. [**Package & Environment Management**](package-management.html)
   * Dependency drift and the non-determinism of unpinned scientific software
   * Virtual environment mechanics and interpreter isolation
   * Modern dependency management: `uv` for Python packaging and lockfiles vs. `conda`/`mamba` for compiled binaries
5. [**Version Control & Collaborative Git Workflows**](version-control.html)
   * The core Git mental model: Working Directory, Staging Area, and Commit History (DAG)
   * Branching strategies, GitHub Pull Requests, and collaborative peer code review
   * Merge conflicts, resolution mechanics, and history hygiene
6. [**Software Development Lifecycle & Reproducibility**](sdlc-and-reproducibility.html)
   * The SDLC phases applied to computational biology: Design $\rightarrow$ Implementation $\rightarrow$ Verification $\rightarrow$ Deployment $\rightarrow$ Maintenance
   * Automated unit testing with `pytest` and Continuous Integration (CI) via GitHub Actions
   * Structured logging vs. `print()` statements; containerization with Docker and Singularity
