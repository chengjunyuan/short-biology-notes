---
layout: default
title: Automated Testing & Continuous Integration (CI)
parent: CS211 Software Engineering & OOP
nav_order: 4
---

# Automated Testing & Continuous Integration (CI)

In biology, experimental assays use positive and negative controls to validate results. In software engineering, **Automated Unit Tests** are the positive and negative controls of your codebase.

---

## 1. Why Automated Testing Is Critical in Computational Biology

Bioinformatics software is notorious for **silent bugs**: code that runs to completion without throwing an error, but produces mathematically incorrect variant frequencies or shifted genomic coordinates.

* Manual testing (running your script on one test file and eyeballing the result) only tests the "happy path".
* Automated tests run instantly across hundreds of tricky edge cases every time you make a code change.

---

## 2. Unit Testing with `pytest`

`pytest` is the standard testing framework in Python. Tests are simply Python functions prefixed with `test_` that assert expected outcomes:

```python
# In src/sequence_utils.py
def calculate_gc(seq: str) -> float:
    if not seq:
        return 0.0
    return (seq.count("G") + seq.count("C")) / len(seq)

# In tests/test_sequence_utils.py
import pytest
from src.sequence_utils import calculate_gc

def test_calculate_gc_standard():
    assert calculate_gc("ATGC") == 0.5
    assert calculate_gc("GGCC") == 1.0
    assert calculate_gc("AATT") == 0.0

def test_calculate_gc_empty_string():
    # Edge case: Empty input should not divide by zero!
    assert calculate_gc("") == 0.0

def test_calculate_gc_case_sensitivity():
    # Defensive test: Lowercase characters
    assert calculate_gc("atgc") == 0.5
```

Running `pytest` in the terminal executes all test functions in parallel and reports passing/failing assertions.

---

## 3. Continuous Integration (CI) with GitHub Actions

What prevents someone from accidentally committing broken code to GitHub?

**Continuous Integration (CI)** automatically triggers test suites in an isolated cloud container whenever a commit is pushed or a Pull Request is opened.

```yaml
# .github/workflows/tests.yml
name: Continuous Integration

on: [push, pull_request]

jobs:
  run-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v2
      - run: uv venv && uv pip install -e . pytest
      - run: uv run pytest tests/
```

* If all tests pass: GitHub shows a green checkmark (`✔`), confirming the PR is safe to merge.
* If any test fails: GitHub blocks the merge (`✖`), preventing regressions from entering production.

> **Takeaway**: Automated unit tests are the software equivalent of assay controls. Pair `pytest` with GitHub Actions CI to guarantee that new features never break existing analyses.
