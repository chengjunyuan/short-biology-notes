---
layout: default
title: Introduction to Object-Oriented Programming
parent: CS211 Software Engineering & OOP
nav_order: 1
---

# Introduction to Object-Oriented Programming (OOP)

In pure procedural programming, you pass raw data (like strings or lists) through a series of separate functions. As software grows, tracking which functions can modify which variables becomes messy and bug-prone.

**Object-Oriented Programming (OOP)** is a design paradigm that groups **State (Data)** and **Behavior (Functions)** together into unified entities called **Objects**.

---

## 1. Classes as Blueprints vs. Objects as Instances

Think of a **Class** as a cookie cutter (or a plasmid map) and an **Object** as the actual cookie (or the synthesized plasmid):

```python
class SequenceRecord:
    """A blueprint representing a biological sequence record."""
    
    def __init__(self, identifier: str, sequence: str):
        # Attributes (State)
        self.id = identifier
        self.seq = sequence.upper()
        
    # Methods (Behavior)
    def length(self) -> int:
        return len(self.seq)
    
    def gc_content(self) -> float:
        gc_count = self.seq.count("G") + self.seq.count("C")
        return gc_count / len(self.seq) if self.seq else 0.0

# Creating individual instances (Objects)
gene_a = SequenceRecord("TP53", "ATGCGATCGATCG")
gene_b = SequenceRecord("EGFR", "ATGGCCATTGTA")

print(gene_a.id, gene_a.gc_content())
```

---

## 2. Core Concepts: Encapsulation & Inheritance

### A. Encapsulation (Hiding Internal Mechanics)
Encapsulation means restricting direct access to some of an object's internal components, exposing only clean public methods.
* **Why it matters**: If a user modifies `record.seq` directly to contain invalid characters like `"XYZ"`, downstream analysis crashes. By wrapping modification behind methods or properties, the class can validate data before updating its state.

### B. Inheritance (Reusing & Extending Capabilities)
Inheritance allows a new class to inherit attributes and methods from an existing base class:

```python
class FastqRecord(SequenceRecord):
    """Inherits id and seq from SequenceRecord, but adds quality scores."""
    
    def __init__(self, identifier: str, sequence: str, quality_scores: list[int]):
        super().__init__(identifier, sequence)  # Call parent class initializer
        self.quality = quality_scores
        
    def average_quality(self) -> float:
        return sum(self.quality) / len(self.quality) if self.quality else 0.0
```

`FastqRecord` automatically has access to `.gc_content()` and `.length()` without writing those functions a second time.

> **Takeaway**: OOP is simply a way to model complex domain entities (like genes, alignments, or phylogenetic nodes) by co-locating their data and operations in clear, reusable structures.
