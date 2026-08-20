# Curriculum Style Guide & Pedagogical Framework

*Standard conventions and architectural guidelines for developing and updating bridging curriculum modules (CS, MA, and Applied Bioinformatics).*

---

## 1. Core Pedagogical Philosophy

1. **Problem-First & Rationale-First ("Why Over How")**:
   * Ground every topic in practical practitioner dilemmas rather than abstract trivia or mechanical syntax.
   * *Hardware*: Introduce RAM not merely as a hardware component, but as the indispensable intermediate workspace required because fetching directly from persistent storage stalls CPU execution by orders of magnitude.
   * *Remote Computing*: Introduce `tmux` by first presenting the `SIGHUP` lifecycle failure mode that kills 12-hour jobs when Wi-Fi drops.

2. **Demystify Real-World Decision Frameworks**:
   * Frame topics around decisions practitioners actually make:
     * *Hardware*: What do job resource parameters (`--cpus`, `--mem`, `--gpus`) physically allocate? Why does adding a GPU fail to accelerate CPU-bound software? What metrics define a "good" CPU or GPU?
     * *Operating Systems*: Why do commands behave differently across terminals? What is the architectural difference between `cmd.exe`, PowerShell, and WSL2?
     * *Remote Computing*: When should computation run locally on a laptop vs. on an HPC cluster?

3. **Demystify Technical Vocabulary on First Mention**:
   * Never assume prior systems jargon. Provide immediate, accessible definitions with concrete examples for terms such as:
     * *Bare-metal* vs. *Virtual Machine (Hypervisor)* vs. *Subsystem (WSL2)* vs. *Container*.
     * *Flashing* an OS image to a drive.
     * *Daemons* (silent background service processes, e.g., `sshd`, `tmux`, `cron`).
     * *Nodes* (individual physical computers in a cluster, e.g., Login Node vs. Compute Node).

4. **Historical Grounding for Modern Incompatibilities**:
   * When tools diverge sharply across platforms (e.g. Linux vs. Windows NT), provide a concise historical note explaining *why* the divergence occurred (e.g. Unix's 1970s plain-text stream philosophy and POSIX vs. Windows NT's 1990s object-oriented GUI kernel and Win32 API).

---

## 2. Tone and Voice

* **Tone**: Formal, direct, academic, and rigorous.
* **Eliminate Chatty Filler**:
  * ❌ Avoid conversational openings: *"Welcome to CS100!"*, *"Hey everyone!"*, *"In this fun module..."*
  * ❌ Avoid exclamation marks and casual meta-commentary.
  * ❌ Avoid hand-waving phrases like *"as you probably know"* or *"obviously"*.
* **Emphasize Physical & Algorithmic Mechanisms**: Focus on what happens at the hardware, kernel, network, or memory level.

---

## 3. Course Landing Page Standards (`index.md`)

Course landing pages must follow a clean, consistent format:

```markdown
---
layout: default
title: [Course Code] [Course Title]
nav_order: [N]
has_children: true
---

# [Course Code]: [Course Title]

[1–2 formal paragraphs establishing the physical/mathematical reality, computational scope, and pedagogical purpose of the course.]

---

## Topics

1. [**Topic Title**](topic-filename.html)
   * Concise subtopic 1
   * Concise subtopic 2
   * Concise subtopic 3
2. [**Second Topic Title**](second-topic.html)
   * Concise subtopic 1
   * Concise subtopic 2
```

### Landing Page Rules:
* **Section Heading**: Always use `## Topics` (never "Course Modules", "Syllabus", or "Outline").
* **Subtopic Bullets**: Provide a concise, bulleted list of the actual subject areas covered under each link.
* **No Rationale Headers**: Never write `"Rationale:"` preceding bullet points on landing pages.

---

## 4. Topic Page Architecture (`<topic-name>.md`)

Every topic document must adhere to the following section structure:

```markdown
---
layout: default
title: [Topic Title (Clear Noun Phrase)]
parent: [Course Code] [Course Title]
nav_order: [N]
---

# [Topic Title]

[Opening paragraph framing the topic from first principles.]

---

## 1. [Foundational Triad / Mental Model]
[Deconstruct the core concept into its fundamental components with ASCII/Unicode diagram.]

---

## 2. [Deep Architectural Breakdown & Specifications]
[Structured explanations answering core practitioner questions.]

---

## 3. [Comparison Matrix / Decision Framework]
[Structured Markdown table comparing tools, architectures, or environments.]

---

## 4. [Failure Modes & Practical Remediation]
[Explicit failure outputs (e.g. exit codes, error traces) and exact fix commands.]

---

> **Summary**: [Concise 1–2 sentence synthesis of the topic's architectural takeaway.]
```

---

## 5. Visual and Formatting Standards

### A. System Architecture Diagrams
Use clean, fixed-width ASCII / Unicode box diagrams to illustrate data flows, memory hierarchies, and process relationships:

```
+------------------+         High-Speed Bus        +--------------------+
|  Storage (SSD)   | ----------------------------> | Main Memory (RAM)  |
|  - Non-volatile  |   Loaded into working memory  | - Volatile         |
+------------------+                               +--------------------+
```

### B. Comparison Tables
Organize multidimensional trade-offs using Markdown tables with explicit feature dimensions in the first column:

| Dimension / Metric | Local Laptop | Remote Server / HPC Node |
| :--- | :--- | :--- |
| **Primary Strength** | Interactive coding, plotting | Heavy 24/7 batch processing |
| **Memory Ceiling** | 8–32 GB RAM | 64 GB – 2+ TB RAM |

### C. Failure Modes with Verbatim Errors
Whenever discussing system limits or cross-platform issues, include the exact error message and exit code:
* *OOM Termination*: `Killed: 9` (Exit code 137)
* *GPU VRAM Exhaustion*: `RuntimeError: CUDA out of memory`
* *Line Ending Corruption*: `/bin/bash^M: bad interpreter: No such file or directory`
* *SSH Connection Severed*: `SIGHUP` (Signal 1 - Hangup)

### D. Mathematical & KaTeX Delimiters
* **Inline Math**: Use `$...$` or `\(...\)`.
* **Display Math**: Use `$$...$$` or `\[...\]`.
* **KaTeX Single-Line Convention**: Keep all matrix (`bmatrix`), alignment (`aligned`), and piecewise (`cases`) environments inside Markdown `$$...$$` blocks on single lines without broken `&` tokens to ensure cross-renderer compatibility.

---

## 6. Checklist for Updating Subsequent Courses

When updating courses like CS101, CS201, CS211, MA101, MA201, MA211:

- [ ] **Landing Page (`index.md`)**:
  - [ ] Formal, direct introductory paragraphs.
  - [ ] Heading is `## Topics`.
  - [ ] Bullet points summarize key concepts without conversational fluff or `"Rationale:"` tags.
- [ ] **Topic Pages (`<topic>.md`)**:
  - [ ] Identify the 4–6 core practitioner questions that motivate the page.
  - [ ] Establish foundational mental models and triads first.
  - [ ] Weave answers naturally into narrative prose, tables, and schematics.
  - [ ] Define all technical terminology upon first mention.
  - [ ] Document concrete failure modes with real error signatures and solutions.
  - [ ] Conclude with a clear blockquote `> **Summary**: ...`
- [ ] **Formatting**:
  - [ ] Clean ASCII diagrams for system/data flow.
  - [ ] Structured comparison tables.
  - [ ] Proper KaTeX math formatting.
