---
title: "Commuter pass writeup"
date: 2026-01-19T18:28:12-07:00
draft: false
math: true
---

> __NOTE__: This writeup was provided and written by Prof. Zilin Jiang

[problem link](https://oj.uz/problem/view/JOI18_commuter_pass)

## Problem


Given an undirected graph $G$ with positive edge weights and four special vertices $S,T,U,V$:

- Choose **one** shortest path $P$ from $S$ to $T$.
- Modify the graph by setting the weight of every edge on $P$ to **0** (all other edges keep their original weights).
- Measure the resulting shortest-path distance from $U$ to $V$.

**Goal:** choose $P$ to minimize this $U \to V$ distance.

---

## Overview of the Approach

1. Compute all shortest paths from $S$ to $T$ implicitly by constructing a **shortest-path DAG**.
2. Precompute distances from $U$ and from $V$ to all vertices.
3. Evaluate three structural cases for the optimal $U \to V$ path.

The key idea is that zeroing a shortest $S \to T$ path corresponds to traversing a **directed subpath** of the $S \to T$ shortest-path DAG at zero cost.

---

Run Dijkstra from source $S$, obtaining distances `distS[]`.

For each undirected edge $\{a,b\}$ of weight $w$:

- If `distS[b] = distS[a] + w`, add a **directed critical edge** $a \to b$.
- If `distS[a] = distS[b] + w`, add a **directed critical edge** $b \to a$.

These directed edges form a DAG, oriented away from $S$.

Perform a DFS/BFS **backwards from** $T$ following incoming critical edges. Retain only the visited vertices and edges.

The resulting graph is the $S \to T$ DAG:

- A directed acyclic graph
- Source: $S$
- Sink: $T$
- Every directed path from $S$ to $T$ corresponds to a shortest $S \to T$ path in the original graph

---

Run Dijkstra twice on the **original graph**:

- From $U$: obtain `distU[x]` for all vertices $x$
- From $V$: obtain `distV[x]` for all vertices $x$

---

### Case 1: No Use of the $S \to T$ DAG

The path from \$U\$ to \$V\$ does not benefit from zeroing any edge of the chosen $S \to T$ path.

In this case, the optimal value is simply the original shortest-path distance:

$$\text{case1} = \text{distU}[V].$$

---

### Case 2: Use the DAG in Forward Direction

The path structure is:

$U \to x \; + \; (x \rightsquigarrow y \text{ in DAG, zero cost}) \; + \; y \to V$

We must minimize: $\text{distU}[x] + \text{distV}[y]$ subject to a directed path from $x$ to $y$ in the $S  \to  T$ DAG.

To minimize this, we can fix the endpoint $y$ in the DAG. Then we only need the smallest $\text{distU}[x]$ among all $x$ that can reach $y$ in the DAG; adding $\text{distV}[y]$ gives the best total for that $y$. A topological order lets us compute these minima efficiently.

#### Propagation via Topological Order

Process vertices of the DAG in topological order (increasing `distS[]`).

- Initialize `bestU[z] = distU[z]` for each DAG vertex $z$.
- For every DAG edge $p \to q$:
  ```
  bestU[q] = min(bestU[q], bestU[p])
  ```

After propagation:

- `bestU[y] = min distU[x]` over all $x$ on a directed path from $S$ to $y$.

Compute: $\text{case2} = \min_{y \in \text{DAG}} (\text{bestU}[y] + \text{distV}[y])$

---

### Case 3: Symmetric to Case 2

Swap the roles of $U$ and $V$.

- Initialize `bestV[z] = distV[z]`
- Propagate along the DAG as above

Compute: $\text{case3} = \min_{y \in \text{DAG}} (\text{bestV}[y] + \text{distU}[y])$

---

Final Answer is $\min(\text{case1}, \text{case2}, \text{case3})$

---

## Complexity

Let $n$ be the number of vertices and $m$ the number of edges of $G$.

- Dijkstra from $S, U, V$: $O(m \log m)$ (or $O(m\log n)$)
- DAG construction and pruning: $O(m)$
- DAG propagation along a topological order: $O(n + m)$

**Total:** $O(m \log m)$ time, $O(n + m)$ space.
