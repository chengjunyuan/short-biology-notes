---
layout: default
title: Object-Oriented Programming Fundamentals
parent: CS101 Software Engineering
nav_order: 3
---

# Object-Oriented Programming Fundamentals

In simple procedural scripts, biological data is often stored across disparate primitive types: sequence strings, coordinate tuples, and quality score lists. As analytical pipelines expand, managing relationships between these disconnected variables becomes error-prone. **Object-Oriented Programming (OOP)** addresses this by allowing developers to define **custom data types** that co-locate biological state (data attributes) and domain behavior (methods) into cohesive, self-validating entities.

---

## 1. The Blueprint-to-Instance Architecture

A **Class** serves as a structural blueprint defining what data an entity holds and what operations it can perform. An **Object** is a concrete instance allocated in memory created according to that blueprint.

<!-- Blueprint-to-Instance Architecture Schematic -->
<svg viewBox="0 0 780 290" width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" style="max-width: 780px; display: block; margin: 1.5rem auto; font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;">
  <defs>
    <marker id="arrow-blue-oop" viewBox="0 0 10 10" refX="6" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M 0 1 L 10 5 L 0 9 z" fill="#2563eb" />
    </marker>
  </defs>

  <!-- Background Canvas -->
  <rect width="780" height="290" rx="10" fill="#f8fafc" stroke="#e2e8f0" stroke-width="1.5"/>

  <!-- Class Definition (The Blueprint) -->
  <g transform="translate(25, 18)">
    <rect width="730" height="98" rx="8" fill="#ffffff" stroke="#3b82f6" stroke-width="2"/>
    <rect width="730" height="26" rx="8" fill="#eff6ff" stroke="#3b82f6" stroke-width="2"/>
    <text x="365" y="18" text-anchor="middle" font-size="12" font-weight="700" fill="#1d4ed8">CLASS DEFINITION (The Blueprint): <tspan font-family="monospace">class GenomicVariant</tspan></text>

    <!-- Blueprint Content: State & Methods -->
    <g transform="translate(15, 34)">
      <rect width="340" height="52" rx="4" fill="#f8fafc" stroke="#cbd5e1"/>
      <text x="12" y="18" font-size="10" font-weight="700" fill="#1e293b">Encapsulated State (Attributes &amp; Types):</text>
      <text x="12" y="34" font-size="9" font-family="monospace" fill="#0369a1">chrom: str, pos: int, ref: str, alt: str</text>
      <text x="12" y="46" font-size="9" fill="#64748b">Constructor enforces: <tspan font-family="monospace">pos &gt;= 1</tspan>, valid IUPAC alleles</text>

      <rect x="360" y="0" width="340" height="52" rx="4" fill="#f8fafc" stroke="#cbd5e1"/>
      <text x="372" y="18" font-size="10" font-weight="700" fill="#1e293b">Domain Behavior (Bound Methods):</text>
      <text x="372" y="34" font-size="9" font-family="monospace" fill="#047857">.is_transition() -&gt; bool</text>
      <text x="372" y="46" font-size="9" font-family="monospace" fill="#047857">.is_indel() -&gt; bool, .to_vcf() -&gt; str</text>
    </g>
  </g>

  <!-- Instantiation Flow Indicator -->
  <path d="M 200 116 L 200 142" stroke="#2563eb" stroke-width="2" marker-end="url(#arrow-blue-oop)"/>
  <path d="M 580 116 L 580 142" stroke="#2563eb" stroke-width="2" marker-end="url(#arrow-blue-oop)"/>
  <text x="390" y="132" text-anchor="middle" font-size="10" font-weight="600" fill="#2563eb">Instantiation in Heap Memory (Object Allocation)</text>

  <!-- Instance 1: variant_1 -->
  <g transform="translate(25, 146)">
    <rect width="350" height="126" rx="8" fill="#ffffff" stroke="#10b981" stroke-width="1.5"/>
    <rect width="350" height="26" rx="8" fill="#ecfdf5" stroke="#10b981" stroke-width="1.5"/>
    <text x="175" y="18" text-anchor="middle" font-size="11" font-weight="700" fill="#047857">INSTANCE 1: <tspan font-family="monospace">variant_1</tspan> (Heap: <tspan font-family="monospace">0x7f9a1</tspan>)</text>

    <text x="14" y="46" font-size="10" fill="#334155">• <tspan font-family="monospace">chrom = "chr1"</tspan>, <tspan font-family="monospace">pos = 145028</tspan></text>
    <text x="14" y="64" font-size="10" fill="#334155">• <tspan font-family="monospace">ref = "A"</tspan>, <tspan font-family="monospace">alt = "G"</tspan></text>

    <rect x="12" y="74" width="326" height="42" rx="4" fill="#f0fdf4" stroke="#86efac"/>
    <text x="20" y="92" font-size="10" font-family="monospace" fill="#166534">variant_1.is_transition() ➔ True (A➔G)</text>
    <text x="20" y="106" font-size="9" fill="#15803d">Calculated internally via encapsulated method</text>
  </g>

  <!-- Instance 2: variant_2 -->
  <g transform="translate(405, 146)">
    <rect width="350" height="126" rx="8" fill="#ffffff" stroke="#8b5cf6" stroke-width="1.5"/>
    <rect width="350" height="26" rx="8" fill="#f5f3ff" stroke="#8b5cf6" stroke-width="1.5"/>
    <text x="175" y="18" text-anchor="middle" font-size="11" font-weight="700" fill="#6d28d9">INSTANCE 2: <tspan font-family="monospace">variant_2</tspan> (Heap: <tspan font-family="monospace">0x7f9a2</tspan>)</text>

    <text x="14" y="46" font-size="10" fill="#334155">• <tspan font-family="monospace">chrom = "chr7"</tspan>, <tspan font-family="monospace">pos = 55241708</tspan></text>
    <text x="14" y="64" font-size="10" fill="#334155">• <tspan font-family="monospace">ref = "C"</tspan>, <tspan font-family="monospace">alt = "T"</tspan></text>

    <rect x="12" y="74" width="326" height="42" rx="4" fill="#f5f3ff" stroke="#c4b5fd"/>
    <text x="20" y="92" font-size="10" font-family="monospace" fill="#5b21b6">variant_2.is_transition() ➔ True (C➔T)</text>
    <text x="20" y="106" font-size="9" fill="#6d28d9">Calculated internally via encapsulated method</text>
  </g>
</svg>

### Why Define Custom Data Types?
* **Data Invariant Enforcement**: A plain dictionary `{"chrom": "chr1", "pos": -500}` permits negative coordinates. A class constructor (`__init__`) can validate that `pos >= 1` and that nucleotide strings contain valid IUPAC bases before the object is created.
* **Encapsulated Domain Logic**: Instead of passing raw coordinate variables into external helper functions across multiple scripts, the object itself calculates domain properties (e.g., `variant.length()`, `record.gc_content()`).
* **Semantic Type Safety**: Functions can specify `variant: GenomicVariant` in their type signatures rather than ambiguous type hints like `dict[str, Any]`.

---

## 2. Implementing Domain Models in Python

### A. Defining a Core Domain Class
A standard Python class uses `__init__` to initialize its attributes and validate data integrity:

```python
class GenomicInterval:
    """Represents a 1-based, closed genomic interval."""

    def __init__(self, chrom: str, start: int, end: int):
        if start < 1:
            raise ValueError(f"Start coordinate must be >= 1, got {start}")
        if end < start:
            raise ValueError(f"End coordinate {end} cannot precede start coordinate {start}")
        
        self.chrom = str(chrom)
        self.start = int(start)
        self.end = int(end)

    def length(self) -> int:
        """Returns the base-pair length of the interval."""
        return self.end - self.start + 1

    def overlaps(self, other: "GenomicInterval") -> bool:
        """Checks whether this interval overlaps with another interval on the same chromosome."""
        if self.chrom != other.chrom:
            return False
        return max(self.start, other.start) <= min(self.end, other.end)
```

### B. Lightweight Data Modeling with `@dataclass`
When an entity primarily stores structured data with minimal custom initialization logic, Python's built-in `dataclasses` module automatically generates `__init__`, `__repr__`, and comparison operators (`__eq__`):

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class FastqRecord:
    """Immutable sequencing read record."""
    header: str
    sequence: str
    quality_scores: tuple[int, ...]

    def __post_init__(self) -> None:
        if len(self.sequence) != len(self.quality_scores):
            raise ValueError("Sequence length must match quality score count.")
```

Setting `frozen=True` makes the instance immutable: attributes cannot be modified after instantiation, preventing unexpected state alteration across parallel processing pipelines.

---

## 3. Data Modeling Selection Framework

| Representation Format | Primary Strengths | Limitations | Best Suited For |
| :--- | :--- | :--- | :--- |
| **Raw `dict` / `tuple`** | Zero setup, fast ad-hoc serialization | No schema enforcement, string keys prone to typos | Temporary internal parsing buffers |
| **`typing.NamedTuple`** | Low memory footprint, tuple compatibility | Limited custom method support | Tabular coordinate streams (BED records) |
| **`@dataclass(frozen=True)`**| Concise declaration, auto-generated boilerplate, immutability | Slower instantiation than C-backed tuples | Domain payloads passed across modules |
| **Standard `class`** | Full control over validation, properties, and encapsulation | More verbose boilerplate | Complex biological entities with stateful behaviors |

---

> **Summary**: Object-Oriented Programming enables the creation of custom domain-specific data types. Encapsulating biological state and behavior within classes ensures data validation, prevents silent coordinate corruption, and simplifies pipeline interfaces.
