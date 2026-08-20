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

```
+-----------------------------------------------------------------------------------+
|                        CLASS DEFINITION (The Blueprint)                           |
|                                                                                   |
|  class GenomicVariant:                                                            |
|      - Attributes (State):  chrom: str, pos: int, ref: str, alt: str              |
|      - Methods (Behavior):  is_transition(), is_indel(), to_vcf_record()          |
+-----------------------------------------------------------------------------------+
                                         |
                                         | Instantiation in Memory
                     +-------------------+-------------------+
                     |                                       |
                     v                                       v
+----------------------------------------+   +--------------------------------------+
|  INSTANCE: variant_1 (0x7f9a1)         |   |  INSTANCE: variant_2 (0x7f9a2)       |
|  - chrom: "chr1"                       |   |  - chrom: "chr7"                     |
|  - pos:   145028                       |   |  - pos:   55241708                   |
|  - ref:   "A"                          |   |  - ref:   "C"                        |
|  - alt:   "G"                          |   |  - alt:   "T"                        |
|  - Calls: variant_1.is_transition()   |   |  - Calls: variant_2.is_transition()  |
|           -> True                      |   |           -> True                    |
+----------------------------------------+   +--------------------------------------+
```

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
