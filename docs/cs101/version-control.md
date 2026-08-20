---
layout: default
title: Version Control & Collaborative Git Workflows
parent: CS101 Software Engineering
nav_order: 5
---

# Version Control & Collaborative Git Workflows

Scientific research is inherently iterative and collaborative. Relying on ad-hoc versioning strategies—such as naming files `pipeline_v2_final.py` or syncing folders over cloud storage—inevitably results in overwritten data, untraceable modifications, and irreversible loss of working code. **Git** is a distributed version control system that provides a cryptographic ledger of every modification made to a codebase, while platforms like **GitHub** coordinate peer code review and collaborative pipeline development.

---

## 1. The Core Git Architecture: Three Local States and Remotes

Git does not track files as diff deltas over time; it models the project as a **Directed Acyclic Graph (DAG)** of immutable snapshots (commits).

```
+---------------------------------------------------------------------------------------------------+
|                                          LOCAL WORKSPACE                                          |
|                                                                                                   |
|  +-----------------------+     git add      +----------------------+    git commit   +---------+  |
|  |   Working Directory   | ---------------> | Staging Area (Index) | --------------> |  Local  |  |
|  | - Files currently in  |                  | - Explicit subset of |                 |  Repo   |  |
|  |   active editing state|                  |   files staged for   |                 |  (.git) |  |
|  +-----------------------+                  |   the next snapshot  |                 +---------+  |
|                                             +----------------------+                      |       |
|                                                                                           |       |
+-------------------------------------------------------------------------------------------|-------+
                                                                                            |
                                                                        git push / git pull |
                                                                                            v
                                                                             +---------------------+
                                                                             |   REMOTE (GitHub)   |
                                                                             | - origin/main       |
                                                                             | - Pull Requests     |
                                                                             | - Code Reviews      |
                                                                             +---------------------+
```

1. **Working Directory**: The actual files unpacked on your file system that you edit in your IDE.
2. **Staging Area (Index)**: An intentional intermediate holding area where specific file changes are gathered before creating a permanent snapshot.
3. **Local Repository (`.git/`)**: The local database containing all committed snapshots, branch pointers, and commit metadata.
4. **Remote Repository (GitHub)**: The central cloud mirror that allows distributed team members to synchronize histories, propose changes, and conduct code reviews.

---

## 2. Collaborative Workflows: Branches and Pull Requests

Collaborative teams never commit experimental or unreviewed code directly to the primary production branch (`main`).

```
(main)          ───●────────────────────────────●────────────────────────●─── (Stable Production)
                    \                          /  (Pull Request Merged)
(feat/vcf-parser)    ───●──────────●──────────● (Feature Branch)
                        Commit 1   Commit 2   Code Review Passed
```

### A. Feature Branch Isolation
1. **Branch Creation**: Create a lightweight branch isolated from `main`:
   ```bash
   git checkout -b feature/filter-low-coverage
   ```
2. **Atomic Commits**: Stage and commit related changes with descriptive imperative messages:
   ```bash
   git add src/filters.py tests/test_filters.py
   git commit -m "Add minimum depth filtering to VCF parser"
   ```
3. **Publish to Remote**: Push the local branch to GitHub:
   ```bash
   git push -u origin feature/filter-low-coverage
   ```

### B. The Pull Request (PR) & Scientific Code Review
A **Pull Request** is a formal proposal on GitHub to merge code from a feature branch into `main`. In computational biology, PRs serve as an essential peer review mechanism:
* **Automated Checks**: Continuous integration test suites run automatically against the PR code.
* **Peer Inspection**: Collaborators inspect the exact line-by-line diff, verify mathematical logic, and request corrections before changes enter the master pipeline.

---

## 3. Version Control Strategy Decision Framework

| Strategy / Tool | Ad-Hoc File Duplication (`_v2.py`) | Cloud Storage Sync (Dropbox / Drive) | Distributed Version Control (Git + GitHub) |
| :--- | :--- | :--- | :--- |
| **History Tracking** | Unstructured, manual, and error-prone | File version restore without change rationale | Cryptographic SHA-1/SHA-256 DAG snapshots |
| **Concurrent Editing** | High risk of silent overwrites | Silent conflict copies (`filename (1).py`) | Deterministic branch merges and conflict alerts |
| **Code Review Model** | None | None | Granular line-by-line PR reviews |
| **Reproducibility** | Zero audit trail | Poor (timestamps easily lost) | Exact commit hashes linkable to publications |

---

> **Summary**: Git provides a deterministic cryptographic ledger for tracking software evolution. Using isolated feature branches, descriptive commit histories, and GitHub Pull Requests protects production code and enables rigorous scientific peer review.
