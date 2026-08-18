---
layout: default
title: Graph Algorithms & Network Biology
parent: CS201 Data Structures & Algorithms I
nav_order: 6
---

# Graph Algorithms & Network Biology

Biology is inherently relational: proteins interact in regulatory cascades, metabolites transform along enzymatic pathways, and genome assemblers reconstruct chromosomes by traversing overlaps between sequencing reads.

Graphs are the mathematical language used to model, analyze, and traverse these complex biological systems.

---

## 1. Graph Representations: Adjacency Matrix vs. List

A graph $G = (V, E)$ consists of a set of vertices $V$ (e.g., genes, proteins, k-mers) and edges $E$ (e.g., interactions, overlaps).

```
Adjacency Matrix:                     Adjacency List:
     A  B  C                               A ──> [ B ]
  A [0, 1, 0]                              B ──> [ A, C ]
  B [1, 0, 1]                              C ──> [ B ]
  C [0, 1, 0]
Space: O(|V|^2)                       Space: O(|V| + |E|)  <-- Ideal for sparse bio-networks!
```

* **Adjacency Matrix**: Dense table of size $\|V\| \times \|V\|$. Fast $O(1)$ edge query, but wastes vast memory on sparse biological networks (where each gene typically interacts with only a few partners).
* **Adjacency List**: Array/hash map of lists containing only existing neighbors. Optimal space $O(\|V\| + \|E\|)$ for sparse biological graphs.

---

## 2. Graph Traversal: BFS vs. DFS

### Breadth-First Search (BFS)
* **Strategy**: Explores graph layer-by-layer using a FIFO **Queue**.
* **Property**: Finds the **shortest path in unweighted graphs** (e.g., the minimum number of enzymatic reaction steps connecting Metabolite A to Metabolite B).
* **Time Complexity**: $O(\|V\| + \|E\|)$.

### Depth-First Search (DFS)
* **Strategy**: Explores as deep as possible along each branch before backtracking using a LIFO **Stack** (or recursion).
* **Property**: Detects cycles, connected components, and generates topological ordering.
* **Time Complexity**: $O(\|V\| + \|E\|)$.

---

## 3. Directed Acyclic Graphs (DAGs) & Topological Sorting

A **DAG** is a directed graph containing no directed cycles.

### Applications in Computational Workflows & Biology
1. **Bioinformatics Pipeline Schedulers (Nextflow, Snakemake)**: Represent rule execution dependencies as a DAG.
2. **Topological Sort**: Linear ordering of vertices such that for every directed edge $U \rightarrow V$, vertex $U$ appears before $V$.

```
[ FastQC ] ──> [ Trimming ] ──> [ Alignment ] ──> [ Variant Calling ]
                                       │
                                       └──> [ Coverage Plots ]
```

* **Kahn’s Algorithm (Indegree-based)** or **DFS Post-order** computes topological order in $O(\|V\| + \|E\|)$ time.
* If a topological sort cannot process all vertices, a **cycle exists** (indicating an impossible circular dependency).

---

## 4. Weighted Shortest Paths & Minimum Spanning Trees

### Dijkstra’s Algorithm (Single-Source Shortest Path)
* **When to use**: Finding lowest-cost transmission paths in weighted networks with non-negative edge weights (e.g., phylogenetic distance graphs).
* **Implementation**: Uses a **Min-Heap / Priority Queue** to greedily expand the vertex with the lowest known distance.
* **Complexity**: $O((\|V\| + \|E\|) \log \|V\|)$.

### Minimum Spanning Tree (MST)
* **Concept**: Connecting all vertices together with minimum total edge weight without creating cycles.
* **Kruskal’s Algorithm**: Greedily adds smallest edges using **Union-Find** for cycle detection in $O(\|E\| \log \|E\|)$ time.
* **Prim’s Algorithm**: Grows a connected tree using a **Priority Queue** in $O(\|E\| \log \|V\|)$ time.
* **Biological Use Case**: Constructing minimum-spanning phylogenetic networks and single-cell developmental trajectory pseudotime trees.

> **Takeaway**: Represent sparse biological networks with adjacency lists. Use BFS for unweighted shortest paths, DAG topological sorts for workflow execution, and Dijkstra / MST algorithms for evolutionary and pathway distance analysis.
