# Topic 08: Graph Coloring Problem (Backtracking)

This document provides complete, step-by-step mathematical solutions and state-space tree traces for the **Graph Coloring (m-Coloring)** numerical problems appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition
Given an undirected graph $G = (V, E)$ with $n = |V|$ vertices represented by an $n \times n$ adjacency matrix $A$, and a positive integer $m$ representing the number of available colors $\{1, 2, \dots, m\}$.

The **$m$-Coloring Decision / Optimization Problem** asks whether it is possible to assign a color from $\{1, 2, \dots, m\}$ to each vertex such that **no two adjacent vertices share the same color**:
$$\forall (u, v) \in E \implies \text{color}(u) \ne \text{color}(v)$$

The solution is represented as an $n$-tuple:
$$X = (x_1, x_2, \dots, x_n) \quad \text{where } x_i \in \{1, 2, \dots, m\}$$

### 1.2 Backtracking Algorithm (`mColoring` & `NextValue`)

```pascal
Algorithm NextValue(k):
    repeat
        x[k] = (x[k] + 1) mod (m + 1)   // Next color candidate
        if x[k] = 0 then return        // All colors exhausted
        for j = 1 to k - 1 do
            if (A[k, j] != 0 and x[k] == x[j]) then
                break                  // Conflict detected with an adjacent vertex
        if j == k then return          // Distinct color found; return valid color
    until false

Algorithm mColoring(k):
    repeat
        NextValue(k)                   // Assign valid color to vertex k
        if x[k] == 0 then return       // No valid color exists; backtrack
        if k == n then
            write(x[1..n])             // Complete valid coloring found
        else
            mColoring(k + 1)           // Recurse for vertex k + 1
    until false
```

### 1.3 Complexity Analysis
- **State-Space Tree:** An $m$-ary tree with depth $n$.
- **Number of Nodes in Worst Case:** $\sum_{i=0}^n m^i = \frac{m^{n+1} - 1}{m - 1} = O(m^n)$.
- **Time Complexity:** Each node checks up to $n$ adjacent vertices $\implies O(n \cdot m^n)$.
- **Space Complexity:** $O(n)$ recursion stack and color array $X$.

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU Dec 2022/Jan 2023 [8 Marks]

#### Problem Statement
> Explain backtracking with the graph coloring problem. Find a valid solution and draw the state-space tree for the following graph with $m = 3$ colors (Red = 1, Green = 2, Blue = 3):
>
> **Adjacency Matrix $A$ for Graph $G$ ($n = 5$ vertices $C_1, C_2, C_3, C_4, C_5$):**
>
> | Vertex | $C_1$ | $C_2$ | $C_3$ | $C_4$ | $C_5$ |
> | :---: | :---: | :---: | :---: | :---: | :---: |
> | **$C_1$** | 0 | 1 | 0 | 1 | 0 |
> | **$C_2$** | 1 | 0 | 1 | 0 | 0 |
> | **$C_3$** | 0 | 1 | 0 | 1 | 1 |
> | **$C_4$** | 1 | 0 | 1 | 0 | 1 |
> | **$C_5$** | 0 | 0 | 1 | 1 | 0 |

---

#### Step 1: Adjacency List & Graph Structure
From the adjacency matrix, the edges are:
- $C_1$ is adjacent to: $\{C_2, C_4\}$
- $C_2$ is adjacent to: $\{C_1, C_3\}$
- $C_3$ is adjacent to: $\{C_2, C_4, C_5\}$
- $C_4$ is adjacent to: $\{C_1, C_3, C_5\}$
- $C_5$ is adjacent to: $\{C_3, C_4\}$

Number of colors: $m = 3 \implies \{1: \text{Red}, 2: \text{Green}, 3: \text{Blue}\}$.

---

#### Step 2: Step-by-Step Backtracking Trace

1. **Vertex $C_1$ (Level 1):**
   - Candidate color: $x_1 = 1$ (Red).
   - Valid, as no previous vertices are colored.
   - Vector: $(1, 0, 0, 0, 0)$

2. **Vertex $C_2$ (Level 2):**
   - Check color 1 (Red): Adjacent to $C_1$ ($A[2, 1] = 1$) and $x_1 = 1 \implies$ **Conflict!**
   - Try color 2 (Green): No adjacent vertex has color 2 $\implies$ **Valid!**
   - Assign $x_2 = 2$ (Green).
   - Vector: $(1, 2, 0, 0, 0)$

3. **Vertex $C_3$ (Level 3):**
   - Adjacent to $C_2$ (Green). Not adjacent to $C_1$.
   - Try color 1 (Red):
     - Check $C_2$: $x_2 = 2 \ne 1$ (No conflict).
     - Check $C_1$: $A[3, 1] = 0$ (No edge).
     - Color 1 is **Valid!**
   - Assign $x_3 = 1$ (Red).
   - Vector: $(1, 2, 1, 0, 0)$

4. **Vertex $C_4$ (Level 4):**
   - Adjacent to $C_1$ (Red) and $C_3$ (Red).
   - Try color 1 (Red): $x_1 = 1, x_3 = 1 \implies$ **Conflict!**
   - Try color 2 (Green):
     - Check $C_1$: $A[4, 1] = 1, x_1 = 1 \ne 2$ (Compatible).
     - Check $C_2$: $A[4, 2] = 0$ (No edge).
     - Check $C_3$: $A[4, 3] = 1, x_3 = 1 \ne 2$ (Compatible).
     - Color 2 is **Valid!**
   - Assign $x_4 = 2$ (Green).
   - Vector: $(1, 2, 1, 2, 0)$

5. **Vertex $C_5$ (Level 5):**
   - Adjacent to $C_3$ (Red) and $C_4$ (Green).
   - Try color 1 (Red): Adjacent to $C_3$ ($x_3 = 1$) $\implies$ **Conflict!**
   - Try color 2 (Green): Adjacent to $C_4$ ($x_4 = 2$) $\implies$ **Conflict!**
   - Try color 3 (Blue):
     - Check $C_3$: $x_3 = 1 \ne 3$ (Compatible).
     - Check $C_4$: $x_4 = 2 \ne 3$ (Compatible).
     - Check $C_1, C_2$: No edges.
     - Color 3 is **Valid!**
   - Assign $x_5 = 3$ (Blue).
   - All $n = 5$ vertices successfully colored!

---

#### Step 3: State-Space Search Tree

```
                                [ Root ]
                                   |
                             C1 = 1 (Red)
                                   |
                            C2 = 2 (Green)  [C2=1 pruned: conflict C1]
                                   |
                             C3 = 1 (Red)   [C3=2 pruned: conflict C2]
                                   |
                            C4 = 2 (Green)  [C4=1 pruned: conflict C1, C3]
                                   |
                             C5 = 3 (Blue)  [C5=1 pruned: conflict C3]
                                            [C5=2 pruned: conflict C4]
                                   |
                          [ SUCCESS LEAF ]
                        Vector: (1, 2, 1, 2, 3)
```

---

#### Step 4: Final Output
- **Optimal Coloring Vector:**
  $$X = (x_1, x_2, x_3, x_4, x_5) = \mathbf{(1, 2, 1, 2, 3)}$$
- **Color Assignments:**
  - Vertex $C_1 \implies$ **Red** (Color 1)
  - Vertex $C_2 \implies$ **Green** (Color 2)
  - Vertex $C_3 \implies$ **Red** (Color 1)
  - Vertex $C_4 \implies$ **Green** (Color 2)
  - Vertex $C_5 \implies$ **Blue** (Color 3)
- **Chromatic Number Verification:** The graph has odd cycle $C_3 - C_4 - C_5 - C_3$ of length 3 ($K_3$ subgraph), so $\chi(G) \ge 3$. Thus, $m = 3$ colors is minimal.

---

### Instance B: 4-Vertex Cycle / Planar Graph [10 Marks]

#### Problem Statement
> Given a 4-vertex cycle graph $G = (V, E)$ where $V = \{A, B, C, D\}$ with edges:
> $$E = \{(A, B), (B, C), (C, D), (D, A)\}$$
> Trace the backtracking algorithm to color the graph using:
> 1. $m = 2$ colors (Red = 1, Blue = 2)
> 2. $m = 3$ colors

---

#### Part 1: Trace for $m = 2$ Colors

1. **Vertex $A$:** Assign Color 1 (Red).
2. **Vertex $B$ (adjacent to $A$):**
   - Try 1: Conflict with $A$.
   - Try 2: Compatible. Assign Color 2 (Blue).
3. **Vertex $C$ (adjacent to $B$):**
   - Try 1: Compatible (not adjacent to $A$). Assign Color 1 (Red).
4. **Vertex $D$ (adjacent to $C$ and $A$):**
   - Try 1: Conflict with $A$ ($x_A = 1$) and $C$ ($x_C = 1$).
   - Try 2: Compatible (adjacent vertices $A, C$ both have Color 1). Assign Color 2 (Blue).

**First Solution with $m = 2$:**
$$X_1 = (A: 1, B: 2, C: 1, D: 2) \implies \mathbf{(Red, Blue, Red, Blue)}$$

**Symmetric Second Solution:**
$$X_2 = (A: 2, B: 1, C: 2, D: 1) \implies \mathbf{(Blue, Red, Blue, Red)}$$

Both solutions verify that an even cycle $C_4$ is bipartite and strictly 2-colorable ($\chi(C_4) = 2$).
