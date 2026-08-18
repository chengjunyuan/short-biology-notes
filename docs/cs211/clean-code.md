---
layout: default
title: Clean Code, Style & Static Analysis
parent: CS211 Software Engineering & OOP
nav_order: 3
---

# Clean Code, Style & Static Analysis

In academia and industry alike, **code is read far more often than it is written**. Writing clear, maintainable code is not aesthetic perfectionism—it is essential for preventing scientific errors.

---

## 1. Core Principles of Maintainable Code

### A. DRY (Don't Repeat Yourself)
* If you find yourself copying and pasting a 10-line block of code to parse multiple files or normalize expression counts, wrap it into a reusable function or class.
* **Why**: When a bug is discovered in duplicated logic, you must remember to fix it in 15 different places. With a single function, you fix it once.

### B. Meaningful Variable Names
```python
# Bad: Cryptic and prone to misunderstandings
def p(d, c):
    return [x for x in d if x[3] > c]

# Good: Explicit self-documenting code
def filter_high_coverage_intervals(intervals: list[tuple[str, int, int, float]], min_coverage: float) -> list[tuple[str, int, int, float]]:
    return [interval for interval in intervals if interval[3] > min_coverage]
```

---

## 2. Style Standards & PEP 8

Python follows the official **PEP 8** style guide (standardizing 4-space indentation, `snake_case` for functions/variables, `PascalCase` for classes, and line lengths).

Instead of formatting code manually:
* **Formatters (`ruff format`, `black`)**: Automatically reformat code to standard style upon saving.
* **Linters (`ruff check`, `flake8`)**: Statically analyze code to detect unused variables, uninitialized references, and syntax anti-patterns before running the script.

---

## 3. Static Type Checking with Type Annotations (`mypy`)

Python 3 supports type hints. While optional at runtime, type annotations allow static analyzers like `mypy` to catch type mismatch errors before launching expensive compute jobs:

```python
def reverse_complement(sequence: str) -> str:
    complement_map = str.maketrans("ACGTacgt", "TGCAtgca")
    return sequence.translate(complement_map)[::-1]
```

If another function mistakenly passes a list of integers to `reverse_complement`, `mypy` flags the bug instantly in your IDE without needing to run the script.

> **Takeaway**: Use descriptive naming, adhere to PEP 8 with automated formatters (`ruff`), and declare type hints so static tools catch bugs before compute time is wasted.
