---
layout: default
title: CS211 Software Engineering & OOP
nav_order: 5
has_children: true
---

# CS211: Software Engineering & Object-Oriented Design

Welcome to **CS211: Software Engineering & OOP**!

Scientific code is often written under tight publication deadlines as one-off scripts. However, unmaintainable scripts lead to unrepeatable analyses, undetected data corruption bugs, and software that breaks as soon as a lab member graduates.

This course teaches you how to transition from writing exploratory scripts to engineering **maintainable, testable, and reproducible scientific software**.

---

## Course Modules

1. [**Introduction to Object-Oriented Programming (OOP)**](oop-intro.html)
   * *Rationale*: Why bundling biological state (attributes) and operations (methods) inside classes prevents data desynchronization and makes complex domain models intuitive.
2. [**Version Control with Git & GitHub**](version-control.html)
   * *Rationale*: Why naming files `script_v2_final_FINAL.py` fails, and how branches, commits, and pull requests enable robust collaboration.
3. [**Clean Code, Style & Static Analysis**](clean-code.html)
   * *Rationale*: Why code readability is a prerequisite for scientific correctness, and how automated linters (`ruff`) and type annotations catch bugs before execution.
4. [**Automated Testing & Continuous Integration (CI)**](testing-and-ci.html)
   * *Rationale*: Why manual testing fails on scientific edge cases, how `pytest` guarantees code integrity, and why CI/CD pipelines run tests automatically on every GitHub push.
5. [**Project Structure & Reproducibility**](reproducibility.html)
   * *Rationale*: How structuring a standard Python package (`pyproject.toml`), logging, and containerization guarantees that your computational analysis remains runnable 5 years from now.
