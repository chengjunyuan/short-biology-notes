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

<!-- Core Git Architecture Schematic -->
<svg viewBox="0 0 780 270" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-blue-git" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#2563eb" />
    </marker>
    <marker id="arrow-purple-git" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#7c3aed" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="270" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Left: Local Workspace -->
  <g transform="translate(20, 18)">
    <rect width="490" height="234" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="1.5"/>
    <rect width="490" height="26" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
    <text x="245" y="18" text-anchor="middle" font-size="12" font-weight="700" fill="#1d4ed8">Local Developer Workspace</text>

    <!-- State 1: Working Directory -->
    <g transform="translate(5, 36)">
      <rect width="135" height="155" rx="6" fill="#f8fafc" stroke="#94a3b8"/>
      <rect width="135" height="24" rx="6" fill="#f1f5f9"/>
      <text x="67" y="16" text-anchor="middle" font-size="10" font-weight="700" fill="#1e293b">1. Working Tree</text>
      <text x="10" y="44" font-size="10" fill="#334155">• Active files on disk</text>
      <text x="10" y="62" font-size="10" fill="#334155">• Unstaged edits</text>
      <text x="10" y="80" font-size="10" fill="#334155">• In IDE editor</text>
      <rect x="8" y="125" width="119" height="20" rx="3" fill="#e2e8f0"/>
      <text x="67" y="139" text-anchor="middle" font-size="9" font-weight="600" fill="#475569">Raw Disk State</text>
    </g>

    <!-- Arrow: git add -->
    <path d="M 143 110 L 173 110" stroke="#2563eb" stroke-width="2" marker-end="url(#arrow-blue-git)"/>
    <text x="160" y="100" text-anchor="middle" font-size="9" font-family="monospace" font-weight="700" fill="#2563eb">git add</text>

    <!-- State 2: Staging Area -->
    <g transform="translate(180, 36)">
      <rect width="135" height="155" rx="6" fill="#eff6ff" stroke="#3b82f6" stroke-width="1.5"/>
      <rect width="135" height="24" rx="6" fill="#dbeafe"/>
      <text x="67" y="16" text-anchor="middle" font-size="10" font-weight="700" fill="#1e40af">2. Staging (Index)</text>
      <text x="10" y="44" font-size="10" fill="#1e293b">• Curated payload</text>
      <text x="10" y="62" font-size="10" fill="#1e293b">• Explicit file subset</text>
      <text x="10" y="80" font-size="10" fill="#1e293b">• Next snapshot</text>
      <rect x="8" y="125" width="119" height="20" rx="3" fill="#bfdbfe"/>
      <text x="67" y="139" text-anchor="middle" font-size="9" font-weight="700" fill="#1d4ed8">Draft Snapshot</text>
    </g>

    <!-- Arrow: git commit -->
    <path d="M 318 110 L 348 110" stroke="#2563eb" stroke-width="2" marker-end="url(#arrow-blue-git)"/>
    <text x="335" y="100" text-anchor="middle" font-size="8" font-family="monospace" font-weight="700" fill="#2563eb">commit</text>

    <!-- State 3: Local Repo (.git) -->
    <g transform="translate(355, 36)">
      <rect width="130" height="155" rx="6" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
      <rect width="130" height="24" rx="6" fill="#d1fae5"/>
      <text x="65" y="16" text-anchor="middle" font-size="10" font-weight="700" fill="#065f46">3. Local Repo (.git)</text>
      <text x="10" y="44" font-size="10" fill="#064e3b">• Permanent DAG</text>
      <text x="10" y="62" font-size="10" fill="#064e3b">• SHA-1 / SHA-256</text>
      <text x="10" y="80" font-size="10" fill="#064e3b">• Immutable commits</text>
      <rect x="8" y="125" width="114" height="20" rx="3" fill="#a7f3d0"/>
      <text x="65" y="139" text-anchor="middle" font-size="9" font-weight="700" fill="#047857">Local History</text>
    </g>

    <text x="245" y="218" text-anchor="middle" font-size="10" fill="#64748b">Local operations require zero internet connection</text>
  </g>

  <!-- Network Sync Arrows -->
  <path d="M 515 90 L 544 90" stroke="#7c3aed" stroke-width="2" marker-end="url(#arrow-purple-git)"/>
  <text x="530" y="80" text-anchor="middle" font-size="8" font-family="monospace" font-weight="700" fill="#7c3aed">push</text>

  <path d="M 546 140 L 516 140" stroke="#7c3aed" stroke-width="2" marker-end="url(#arrow-purple-git)"/>
  <text x="530" y="155" text-anchor="middle" font-size="8" font-family="monospace" font-weight="700" fill="#7c3aed">pull</text>

  <!-- Right: Remote Repository (GitHub) -->
  <g transform="translate(550, 18)">
    <rect width="210" height="234" rx="8" fill="#ffffff" stroke="#8b5cf6" stroke-width="1.5"/>
    <rect width="210" height="26" rx="8" fill="#f5f3ff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="105" y="18" text-anchor="middle" font-size="12" font-weight="700" fill="#6d28d9">Remote (GitHub Cloud)</text>

    <text x="14" y="52" font-size="11" font-weight="600" fill="#334155">• <tspan font-family="monospace">origin/main</tspan> upstream</text>
    <text x="14" y="74" font-size="11" fill="#475569">• Pull Requests (PRs)</text>
    <text x="14" y="96" font-size="11" fill="#475569">• Peer Code Review</text>
    <text x="14" y="118" font-size="11" fill="#475569">• Automated CI Test Runners</text>

    <rect x="12" y="145" width="186" height="65" rx="5" fill="#ede9fe"/>
    <text x="105" y="165" text-anchor="middle" font-size="10" font-weight="700" fill="#5b21b6">Central Collaboration Hub</text>
    <text x="105" y="182" text-anchor="middle" font-size="9" fill="#6d28d9">Coordinates team sync &amp;</text>
    <text x="105" y="196" text-anchor="middle" font-size="9" fill="#6d28d9">publication snapshot DOIs</text>
  </g>
</svg>

1. **Working Directory**: The actual files unpacked on your file system that you edit in your IDE.
2. **Staging Area (Index)**: An intentional intermediate holding area where specific file changes are gathered before creating a permanent snapshot.
3. **Local Repository (`.git/`)**: The local database containing all committed snapshots, branch pointers, and commit metadata.
4. **Remote Repository (GitHub)**: The central cloud mirror that allows distributed team members to synchronize histories, propose changes, and conduct code reviews.

---

## 2. Collaborative Workflows: Branches and Pull Requests

Collaborative teams never commit experimental or unreviewed code directly to the primary production branch (`main`).

<!-- Collaborative Git Branching and Pull Request Schematic -->
<svg viewBox="0 0 780 230" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-slate-branch" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#475569" />
    </marker>
    <marker id="arrow-green-branch" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#059669" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="230" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Main Branch Line -->
  <line x1="80" y1="60" x2="720" y2="60" stroke="#334155" stroke-width="3"/>
  <rect x="15" y="46" width="60" height="26" rx="4" fill="#334155"/>
  <text x="45" y="63" text-anchor="middle" font-size="11" font-weight="700" fill="#ffffff">main</text>

  <!-- Commit 1 on Main -->
  <circle cx="120" cy="60" r="10" fill="#3b82f6" stroke="#1e40af" stroke-width="2"/>
  <text x="120" y="42" text-anchor="middle" font-size="10" font-weight="600" fill="#1e293b">v1.0.0</text>

  <!-- Branch Divergence Line -->
  <path d="M 120 60 Q 170 140 220 140" fill="none" stroke="#059669" stroke-width="2.5" stroke-dasharray="4,2"/>

  <!-- Feature Branch Line -->
  <line x1="220" y1="140" x2="560" y2="140" stroke="#059669" stroke-width="2.5"/>
  <rect x="15" y="126" width="180" height="26" rx="4" fill="#059669"/>
  <text x="105" y="143" text-anchor="middle" font-size="10" font-weight="700" fill="#ffffff">feat/filter-low-coverage</text>

  <!-- Feature Commit 1 -->
  <circle cx="270" cy="140" r="9" fill="#10b981" stroke="#047857" stroke-width="2"/>
  <text x="270" y="168" text-anchor="middle" font-size="9" font-weight="600" fill="#065f46">Commit 1</text>
  <text x="270" y="180" text-anchor="middle" font-size="8" fill="#64748b">VCF filter logic</text>

  <!-- Feature Commit 2 -->
  <circle cx="400" cy="140" r="9" fill="#10b981" stroke="#047857" stroke-width="2"/>
  <text x="400" y="168" text-anchor="middle" font-size="9" font-weight="600" fill="#065f46">Commit 2</text>
  <text x="400" y="180" text-anchor="middle" font-size="8" fill="#64748b">Add pytest suite</text>

  <!-- PR & Review Milestone on Feature Branch -->
  <g transform="translate(480, 115)">
    <rect width="130" height="50" rx="5" fill="#ffffff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="65" y="18" text-anchor="middle" font-size="10" font-weight="700" fill="#6d28d9">Pull Request (PR)</text>
    <text x="65" y="32" text-anchor="middle" font-size="9" fill="#059669">✓ CI Tests Pass</text>
    <text x="65" y="44" text-anchor="middle" font-size="9" fill="#475569">✓ Peer Review Approved</text>
  </g>

  <!-- Merge Back to Main -->
  <path d="M 610 140 Q 640 100 660 60" fill="none" stroke="#059669" stroke-width="2.5" marker-end="url(#arrow-green-branch)"/>

  <!-- Merge Commit on Main -->
  <circle cx="660" cy="60" r="10" fill="#8b5cf6" stroke="#5b21b6" stroke-width="2"/>
  <text x="660" y="42" text-anchor="middle" font-size="10" font-weight="700" fill="#5b21b6">Merged (v1.1.0)</text>

  <!-- Production Label -->
  <text x="715" y="85" text-anchor="end" font-size="10" font-weight="600" fill="#64748b">Protected Stable Production</text>
</svg>

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
