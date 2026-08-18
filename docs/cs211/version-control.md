---
layout: default
title: Version Control with Git & GitHub
parent: CS211 Software Engineering & OOP
nav_order: 2
---

# Version Control with Git & GitHub

Why do we use Git instead of saving copies of our analysis scripts as `analysis_final.py`, `analysis_final_v2.py`, and `analysis_REAL_final.py`?

Version control provides an immutable, time-stamped, cryptographic record of every change made to a project, enabling safe experimentation and seamless collaboration.

---

## 1. The Core Mental Model of Git

Git tracks changes across three distinct areas on your local machine:

```
[ Working Directory ]  ── git add ──>  [ Staging Area (Index) ]  ── git commit ──>  [ Git Repository (.git) ]
  (Files you edit)                       (Snapshot ready to commit)                    (Permanent history)
```

* **Working Directory**: The actual files you see and edit in your IDE.
* **Staging Area**: An intentional snapshot of files you choose to group into the next commit.
* **Commit History**: A directed acyclic graph (DAG) of snapshots. Each commit has a unique SHA-1/SHA-256 hash, an author, a timestamp, and a commit message explaining *why* the change was made.

---

## 2. Branching & Pull Requests: Safe Isolation

### The Power of Branches
Never work directly on your main production branch (`main`). If an experimental feature or parameter tweak breaks the pipeline, the master pipeline remains undamaged.

```
(main)       ───●───────────●──────────●────────── (Production)
                 \                    /
(feat/trim)       ───●──────────●──── (Feature branch)
```

1. Create a feature branch: `git checkout -b feature/adapter-trimming`.
2. Make commits, test thoroughly.
3. Merge back into `main` once verified.

### Pull Requests & Code Review
On GitHub, a **Pull Request (PR)** is a formal request to merge your branch into `main`.
* **Why Code Review Matters**: Having a lab colleague review your code catches algorithmic edge cases, unhandled assumptions, and documentation gaps before results are published.

---

## 3. Resolving Merge Conflicts

When two collaborators edit the same lines of the same file concurrently, Git stops and asks for human intervention:

```
<<<<<<< HEAD (Current Change on main)
min_quality_score = 30
=======
min_quality_score = 25
>>>>>>> feature/lenient-filter (Incoming Change)
```

Git forces the developer to choose the intended logic, remove the conflict markers, and commit the resolved state.

> **Takeaway**: Git is not just backup storage; it is an experimental ledger that enables collaborative peer review, fearless refactoring, and exact historical reproducibility.
