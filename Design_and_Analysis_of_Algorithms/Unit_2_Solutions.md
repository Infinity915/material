# SPPU B.E. Computer Engineering (2019 Pattern)
## Design and Analysis of Algorithms — Unit 2: Backtracking & Branch and Bound
### Complete Examination Solutions Guide (Engineered for Full 8/8 Marks)

---

## Q1. Explain Backtracking: Principle, control abstraction, and time analysis of control abstraction with a suitable example. (Frequency: 3) [8 Marks]

### Introduction & Core Intuition

**Backtracking** is a systematic algorithmic technique for solving **combinatorial** search problems by exploring a conceptual **State Space Tree** using Depth-**First Search** (DFS). Whenever the **algorithm** determines that a partial solution cannot possibly be extended to a valid or optimal complete solution, it abandons the branch ('prunes' the dead end) and <i>backtracks</i> to the parent **node** to explore alternative decision paths.

### **Core Principle of the Backtracking Strategy**

Unlike brute-force **exhaustive search**, which blindfoldedly generates and tests all candidate combinations, **backtracking** builds candidate solutions incrementally. At each level k of the **state space tree**, the **algorithm** assigns a value to decision variable x_k. Before descending deeper, it invokes a **Bounding Function** (or constraint predicate). If the partial **vector** (x_1, x_2, ..., x_k) violates any problem constraint, the subtree rooted at this node is pruned immediately, saving enormous computational effort.

### **Control Abstraction for Recursive Backtracking**

The **control abstraction** for **backtracking** is **typically** **implemented** as a recursive **function**. It accepts the current solution depth k and the partial assignment **vector** x. A loop iterates through all admissible values for x_k. For each candidate value, it checks validity using a problem-specific constraint function. If valid, it checks whether a complete solution has been reached; if so, it records the solution; otherwise, it recursively calls itself for level k + 1. Upon function return, it naturally backtracks to test the next candidate.

### **Terminology of the State Space Search**

**Backtracking** operates on distinct **node** classifications: **The Live Node** is a **generated** node **whose** children have not yet been completely evaluated. The **E-Node** (Expanding Node) is the currently active live node whose children are being explored. A Dead Node is a generated node that either violates constraints or whose complete subtree has been exhausted. Backtracking maintains only the current path from the root to the E-node in memory using the call stack.

### **Time Complexity Analysis of Backtracking**

The worst-case **time complexity** of **backtracking** remains exponential—typically O(b^d), where b is the branching factor and d is the maximum **tree** depth (e.g., O(2^n) for subset selection or O(n!) for permutation problems). However, the average-case runtime is drastically lower in practice because effective bounding **functions** prune vast subtrees early in the **search**, avoiding the exploration of millions of **infeasible** states.

### **Practical Exam Example: N-Queens Problem**

A classic demonstration of backtracking is the **4-Queens Problem**: place 4 non-attacking queens on a 4x4 chessboard. The algorithm places Queen 1 in row 1, col 1. It attempts Queen 2 in row 2: cols 1 and 2 attack Queen 1; col 3 is valid. It attempts Queen 3 in row 3: all 4 columns attack existing queens! Instead of proceeding further, the algorithm prunes the branch, backtracks to row 2, moves Queen 2 to col 4, and proceeds forward, eventually locating the valid non-attacking configuration without checking all 4^4 = 256 positions.

### Architectural Flow & System Diagram

![Figure 2.1: Backtracking: Principle, control abstraction, and time analysis of control abstraction with a suitable example](diagrams/u2_q1.png)

*Figure 2.1: Architectural Flow and Conceptual System Diagram for Backtracking: Principle, control abstraction, and time analysis of control abstraction with a suitable example*

### Mathematical Formulation & Algorithmic Mechanics

**Recursive Backtracking Control Abstraction:**

```text
Algorithm Backtrack(k):
    for each candidate c ∈ Domain(k) do:
        x[k] = c
        if BoundingTest(x[1..k]) then:
            if IsCompleteSolution(x[1..k]) then:
                Output(x[1..k])
            else:
                Backtrack(k + 1)
    // Implicit backtrack occurs when loop terminates
```

The recursive control abstraction defined above formalizes the Backtracking search strategy. The procedure explores a state-space tree depth-first, representing candidate solutions as tuples $(x_1, x_2, \dots, x_k)$. At each depth $k$, the algorithm evaluates domain values via `BoundingTest(x[1..k])`. If the partial vector violates problem constraints, the subtree is pruned and execution backtracks immediately, avoiding exhaustive $O(m^n)$ brute-force enumeration.

### Worked Example & Practical Simulation

**Trace** on 4-**Queens Problem** (Placing 4 non-**attacking** **queens**): • Step 1: **Place** Q1 at (Row 1, Col 2). • Step 2: Try Q2 at Row 2 -> Col 1 **attacks** diagonally; Col 2 attacks vertically; Col 3 attacks diagonally. Place Q2 at (Row 2, Col 4). • Step 3: Try Q3 at Row 3 -> Col 1 is safe! Place Q3 at (Row 3, Col 1). • Step 4: Try Q4 at Row 4 -> Col 1, 2, 4 are attacked. Col 3 is safe! Place Q4 at (Row 4, Col 3). Complete Non-Attacking Solution Found: Queen Positions = [Col 2, Col 4, Col 1, Col 3].

### Key Comparative Evaluation

| Algorithmic Dimension | Backtracking Strategy | Brute-Force Exhaustive Search |
| :--- | :--- | :--- |
| Search Paradigm | Depth-First Search (DFS) with constraint bounding | Unconstrained generate-and-test of entire space |
| State Pruning | Prunes dead-end subtrees immediately upon failure | Evaluates all leaves even if early choices are invalid |
| Memory Usage | Minimal O(d) recursion stack space | Often requires O(b^d) memory or large queues |
| Average-Case Performance | Often fast polynomial-like behavior for constrained inputs | Always suffers worst-case exponential time O(b^d) |

### Concluding Exam Takeaway

**Backtracking** delivers an elegant depth-first pruning framework that eliminates infeasible **combinatorial** paths early, solving heavily constrained **NP-hard** puzzles efficiently with minimal O(d) stack **memory**. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 10: Backtracking, Sections 10.1–10.3, Pages 385–393.

---

## Q2. What is the sum of subset problem? Solve it for given instances (e.g., finding a sum of 10 or 30 from a given set) using the backtracking approach and show the state space tree being created. (Frequency: 5) [8 Marks]

### Introduction & Core Intuition

**The Sum** of Subsets Problem is a classic decision and **optimization** problem: given a set of n positive integers S = {**w_1**, w_2, ..., w_n} and a positive target sum M, find all subsets of S whose elements sum up to exactly M. It is an **NP-complete** problem that is solved efficiently via **backtracking** by pruning branches whose partial sum exceeds M or cannot possibly reach M.

### **Mathematical Formulation and Constraint Equations**

We are **given** **weights** w_i > 0 and **target** M. We **represent** the solution as a binary **vector** X = (x_1, x_2, ..., x_n), where **x_i** in {0, 1}. If x_i = 1, element w_i is included in the subset; if x_i = 0, it is excluded. The objective is to satisfy: sum(w_i * x_i for i=1 to n) = M. To maximize pruning efficiency, we sort the elements in non-decreasing order: w_1 <= w_2 <= ... <= w_n.

### **Two Powerful Bounding Functions for Pruning**

**Backtracking** incorporates two **distinct** **bounding** criteria at each **node** at depth k with current accumulated sum s = sum(w_i * **x_i** for i=1 to k) and remaining available weight r = sum(w_i for i=k+1 to n): (1) **Upper Bound Pruning**: If s + w_(k+1) > M, including any further element will exceed M, so the left child branch can be pruned; (2) **Lower Bound Pruning**: If s + r < M, even taking all remaining available elements cannot reach M, so the right child branch is pruned.

### **State Space Tree Construction Protocol**

The **state space tree** is a binary **tree** where the left **branch** from **node** k **represents** including element w_(k+1) (setting x_(k+1) = 1), and the right branch represents excluding element w_(k+1) (setting x_(k+1) = 0). The tree is explored using Depth-**First Search**. When s == M, a valid solution subset is recorded, and the algorithm backtracks to uncover alternative combinations.

### **Complete Walkthrough for Instance S = {3, 5, 7}, Target M = 10**

Let S = {3, 5, 7} with **total** **weight** 15 and **target** M = 10. **Node** 1 (Root, s=0, r=15): (1) Left **branch** **includes** 3: s=3, r=12. (2) From here, left branch includes 5: s=8, r=7. From here, left branch tries +7: s=15 > 10 (Pruned by upper bound!). Right branch excludes 7: s=8 != 10 (Dead end). (3) Backtrack to s=3, right branch excludes 5: s=3, r=7. From here, left branch includes 7: s = 3 + 7 = 10 == M! Solution Found: {3, 7} (vector: [1, 0, 1]). (4) Backtrack to root, right branch excludes 3: s=0, r=12. Left tries +5: s=5. Left tries +7: s=12 > 10 (Pruned). Right excludes 7: s=5 != 10. Entire tree explored; unique solution is {3, 7}.

### **Asymptotic Complexity and Efficiency**

In the worst case where no pruning occurs, the state space tree contains 2^(n+1) - 1 nodes, yielding an exponential runtime of O(2^n). However, with sorted elements and dual bounding functions, the effective branching factor drops dramatically, reducing search time to polynomial proportions for typical examination datasets.

### Architectural Flow & System Diagram

![Figure 2.2: What is the sum of subset problem? Solve it for given instances (e](diagrams/u2_q2.png)

*Figure 2.2: Architectural Flow and Conceptual System Diagram for What is the sum of subset problem? Solve it for given instances (e*

### Mathematical Formulation & Algorithmic Mechanics

**Sum of Subsets Bounding Functions:**

```text
Current Partial Sum: s = ∑_(i=1)^k w_i · x_i
Remaining Available Weight: r = ∑_(i=k+1)^n w_i
Left Branch Validity (Include w_(k+1)): s + w_(k+1) ≤ M
Right Branch Validity (Exclude w_(k+1)): s + r - w_(k+1) ≥ M
```

The bounding functions defined above govern the Sum of Subsets backtracking algorithm. Pruning is enforced by two mathematical criteria: a branch is terminated if the accumulated sum exceeds target weight ($s + w_{k+1} > M$), or if the remaining sum of all unconsidered items is insufficient to reach the target ($s + \sum_{i=k+1}^n w_i < M$). These bounding tests prune exponential subtrees, achieving efficient average-case performance.

### Worked Example & Practical Simulation

**Backtracking** Search **Trace** on Set S = {5, 10, 12, 13, 15, 18}, **Target** M = 30: • **Solution** 1: {5, 10, 15} -> 5 + 10 + 15 = 30 (**Vector**: [1, 1, 0, 0, 1, 0]) • **Solution** 2: {5, 12, 13} -> 5 + 12 + 13 = 30 (Vector: [1, 0, 1, 1, 0, 0]) • Solution 3: {12, 18} -> 12 + 18 = 30 (Vector: [0, 0, 1, 0, 0, 1]) Pruned Branches: Any branch where partial sum exceeds 30 or remaining sum is insufficient is pruned immediately.

### Key Comparative Evaluation

| Node Path | Elements Included | Current Sum (s) | Remaining Sum (r) | Bounding Test Evaluation | Action Taken |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Root -> Left | {3} | 3 | 12 | s + w2 = 3 + 5 = 8 <= 10 | Valid; descend left |
| Root -> L -> L | {3, 5} | 8 | 7 | s + w3 = 8 + 7 = 15 > 10 | PRUNED by Upper Bound |
| Root -> L -> R -> L | {3, 7} | 10 | 0 | s == Target M = 10 | SOLUTION FOUND: {3, 7} |
| Root -> Right | {} | 0 | 12 | s + r = 0 + 12 >= 10 | Valid; descend right |

### Concluding Exam Takeaway

**Backtracking** solves the Sum of **Subsets** **problem** by dynamically navigating a binary **state space tree**, using dual sum **bounds** to prune **infeasible** paths and isolate exact matching combinations like {3, 7} for target 10.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 10: Backtracking, Section 10.5: Sum of Subsets, Pages 398–404.

---

## Q3. Explain backtracking with the graph coloring problem by writing a recursive backtracking algorithm to color all vertices of a given graph (adjacency matrix) using 'm' colors, and comment on its time complexity. (Frequency: 4) [8 Marks]

### Introduction & Core Intuition

The **Graph Coloring** Problem (m-Colorability) requires assigning one of m available colors to each vertex of an undirected **graph** G = (V, E) such that no two adjacent vertices share the exact same color. Determining whether an arbitrary graph is m-colorable is **NP-complete** for m >= 3, and **backtracking** provides the standard exact **algorithm** to compute all valid colorings.

### **Problem Formulation and Adjacency Representation**

Let G = (V, E) be a **graph** with n **vertices** **represented** by an n x n **binary** adjacency **matrix** A, where A[i][j] = 1 if an edge **connects** vertex i and vertex j, and A[i][j] = 0 otherwise. We are given m available colors represented by integers {1, 2, ..., m}. The solution is a vector X = (x_1, x_2, ..., x_n), where x_k in {1, 2, ..., m} denotes the color assigned to vertex k. The fundamental constraint requires that if A[i][j] == 1, then x_i != x_j.

### **Recursive Backtracking Algorithmic Mechanism**

The **algorithm** proceeds **vertex** by **vertex** from k = 1 to n. A helper **function** NextValue(k) is **invoked** to **determine** a valid color for vertex k. NextValue cycles through colors c = (x_k + 1) mod (m + 1). For each candidate color, it inspects all previous vertices j = 1 to k - 1. If an edge exists (A[k][j] == 1) and vertex j already has color c (x_j == c), the candidate color is invalid. If a conflict-free color is found, NextValue returns it; if all m colors conflict, it resets x_k = 0 and triggers backtracking.

### **State Space Tree Structure for m-Coloring**

The **state space tree** has **depth** n, and each internal **node** has up to m **children** **corresponding** to the m **color** assignments for that vertex. The root represents zero colored vertices. Level 1 assigns colors to vertex 1, Level 2 assigns colors to vertex 2, and so forth. If NextValue(k) finds no valid color, the algorithm prunes that entire subtree and backtracks to vertex k - 1 to try the next available color.

### **Time Complexity Analysis of m-Coloring**

In the worst **case** (such as an empty **graph** with no **edges**), every **vertex** can take any of the m colors independently, producing a **state space tree** with m^n leaf **nodes**. At each node, verifying adjacency conflicts against up to n - 1 vertices takes O(n) time. Therefore, the worst-case time complexity is O(n * m^n). In practice, dense edge constraints prune massive portions of the search tree early, allowing the algorithm to execute orders of magnitude faster on realistic planar and scheduling graphs.

### **Real-World Engineering Applications**

Graph coloring via backtracking models numerous critical computational systems: (1) **Compiler Register Allocation**, where variables with overlapping lifetimes are vertices and physical CPU registers are colors; (2) **Exam Timetable Scheduling**, where conflicting student courses cannot share the same timeslot color; and (3) **Radio Frequency Allocation**, where adjacent transmission towers cannot use identical frequency bands.

### Architectural Flow & System Diagram

![Figure 2.3: backtracking with the graph coloring problem by writing a recursive backtracking algorithm to color all vertices of a given graph (adjacency matrix) using 'm' colors, and comment on its time complexity](diagrams/u2_q3.png)

*Figure 2.3: Architectural Flow and Conceptual System Diagram for backtracking with the graph coloring problem by writing a recursive backtracking algorithm to color all vertices of a given graph (adjacency matrix) using 'm' colors, and comment on its time complexity*

### Mathematical Formulation & Algorithmic Mechanics

**Recursive m-Coloring Backtracking Algorithm:**

```text
Algorithm mColoring(k):
    repeat:
        NextValue(k)  // Assigns valid color to x[k] or 0
        if x[k] == 0 then return  // Backtrack to parent
        if k == n then:
            Output(x[1..n])  // Valid coloring found
        else:
            mColoring(k + 1)  // Recurse to next vertex
    until false

Algorithm NextValue(k):
    repeat:
        x[k] = (x[k] + 1) mod (m + 1)
        if x[k] == 0 then return
        for j = 1 to n do:
            if A[k, j] == 1 and x[k] == x[j] then break
        if j == n + 1 then return  // Valid color found
```

The recursive $m$-Coloring backtracking algorithm defined above assigns colors to vertices such that no two adjacent vertices share the same color. The `NextValue` subroutine evaluates color assignments $c \in [1, m]$ for vertex $k$, verifying non-conflict against all adjacent neighbors ($G[k][j] == 1 \implies x[k] 
eq x[j]$). If no valid color exists in $[1, m]$, the algorithm backtracks to re-color vertex $k-1$.

### Worked Example & Practical Simulation

**Coloring** a 4-**Vertex Cycle Graph** C4 with m = 3 **Colors** {1:Red, 2:**Green**, 3:Blue}: • Adjacency **Matrix**: Edges (1-2), (2-3), (3-4), (4-1). • Step 1: **Vertex** 1 gets Color 1 (Red). • Step 2: Vertex 2 tries Color 1 (Conflict with V1) -> Gets Color 2 (Green). • Step 3: Vertex 3 tries Color 1 (Safe! No edge to V1) -> Gets Color 1 (Red). • Step 4: Vertex 4 tries Color 1 (Conflict with V1 & V3) -> Tries Color 2 (Safe!) -> Gets Color 2 (Green). Valid Coloring Vector: X = [1, 2, 1, 2] (Red, Green, Red, Green). Chromatic Number is 2.

### Key Comparative Evaluation

| Graph Topology | Vertices n | Colors m | Worst-Case Nodes | Actual Explored Nodes | Is m-Colorable? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Triangle Graph K3 | 3 | 2 | 2^3 = 8 | 4 (Prunes early on odd cycle) | NO (Requires 3 colors) |
| Cycle Graph C4 | 4 | 2 | 2^4 = 16 | 6 (Symmetric bipartite search) | YES (Vector: [1, 2, 1, 2]) |
| Complete Graph K4 | 4 | 3 | 3^4 = 81 | 12 (Each vertex must differ) | NO (Requires 4 colors) |
| Petersen Graph | 10 | 3 | 3^10 = 59,049 | 240 with pruning | YES (Known 3-colorable) |

### Concluding Exam Takeaway

**Backtracking** solves the NP-complete **Graph Coloring** problem by combining depth-first **search** with the NextValue conflict-checking **function**, isolating valid color assignments in O(n·m^n) **worst-case time**. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 10: Backtracking, Section 10.6: Graph Coloring, Pages 404–409.

---

## Q4. Prove that the full state space tree of finding the sum of subset of n elements using backtracking will have 2^(n+1) - 1 nodes (excluding leaf nodes). (Frequency: 1) [8 Marks]

### Introduction & Core Intuition

In theoretical **algorithm** analysis, understanding the maximum **combinatorial** dimensions of a search space is critical. For the Sum of Subsets problem on n elements, decisions are binary (include or exclude each element). Proving that the full **state space tree** contains exactly 2^(n+1) - **1 nodes** establishes the theoretical upper **bound** on search **complexity** before pruning is applied.

### **Structural Topology of the Binary State Space Tree**

**Consider** **finding** a **subset** of n **elements**: S = {**w_1**, w_2, ..., w_n}. At each level i (for i = 1 to n), the **algorithm** makes a binary choice for element w_i: either include it (x_i = 1) or exclude it (x_i = 0). This binary decision structure means that every internal node in the unpruned state space tree generates exactly two child branches. The resulting search space forms a **Complete Binary Tree** of depth n.

### **Level-by-Level Node Enumeration**

Let L **denote** the **level** of the **tree**, starting at the root where L = 0: **Level** 0 **contains** 2^0 = 1 **node** (the root, representing the empty subset before any decisions). Level 1 makes the binary choice for element w_1, producing 2^1 = 2 nodes. Level 2 makes binary choices for element w_2, producing 2^2 = 4 nodes. In general, at any level i (where 0 <= i <= n), the number of nodes present is exactly 2^i.

### **Summation of Nodes across All Tree Levels**

To **determine** the **total** **number** of **nodes** N in the full **state space tree** from **Level** 0 through Level n, we compute the finite geometric summation: N = sum(2^i for i = 0 to n) = 2^0 + 2^1 + 2^2 + ... + 2^n.

### **Closed-Form Geometric Series Derivation**

**Using** the **standard** **formula** for the sum of a **finite** **geometric** **progression** S_n = a * (r^(n+1) - 1) / (r - 1) with first term a = 1 and common ratio r = 2: N = 1 * (2^(n+1) - 1) / (2 - 1) = 2^(n+1) - 1. Therefore, the total number of nodes in a full state space tree of depth n is mathematically proven to be precisely **2^(n+1) - 1**.

### **Node Classification: Internal Nodes vs Leaf Nodes**

In this full tree, the leaves reside at Level n, representing the 2^n distinct candidate subsets of the power set 2^S. The number of leaf nodes is L_count = 2^n. The number of internal (non-leaf) nodes is: I_count = Total Nodes - Leaf Nodes = (2^(n+1) - 1) - 2^n = (2 * 2^n - 1) - 2^n = 2^n - 1. If the examination question defines 'excluding leaf nodes' in the context of the total tree size up to level n, the geometric proof confirms that the complete tree structure contains 2^(n+1) - 1 total nodes, and 2^n - 1 internal decision nodes.

### Architectural Flow & System Diagram

![Figure 2.4: Prove that the full state space tree of finding the sum of subset of n elements using backtracking will have 2^(n+1) - 1 nodes (excluding leaf nodes)](diagrams/u2_q4.png)

*Figure 2.4: Architectural Flow and Conceptual System Diagram for Prove that the full state space tree of finding the sum of subset of n elements using backtracking will have 2^(n+1) - 1 nodes (excluding leaf nodes)*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Geometric Proof Derivation:**

```text
Total Nodes N = ∑_(i=0)^n 2^i = 2^0 + 2^1 + 2^2 + ··· + 2^n
Apply Geometric Series Sum: S = a · (r^(k) - 1) / (r - 1)   [a = 1, r = 2, k = n+1 terms]
N = 1 · ( 2^(n+1) - 1 ) / ( 2 - 1 ) = 2^(n+1) - 1   [Q.E.D.]
Internal Decision Nodes = (2^(n+1) - 1) - 2^n = 2^n - 1
```

The geometric series summation defined above derives the exact node count of the complete state-space tree for finding the sum of subsets of $n$ elements. Because each element has two binary choices (inclusion or exclusion), the tree is a complete binary tree of depth $n$. Summing nodes across all levels $k \in [0, n]$ yields $\sum_{k=0}^n 2^k = 2^{n+1} - 1$ total nodes, proving the theoretical exponential upper bound before pruning.

### Worked Example & Practical Simulation

**Concrete** Proof **Verification** for n = 3 **Elements** (S = {w1, w2, w3}): • **Level** 0 (Root): 2^0 = 1 **node**. • Level 1 (Decisions on w1): 2^1 = **2 nodes**. • Level 2 (Decisions on w2): 2^2 = 4 nodes. • Level 3 (Decisions on w3 / Leaves): 2^3 = 8 nodes. • Total Node Count N = 1 + 2 + 4 + 8 = 15. • Formula Evaluation: 2^(3+1) - 1 = 2^4 - 1 = 16 - 1 = 15 nodes (Matches exactly!). • Internal Nodes: 1 + 2 + 4 = 7 = 2^3 - 1.

### Key Comparative Evaluation

| Number of Elements (n) | Level 0 to n Node Breakdown | Formula: 2^(n+1) - 1 | Total Tree Nodes | Leaf Nodes (2^n) |
| :--- | :--- | :--- | :--- | :--- |
| n = 1 | 2^0 + 2^1 = 1 + 2 | 2^2 - 1 = 4 - 1 | 3 nodes | 2 leaves |
| n = 2 | 1 + 2 + 4 | 2^3 - 1 = 8 - 1 | 7 nodes | 4 leaves |
| n = 3 | 1 + 2 + 4 + 8 | 2^4 - 1 = 16 - 1 | 15 nodes | 8 leaves |
| n = 4 | 1 + 2 + 4 + 8 + 16 | 2^5 - 1 = 32 - 1 | 31 nodes | 16 leaves |

### Concluding Exam Takeaway

Through geometric series expansion, the total number of **nodes** in a full binary **state space tree** of depth n is conclusively proven to be 2^(n+1) - 1, illustrating the exponential **baseline** that **backtracking** bounding **functions** must prune.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 10: Backtracking, Section 10.2: State Space Trees, Pages 388–391.

---

## Q5. What is the Branch and Bound method? Write a control abstraction for Branch and Bound / Least Cost search and enlist its applications. (Frequency: 4) [8 Marks]

### Introduction & Core Intuition

**Branch and Bound** (B&B) is a **state**-space search paradigm designed exclusively for solving discrete and combinatorial **optimization problems**. Unlike **backtracking**, which traverses state **trees** using depth-first search, Branch and Bound uses Breadth-First (FIFO), Depth-First (LIFO), or Best-First (Least-Cost) search guided by mathematical **Bounding Functions** that prune non-promising subtrees.

### **Core Mechanisms: Branching and Bounding**

**Branch and Bound** relies on two complementary mechanisms: Branching partitions the solution space into smaller, mutually disjoint sub-problems by generating all children of an active **node** (the **E-Node**) simultaneously. Bounding computes a mathematical estimate (a **lower bound** for minimization, or an upper **bound** for maximization) on the best possible objective value achievable from any leaf in that node's subtree. If a node's bound is worse than the currently known best integer solution, the node is killed immediately without generating its descendants.

### **Least-Cost (LC) Search Strategy**

In Least-Cost (LC) **Branch and Bound**, **live nodes** are not explored in simple FIFO order. Instead, they are maintained in a **Priority Queue** (**Min-Heap**) ranked by an intelligent heuristic **bounding function** c_hat(x). The algorithm always selects the live node with the most promising bound (the lowest **lower bound** for minimization) to serve as the next **E-Node**. This targeted expansion steers the search directly toward optimal solutions, enabling aggressive pruning of competing subtrees.

### **Control Abstraction for LC Search**

The **control abstraction** initializes an empty **priority queue** and inserts the root **node** with its initial **bound**. An upper bound variable U is initialized to infinity (for minimization). In each iteration, the **algorithm** extracts the node with minimum c_hat from the queue. If its bound exceeds U, it is discarded. Otherwise, the algorithm generates all its children. For each child, if it represents a complete leaf solution with cost < U, U is updated. If the child is an internal node whose bound is strictly less than U, it is inserted into the priority queue.

### **Key Differences in Node Life Cycle**

In **Backtracking**, an **E-Node** generates one child at a time and remains active until its entire subtree finishes (DFS). In **Branch and Bound**, an E-**Node** generates <i>all</i> its children at once and is immediately retired from being the E-Node; its valid children enter the pool of **live nodes**. This eliminates recursion stack limitations but requires storing all live nodes in heap memory.

### **Primary Real-World Applications**

Branch and Bound is the standard industrial solver for NP-hard integer optimization challenges: (1) **Traveling Salesperson Problem (TSP)**, using reduced cost matrices to bound tour lengths; (2) **0/1 Knapsack Problem**, using fractional upper bounds; (3) **Integer Linear Programming (ILP)**, using LP relaxations; and (4) **Job Shop Scheduling**, minimizing total completion time (makespan).

### Architectural Flow & System Diagram

![Figure 2.5: What is the Branch and Bound method?  a control abstraction for Branch and Bound / Least Cost search and enlist its applications](diagrams/u2_q5.png)

*Figure 2.5: Architectural Flow and Conceptual System Diagram for What is the Branch and Bound method?  a control abstraction for Branch and Bound / Least Cost search and enlist its applications*

### Mathematical Formulation & Algorithmic Mechanics

**Least-Cost Branch and Bound (LCBB) Control Abstraction:**

```text
Algorithm LCBB(Root):
    U = infinity  // Best known upper bound for minimization
    PQ = PriorityQueue()  // Min-heap ordered by c_hat(node)
    PQ.insert(Root)
    while not PQ.isEmpty() do:
        E = PQ.extractMin()
        if c_hat(E) >= U then continue  // Prune node
        for each child x of E do:
            if IsCompleteSolution(x) then:
                if Cost(x) < U then U = Cost(x), BestSol = x
            else if c_hat(x) < U then:
                PQ.insert(x)
    return BestSol
```

The Least-Cost Branch and Bound (LCBB) control abstraction defined above explores optimization state-space trees using a priority queue. Each active live node $x$ is ranked by a lower-bound heuristic cost function $\hat{c}(x) = f(x) + \hat{h}(x)$. The algorithm always expands the most promising node with minimal $\hat{c}(x)$, pruning any branch whose lower bound exceeds the current global upper bound $U$ ($\hat{c}(x) \ge U$).

### Worked Example & Practical Simulation

LCBB **Execution** on **Minimization Problem**: • Initial Root **Bound**: c_hat(Root) = 25, Best U = infinity. • Expand Root -> Generates Children A(bound=28), B(bound=35), C(bound=29). • PQ **State**: {A:28, C:29, B:35}. Extract A (Lowest bound = 28). • Expand A -> Generates Leaf Sol1 with Cost = 30. Update Best U = 30! • Inspect B in PQ: Bound c_hat(B) = 35 >= U (30) -> **Node** B is PRUNED from Queue! • Extract C (bound 29 < 30) -> Generates leaves > 30. Search terminates with Optimal Cost = 30.

### Key Comparative Evaluation

| B&B Variant | Live Node Data Structure | Node Selection Order | Memory Usage Profile | Optimization Directness |
| :--- | :--- | :--- | :--- | :--- |
| FIFO Branch and Bound | Standard Queue | Breadth-First Search (BFS) | High (Stores full level) | Weak (Blind level exploration) |
| LIFO Branch and Bound | Standard Stack | Depth-First Search (DFS) | Moderate (Similar to backtracking) | Moderate (May get stuck in bad branches) |
| Least-Cost (LC) B&B | Min-Heap Priority Queue | Best-First Search (lowest bound) | Highest (Holds active frontier) | Maximum (Steers directly to optimal) |

### Concluding Exam Takeaway

**Branch and Bound** provides a mathematically rigorous **optimization** framework that uses priority-queue driven least-cost **search** to isolate global optima while pruning large swaths of **combinatorial** space. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 11: Branch and Bound, Sections 11.1–11.3, Pages 425–432.

---

## Q6. Compare backtracking with the branch and bound method with respect to search technique, exploration of state space tree, and kinds of problems that can be solved. (Frequency: 1) [8 Marks]

### Introduction & Core Intuition

**Backtracking** and **Branch and Bound** are the two primary state-space exploration paradigms in computer science for solving **combinatorial** problems that cannot be tackled in **polynomial time**. While both prune **infeasible** regions of a state tree, they differ fundamentally in their traversal techniques, memory management, and problem domains.

### **Comparison by Search Technique**

**Backtracking** strictly uses Depth-**First Search** (DFS) to traverse the **state space tree**. It advances down a single decision path, assigning values variable by variable, until a constraint violation or complete solution occurs. In contrast, **Branch and Bound** uses Breadth-First **Search** (FIFO) or Best-First / Least-Cost Search (LC). It evaluates all children of the active **node** simultaneously and uses an intelligent priority queue to decide which node across the entire tree to expand next.

### **Comparison by State Space Tree Exploration and Node Generation**

In **Backtracking**, the expanding **node** (**E-Node**) generates its children <i>lazily, one at a time</i>. The **algorithm** explores the first child's subtree completely before generating the second child. In **Branch and Bound**, the E-Node generates <i>all of its children eagerly at once</i>. Once all children are produced, the E-Node is discarded, and its children enter the global pool of live nodes. This means Branch and Bound explores the tree horizontally and heuristically across multiple frontiers.

### **Comparison by Kinds of Problems Solved**

**Backtracking** is primarily tailored for **Decision Problems** (determining if a valid configuration exists, such as N-Queens) and Enumeration Problems (finding all valid solutions, such as generating all Hamiltonian cycles or all subsets summing to M). It can solve **optimization problems**, but must **search** the entire **tree** to prove optimality. **Branch and Bound** is designed exclusively for Optimization Problems** (minimizing cost or maximizing profit, such as TSP or 0/1 Knapsack); it is rarely used for pure decision problems.

### **Comparison by Bounding and Pruning Mechanisms**

**Backtracking** prunes branches using a Feasibility **Bounding Function** (a Boolean predicate checking whether current constraints are satisfied). If the partial vector violates constraints, it backtracks. **Branch and Bound** prunes branches using a continuous Mathematical **Optimization** Bound (**lower bound** c_hat or upper bound u_hat). A node is pruned not because it is **infeasible**, but because its mathematical best-case potential cannot beat the currently known global solution.

### **Memory Consumption and Stack vs Heap Overhead**

Backtracking requires exceptionally minimal memory: it maintains only the active path from root to current node on the recursion stack, consuming strictly **O(depth) = O(n)** space. Branch and Bound maintains all active live nodes in a global priority queue in heap memory. Because the number of live nodes can grow exponentially with tree width, Branch and Bound consumes **O(2^n)** memory, making it highly vulnerable to memory exhaustion.

### Architectural Flow & System Diagram

![Figure 2.6: Compare backtracking with the branch and bound method with respect to search technique, exploration of state space tree, and kinds of problems that can be solved](diagrams/u2_q6.png)

*Figure 2.6: Architectural Flow and Conceptual System Diagram for Compare backtracking with the branch and bound method with respect to search technique, exploration of state space tree, and kinds of problems that can be solved*

### Mathematical Formulation & Algorithmic Mechanics

**Backtracking (DFS) vs Branch and Bound (LC) Decision Logic:**

```text
Backtracking Pruning Test: Valid(x[1..k]) == False  ==>  Backtrack to Parent
Branch and Bound Pruning Test (Minimization): c_hat(Node) ≥ U  ==>  Kill Node
Where: c_hat(Node) is the lower bound estimate and U is the best known complete tour cost.
```

The decision logic defined above contrasts Backtracking and Branch and Bound. Backtracking explores decision trees depth-first using an implicit recursion stack, requiring minimal $O(n)$ memory and operating primarily on constraint satisfaction problems. Branch and Bound uses breadth-first or best-first search via a min-heap priority queue, maintaining live nodes globally to solve optimization problems at the expense of exponential worst-case memory.

### Worked Example & Practical Simulation

Comparative Behavior on **0/1 Knapsack** Instance: • **Backtracking** Approach: Explores depth-first; packs Item 1, descends to Item 2, backtracks when weight exceeds capacity. Stores only 1 path in recursion stack (Memory = O(n) = few bytes). • **Branch and Bound** (LCBB) Approach: Generates both Include and Exclude children at once; computes **Fractional Knapsack** upper bound. Places both in **Priority Queue**. Explores node with highest bound first, pruning the other when its upper bound falls below current best profit.   Requires maintaining priority queue of live nodes in memory.

### Key Comparative Evaluation

| Comparative Dimension | Backtracking Paradigm | Branch and Bound Paradigm |
| :--- | :--- | :--- |
| Search Technique | Strict Depth-First Search (DFS) using recursion | Breadth-First (FIFO) or Best-First (Least-Cost Priority Queue) |
| Exploration Pattern | Vertical: dives deep along one path, backtracks on dead end | Horizontal / Heuristic: expands all children, hops across tree frontiers |
| Node Generation | Lazy: generates children one by one as needed | Eager: generates all children of E-node simultaneously |
| Problem Domains | Decision Problems, Constraint Satisfaction, Enumeration | Exclusively Optimization Problems (Minimization / Maximization) |

### Concluding Exam Takeaway

**Backtracking** trades computational time for minimal linear **memory** to solve decision and enumeration puzzles, whereas **Branch and Bound** consumes large heap memory to aggressively prune **optimization** trees via mathematical **bounds**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 10 (Backtracking) vs Chapter 11 (Branch and Bound), Pages 385–393 & 425–431.

---

## Q7. What are the drawbacks/major drawbacks of the branch and bound method? (Frequency: 2) [8 Marks]

### Introduction & Core Intuition

While **Branch and Bound** is a mathematically robust and widely utilized framework for finding exact optimal solutions to NP-hard **optimization problems**, it suffers from several severe computational and architectural drawbacks that limit its scalability on large real-world problem instances. This foundational concept enables engineers to **model** complex **system** dynamics and make **deterministic** decisions under real-world constraints.

### **1. Catastrophic Memory Consumption (Exponential Space Overhead)**

The single most crippling drawback of **Branch and Bound**—especially Least-Cost (LC) and FIFO variants—is its exponential memory consumption. Unlike **backtracking** which stores only a single linear path of depth O(n) on the recursion stack, Branch and **Bound** maintains all active, unexpanded **nodes** in a global **Priority Queue** in RAM. For complex **combinatorial** problems, the number of live nodes in the queue grows exponentially with problem size, rapidly exhausting system memory and causing Out-of-Memory (OOM) crashes.

### **2. High Computational Overhead of Bounding Functions**

At every branching step, the **algorithm** must compute a mathematical bound (such as solving a **Fractional Knapsack** relaxation, computing a minimum spanning tree, or reducing an entire cost **matrix**) for <i>every newly generated child **node**</i>. When an **E-node** generates dozens of children, computing complex bounding **functions** consumes significant CPU clock cycles. If the bounding calculations are computationally heavy, the total execution time can exceed that of simpler heuristic algorithms.

### **3. Worst-Case Exponential Time Complexity**

**Branch and Bound** is fundamentally an exact **algorithm** for **NP-hard** problems. In the worst case—such as when the **bounding function** is loose or when the problem instances exhibit flat cost landscapes where many **subproblems** share identical bounds—the bounding **function** fails to prune branches effectively. Under these conditions, the algorithm degenerates to an exhaustive brute-force search, examining O(2^n) or O(n!) nodes and taking days or weeks to terminate.

### **4. Extreme Sensitivity to the Quality of Bounding Functions**

The **performance** of **Branch and Bound** is hypersensitive to the tightness of the **bounding function**. A tight **bound** prunes aggressive volumes of the **state space tree** early, leading to fast termination. However, designing tight bounds that are simultaneously cheap to compute is extraordinarily difficult. A loose bound produces weak pruning, causing the **priority queue** to balloon uncontrollably.

### **5. Non-Suitability for Multi-Objective or Decision Problems**

Branch and Bound is engineered exclusively for single-objective numerical optimization (minimizing a scalar cost or maximizing a scalar profit). It cannot be applied directly to pure decision problems (such as determining satisfiability in Boolean SAT) or to multi-objective optimization where Pareto frontiers must be identified, severely restricting its versatility compared to constraint satisfaction engines.

### Architectural Flow & System Diagram

![Figure 2.7: What are the drawbacks/major drawbacks of the bran](diagrams/u2_q7.png)

*Figure 2.7: Architectural Flow and Conceptual System Diagram for What are the drawbacks/major drawbacks of the bran*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Characterization of Branch and Bound Bottlenecks:**

```text
Memory Space Complexity: S(n) = O( |Live Nodes in Queue| ) = O( b^d )   [Exponential!]
Total Time: T(n) = ∑_(v ∈ ExploredNodes) [ T_branch(v) + |Children(v)| · T_bound(child) ]
Pruning Inefficiency Condition: If c_hat(v) << TrueCost(v), Node v is NOT pruned, causing tree explosion.
```

The mathematical formulations defined above characterize the fundamental bottlenecks of Branch and Bound. While bounding functions aggressively prune sub-optimal regions, pathological instances force the algorithm to store an exponential number of live nodes in the priority queue ($O(2^n)$ heap memory). Calculating complex bounding functions at every branching step introduces substantial computational overhead that can exceed brute-force search.

### Worked Example & Practical Simulation

**Practical Demonstration** of B&B Failure on Large TSP Instance: • TSP with n = 40 Cities: Total Permutations = 39! / 2 ≈ 1.0 x 10^46 tours. **Branch and Bound** using reduced matrices generates over 50,000,000 **live nodes** within minutes. Each node stores an (n x n) reduced matrix = 40 x 40 x **4 bytes** ≈ 6.4 KB. Total RAM needed for **Priority Queue**: 50,000,000 x 6.4 KB ≈ 320 Gigabytes! (System Crashes). Demonstrates why industrial solvers switch to Genetic Algorithms, Simulated Annealing, or Christofides heuristics for n > 30.

### Key Comparative Evaluation

| Drawback Category | Underlying Technical Cause | Real-World System Impact | Mitigation Strategy |
| :--- | :--- | :--- | :--- |
| Space Explosion | Priority queue stores all active live nodes in RAM | Out-of-Memory crashes on large n | Switch to Depth-First B&B or Beam Search |
| Bounding Overhead | Calculating bounds (matrix reductions/relaxations) for every child | High CPU latency per expansion step | Cache intermediate calculations; use simpler bounds |
| Worst-Case Runtime | Loose bounds fail to eliminate sub-optimal paths | Exponential O(2^n) or O(n!) execution | Set time-out limits; accept near-optimal solution |
| Implementation Complexity | Requires custom priority queues, matrix copies, state objects | High code complexity and bug susceptibility | Use established solvers like CPLEX or Gurobi |

### Concluding Exam Takeaway

The primary drawbacks of **Branch and Bound** are its exponential **memory** consumption and computational bounding **overhead**, which cause it to exhaust **system** RAM or degenerate to brute force when bounding **functions** are loose.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 11: Branch and Bound, Section 11.2: Limitations and Complexity, Pages 428–431.

---

## Q8. Write a branch and bound algorithm for solving the 0/1 Knapsack problem and use the LC (Least Cost) branch and bound method to solve given knapsack instances. (Frequency: 5) [8 Marks]

### Introduction & Core Intuition

Solving the **0/1 Knapsack** Problem using Least-Cost **Branch and Bound** (LCBB) involves converting the maximization problem into a minimization framework and utilizing the continuous **Fractional Knapsack** solution as an upper **bounding function** to prune inferior decision subtrees. This foundational concept enables engineers to model complex system dynamics and make **deterministic** decisions under real-world constraints.

### **Mathematical Formulation as a Minimization Problem**

Standard **Branch and Bound** formulations are designed for minimization. To solve the maximization **0/1 Knapsack** problem (maximizing profit P = sum(pi * xi)), we negate the objective function: minimize -P. The upper bound on profit calculated by the **Fractional Knapsack** relaxation corresponds to the lowest **lower bound** on -P, establishing the **bounding function** c_hat(x).

### **Bounding Function Computation via Fractional Relaxation**

**Items** are **sorted** in non-**increasing** **order** of their **value**-to-weight ratio: p1/w1 >= p2/w2 >= ... >= pn/wn. For any **node** at depth i with accumulated weight W_curr and profit P_curr: (1) Pack as many remaining items i+1, i+2, ... fully as capacity allows; (2) When the next item cannot fit completely, take the allowable fraction of its weight to completely saturate the knapsack capacity W; (3) The resulting total profit represents the **Upper Bound (UB)**. A node's bound is: c_hat(Node) = -(P_curr + fractional profit).

### **LC Branch and Bound Algorithmic Protocol**

A **priority queue** (**min-heap**) stores **live nodes** ranked by c_hat. The variable U records the best (maximum) profit achieved by any **feasible** integer solution found so far (initialized to 0, or -U_profit in minimization). In each iteration, the **node** with minimum c_hat (highest profit upper **bound**) is extracted: (1) Branch on item i+1 to generate Left Child (xi+1 = 1) and Right Child (xi+1 = 0); (2) Left child is valid only if W_curr + wi+1 <= W; (3) For each child, compute its upper bound UB; (4) If UB <= U, the child cannot beat the best known integer solution and is **pruned immediately**; otherwise, it is inserted into the priority queue.

### **Complete Numerical Walkthrough for SPPU Exam Instance**

**Consider** an SPPU **benchmark** **instance**: n = 4 **items**, **Capacity** W = 15 kg. Items sorted by ratio: I1(p=10, w=2, ratio=5), I2(p=10, w=4, ratio=2.5), I3(p=12, w=6, ratio=2), I4(p=18, w=9, ratio=2).<br/>• **Root Node** 1 (Level 0, Weight=0, Profit=0): Pack I1(w=2), I2(w=4), I3(w=6) -> total weight 12, profit 32. Remaining capacity = 3 kg. Take 3/9 of I4: profit = 32 + (3/9)*18 = 32 + 6 = 38. Upper Bound UB(Root) = 38 (c_hat = -38). • Branch on Item 1: Node 2 (Include I1, w=2, p=10) has UB = 38. Node 3 (Exclude I1, w=0, p=0): packs I2(4)+I3(6)=10 kg, takes 5/9 of I4: UB = 10 + 12 + (5/9)*18 = 22 + 10 = 32. Since 38 > 32, Node 2 is expanded first! • From Node 2, branch on Item 2: Node 4 (Include I2, w=6, p=20) has UB = 38. Continue expanding. When we reach Node with {I1, I2, I4}, Weight = 2 + 4 + 9 = 15 kg, Profit = 10 + 10 + 18 = 38 (Feasible integer solution!). Best U = 38. • Node 3 with UB = 32 is pruned because 32 < 38. Optimal solution found: {I1, I2, I4} with Maximum Profit = 38.

### **Efficiency and Space Trade-Off**

By utilizing the continuous fractional knapsack upper bound, LCBB explores only 7 nodes instead of all 2^(4+1) - 1 = 31 nodes in the full tree, demonstrating significant pruning efficiency.

### Architectural Flow & System Diagram

![Figure 2.8: a branch and bound algorithm for solving the 0/1 Knapsack problem and use the LC (Least Cost) branch and bound method to solve given knapsack instances](diagrams/u2_q8.png)

*Figure 2.8: Architectural Flow and Conceptual System Diagram for a branch and bound algorithm for solving the 0/1 Knapsack problem and use the LC (Least Cost) branch and bound method to solve given knapsack instances*

### Mathematical Formulation & Algorithmic Mechanics

**LCBB Knapsack Bounding Formulation:**

```text
Upper Bound Formulation: UB(Node) = P_curr + ∑_(j=i+1)^(k-1) p_j + ( W - W_packed ) · ( p_k / w_k )
Minimization Cost Bound: c_hat(Node) = - UB(Node)
Pruning Condition: If UB(Child) ≤ Current_Best_Integer_Profit  ==>  PRUNE / KILL Child
```

The Least-Cost Branch and Bound formulation defined above solves the 0/1 Knapsack problem. For each node in the state-space tree, an upper bound on achievable profit is calculated by relaxing the remaining capacity to fractional knapsack selection. If a node's upper bound falls below the current best known integer solution ($UB(x) \le 	ext{BestProfit}$), the node is killed immediately, pruning sub-optimal decision branches.

### Worked Example & Practical Simulation

**Summary Table** of **Nodes** Generated for **Knapsack** **Instance** (W = 15): • Node 1 (Root): **Items** **remaining** {1,2,3,4} -> UB = 38, c_hat = -38 (Enqueued). • Node 2 (x1 = 1): Weight = 2, Profit = 10 -> UB = 38, c_hat = -38 (Extracted as best). • Node 3 (x1 = 0): Weight = 0, Profit = 0 -> UB = 32, c_hat = -32 (PRUNED; cannot beat 38). • Node 4 (x1 = 1, x2 = 1): Weight = 6, Profit = 20 -> UB = 38. • Leaf Node (x1=1, x2=1, x3=0, x4=1): Weight = 15, Profit = 38 (Optimal Integer Solution!).

### Key Comparative Evaluation

| Node ID | Decision State | Accumulated Weight | Accumulated Profit | Upper Bound (UB) | Action Taken |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Node 1 | Root (Empty) | 0 kg | 0 | 38 | Expanded (Initial E-Node) |
| Node 2 | x1 = 1 (Include I1) | 2 kg | 10 | 38 | Expanded (Highest UB in PQ) |
| Node 3 | x1 = 0 (Exclude I1) | 0 kg | 0 | 32 | PRUNED (UB 32 <= Best Integer 38) |
| Node 4 | x1=1, x2=1 | 6 kg | 20 | 38 | Expanded |

### Concluding Exam Takeaway

LC **Branch and Bound** solves the **0/1 Knapsack** problem by **leveraging** **fractional** upper **bounds** to **steer** exploration directly toward the optimal combination {I1, I2, I4} while pruning inferior branches.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 11: Branch and Bound, Section 11.4: 0/1 Knapsack using LCBB, Pages 432–439.

---

## Q9. Apply the branch and bound algorithmic strategy to solve the traveling salesman problem for given cost/distance matrices to find the shortest possible route. (Frequency: 3) [8 Marks]

### Introduction & Core Intuition

**The Traveling Salesperson Problem** (TSP) requires finding the minimum-cost **Hamiltonian cycle** that visits every city in a given graph exactly once and returns to the starting vertex. Solving TSP using **Branch and Bound** relies on Reduced Cost Matrices to calculate rigorous lower **bounds** at each **node** of the **state space tree**.

### **The Concept of a Reduced Cost Matrix**

A cost **matrix** M is **defined** as **Reduced** if every row **contains** at **least** one zero and every **column** contains at least one zero, with all remaining entries non-negative. Subtracting a constant r from all elements of row i reduces the total cost of any complete tour by exactly r, because every tour must depart city i exactly once. Similarly, subtracting c from column j reduces every tour cost by c, because every tour must enter city j exactly once. The sum of all subtracted row and column reduction constants represents a guaranteed **Lower Bound** on the cost of any tour in that subproblem.

### **Root Lower Bound Computation Protocol**

**Given** an **initial** n x n distance **matrix** with M[i][i] = **infinity**: (1) **Row Reduction**: For each row i, identify the minimum element row_min = min_j(M[i][j]); if row_min > 0 and row_min != infinity, subtract row_min from every **entry** in row i; (2) Column Reduction: In the resulting matrix, identify the minimum element col_min = min_i(M[i][j]) for each column j; if col_min > 0 and col_min != infinity, subtract col_min from every entry in column j. (3) The lower bound for the root node is: c_hat(Root) = sum(row_min) + sum(col_min).

### **State Space Tree Branching and Matrix Modification**

**Suppose** the current **node** has reduced **matrix** M and **lower bound** c_hat, and we **branch** from city i to city j (incorporating edge (i, j) into the tour): (1) Row i is set to infinity (preventing other departures from city i); (2) Column j is set to infinity (preventing other arrivals into city j); (3) **Entry** M[j][1] is set to infinity (preventing premature return to start); (4) Reduce the resulting matrix, summing all new reduction constants r_new; (5) The lower bound for the child node is: c_hat(Child) = c_hat(Parent) + M[i][j] + r_new.

### **Least-Cost Node Selection and Pruning**

All **live nodes** are maintained in a min-heap **priority queue** ranked by c_hat. In each iteration, LCBB extracts the **node** with the lowest **lower bound**. If a leaf node representing a complete tour is reached with cost L, the global upper **bound** is updated to L. Any existing or newly generated node whose lower bound satisfies c_hat >= L is pruned immediately, because it cannot possibly yield a cheaper tour.

### **Concrete Numerical Walkthrough for SPPU Exam Instance**

Consider a 4-city instance with initial matrix: Row 1: [inf, 10, 15, 20] -> min = 10 -> [inf, 0, 5, 10] Row 2: [5, inf, 9, 10]   -> min = 5  -> [0, inf, 4, 5] Row 3: [6, 13, inf, 12]  -> min = 6  -> [0, 7, inf, 6] Row 4: [8, 8, 9, inf]    -> min = 8  -> [0, 0, 1, inf] Sum of row reductions = 10 + 5 + 6 + 8 = 29. Columns: Col 1 has 0, Col 2 has 0, Col 3 min is 1, Col 4 min is 5. Sum of col reductions = 1 + 5 = 6. Root Lower Bound c_hat(Root) = 29 + 6 = 35. Branching on edge (1, 2): Entry M[1][2] = 0. Set row 1 = inf, col 2 = inf, M[2][1] = inf. Reducing yields r_new = 0. Child bound = 35 + 0 + 0 = 35. Following this path yields the optimal tour <1 -> 2 -> 4 -> 3 -> 1> with Total Minimum Cost = 35.

### Architectural Flow & System Diagram

![Figure 2.9: Apply the branch and bound algorithmic strategy to solve the traveling salesman problem for given cost/distance matrices to find the shortest possible route](diagrams/u2_q9.png)

*Figure 2.9: Architectural Flow and Conceptual System Diagram for Apply the branch and bound algorithmic strategy to solve the traveling salesman problem for given cost/distance matrices to find the shortest possible route*

### Mathematical Formulation & Algorithmic Mechanics

**TSP Matrix Reduction and Child Lower Bound Formula:**

```text
Root Lower Bound: c_hat(Root) = ∑_(i=1)^n min_row(i) + ∑_(j=1)^n min_col(j)
Child Lower Bound: c_hat(Child_(i→j)) = c_hat(Parent) + M[i, j] + r_reduction
Matrix Masking: Set Row i = ∞, Column j = ∞, Entry M[j, 1] = ∞
```

The matrix reduction and lower-bound formulation defined above solves the Traveling Salesman Problem via LCBB. At each node, the cost matrix is reduced by subtracting row and column minima, with the sum of reductions defining a lower bound $c(R)$ on tour cost. Branching to include edge $(i, j)$ computes the child lower bound $\hat{c}(	ext{child}) = \hat{c}(	ext{parent}) + A[i, j] + r$, directing best-first search toward the optimal Hamiltonian tour.

### Worked Example & Practical Simulation

Trace of **Lower Bound** Calculation for **Child Node** (1 -> 2): • Parent **Bound** c_hat(1) = 35, Cost **Matrix** Entry M[1, 2] = 0. • Masking: Row 1 = [inf, inf, inf, inf], Col 2 = [inf, inf, inf, inf], M[2, 1] = inf. • Reduction on remaining sub-matrix yields additional reduction cost r_new = 0. • Child Lower Bound c_hat(**Node** 2) = 35 + 0 + 0 = 35. • Alternative Child (1 -> 3): M[1, 3] = 4, r_new = 11 -> c_hat = 35 + 4 + 11 = 50. Since 35 < 50, LCBB prioritizes Path (1 -> 2), pruning the expensive (1 -> 3) branch.

### Key Comparative Evaluation

| Tour Branch | Matrix Entry M[i, j] | Additional Reduction Cost | Calculated Lower Bound c_hat | Search Status |
| :--- | :--- | :--- | :--- | :--- |
| Node 1 (Root) | - | Row: 29, Col: 6 | 35 | Expanded (Initial E-Node) |
| Node 2 (Path 1->2) | M[1, 2] = 0 | 0 | 35 | Selected for Expansion (Lowest Bound) |
| Node 3 (Path 1->3) | M[1, 3] = 4 | 11 | 50 | Pruned (Bound 50 > Tour 35) |
| Node 4 (Path 1->4) | M[1, 4] = 5 | 8 | 48 | Pruned (Bound 48 > Tour 35) |

### Concluding Exam Takeaway

**Branch and Bound** solves the **Traveling Salesperson Problem** to **global** **optimality** by using **matrix** reductions to dynamically lower-**bound** candidate routes, pruning sub-optimal tours and isolating the 35-cost cycle.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 11: Branch and Bound, Section 11.5: Traveling Salesperson Problem, Pages 439–448.

---
