---
layout: default
title: Reading, Writing, & Parsing Biological Data
parent: CS101 Programming Fundamentals
nav_order: 3
---

# Reading, Writing, & Parsing Biological Data

Most bioinformatics workflows start with standard file formats like **FASTA** (sequences), **FASTQ** (sequences + quality scores), **BED/GFF** (genomic intervals), or **TSV/CSV** (count matrices).

How you open and process these files determines whether your script runs efficiently or crashes with an Out-of-Memory error.

---

## 1. Slurping vs. Streaming: Why Line-by-Line Matters

When reading a file, there are two primary strategies:

### Anti-Pattern: "Slurping" (Reading the Entire File into RAM)
```python
# DANGEROUS: Loads all 50 GB of data into memory at once!
with open("massive_sequencing_run.fastq", "r") as f:
    all_lines = f.readlines()  # Out-of-Memory crash on large files
```

### Best Practice: "Streaming" (Processing Line-by-Line)
```python
# EFFICIENT: Reads one line at a time; memory usage remains constant O(1)
with open("massive_sequencing_run.fastq", "r") as f:
    for line in f:
        # process single line...
        pass
```

In the streaming pattern, only the active line occupies RAM. Even if the file is 500 GB, your Python script will use less than 50 MB of memory.

---

## 2. Context Managers (`with open(...)`)

Always open files using Python's `with` statement:

```python
with open("sample.fasta", "r", encoding="utf-8") as f:
    data = f.read()
# File is automatically closed when exiting the block, even if an error occurs!
```

* **Why it matters**: If you open files with `f = open(...)` without closing them, file handles remain locked by the operating system. In large loops iterating over thousands of sample files, your system will run out of file descriptors (`Too many open files` error).

---

## 3. Parsing Real-World Biological Formats

### A. Simple FASTA Streaming Parser
FASTA files contain header lines starting with `>` followed by multiline sequence characters:

```python
def stream_fasta(filepath: str):
    """Generator yielding (header, sequence) tuples one at a time."""
    current_header = None
    current_seq_chunks = []
    
    with open(filepath, "r", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue  # Skip blank lines safely
            
            if line.startswith(">"):
                if current_header is not None:
                    yield current_header, "".join(current_seq_chunks)
                current_header = line[1:]  # Strip the '>' symbol
                current_seq_chunks = []
            else:
                current_seq_chunks.append(line.upper())
                
        # Yield the final record
        if current_header is not None:
            yield current_header, "".join(current_seq_chunks)
```

### B. Tabular Data (TSV / CSV)
Never use manual string splits (`line.split(",")`) when parsing CSVs that might contain quoted fields or embedded commas. Use Python's standard `csv` module:

```python
import csv

with open("differential_expression.tsv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f, delimiter="\t")
    for row in reader:
        gene = row["gene_id"]
        log2fc = float(row["log2FoldChange"])
        padj = float(row["padj"])
        if padj < 0.05 and abs(log2fc) > 1.0:
            print(f"Significant: {gene} (log2FC = {log2fc})")
```

> **Takeaway**: Stream files line-by-line rather than loading whole files into memory, use context managers (`with open`) to guarantee closed file descriptors, and strip trailing whitespace/blank lines defensively.
