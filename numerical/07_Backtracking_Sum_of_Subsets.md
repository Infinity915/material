# Topic 07: Sum of Subsets Problem (Backtracking)

This document provides complete, step-by-step mathematical solutions and state-space trees for every **Sum of Subsets** numerical problem appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition
Given a set of $n$ positive integers $W = \{w_1, w_2, \dots, w_n\}$ sorted in non-decreasing order:
$$w_1 \le w_2 \le \dots \le w_n$$
and a positive integer target sum $M$.

The objective is to find all subsets of $W$ whose elements sum to exactly $M$. The solution is represented as an $n$-tuple binary vector $X = (x_1, x_2, \dots, x_n)$ where:
$$x_i = \begin{cases} 1 & \text{if } w_i \text{ is included in the subset} \\ 0 & \text{if } w_i \text{ is excluded} \end{cases}$$
such that:
$$\sum_{i=1}^n w_i x_i = M$$

### 1.2 Bounding Functions (Pruning Criteria)
Let:
- $s = \sum_{i=1}^{k-1} w_i x_i$ be the sum of elements selected so far.
- $r = \sum_{i=k}^n w_i$ be the sum of all remaining candidate elements.

At level $k$ in the state-space tree:
1. **Target Met (Solution Leaf):**
   If $s + w_k = M$, then $(x_1, \dots, x_k = 1, x_{k+1} = 0, \dots, x_n = 0)$ is a valid solution.
2. **Left Branch Feasibility ($x_k = 1$, include $w_k$):**
   Explore the left subtree only if:
   $$s + w_k \le M \quad \text{and} \quad s + w_k + w_{k+1} \le M \quad (\text{if } k < n)$$
   *(If $s + w_k > M$, prune immediately because all subsequent weights are even larger).*
3. **Right Branch Feasibility ($x_k = 0$, exclude $w_k$):**
   Explore the right subtree only if:
   $$s + (r - w_k) \ge M$$
   *(If the sum of all remaining elements cannot even reach $M$, prune immediately).*

### 1.3 Complexity Analysis
- **Worst-case Time Complexity:** $O(2^n)$ in the unpruned binary tree.
- **Pruned Search Time:** Substantially faster than $O(2^n)$ in practice due to early bounding and cutoff.
- **Space Complexity:** $O(n)$ recursion stack depth.

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU May/June 2023 & Nov/Dec 2023 [8 Marks]

#### Problem Statement
> Given a set $S = \{2, 3, 5, 6, 8, 10\}$ and target sum $M = 10$. Solve using Backtracking.
> Draw the state-space tree and find all subsets that sum to 10.

---

#### Step 1: Input Setup & Suffix Sum Initialization
- $w = \langle 2, 3, 5, 6, 8, 10 \rangle$, $n = 6$, $M = 10$.
- Total sum: $r = \sum_{i=1}^6 w_i = 2 + 3 + 5 + 6 + 8 + 10 = 34$.
- Since $34 \ge 10$ and $w_1 = 2 \le 10$, a solution is possible.

---

#### Step 2: Systematic State-Space Tree Exploration

```
                         Root (s=0, r=34)
                        /                \
             Include w1=2                 Exclude w1=2
           (s=2, r=32)                    (s=0, r=32)
          /           \                  /           \
     Inc w2=3       Exc w2=3        Inc w2=3       Exc w2=3
    (s=5, r=29)    (s=2, r=29)     (s=3, r=29)    (s=0, r=29)
    /         \    /         \     /         \          :
Inc w3=5   Exc w3=5           :   :           :         :
s=10 [SOL 1]  s=5
```

Let us trace each branch rigorously:

1. **Path with $w_1 = 2$ included ($s = 2$):**
   - **Include $w_2 = 3$ ($s = 5$):**
     - **Include $w_3 = 5$:** $s = 5 + 5 = 10 = M$.
       $$\mathbf{\text{SOLUTION 1 FOUND: } \{2, 3, 5\}}, \quad X = (1, 1, 1, 0, 0, 0)$$
     - **Exclude $w_3 = 5$ ($s = 5$):**
       - Try $w_4 = 6$: $s + w_4 = 5 + 6 = 11 > 10$ $\implies$ **PRUNE** (exceeds $M$).
   - **Exclude $w_2 = 3$ ($s = 2$):**
     - **Include $w_3 = 5$ ($s = 7$):**
       - Try $w_4 = 6$: $s + w_4 = 7 + 6 = 13 > 10$ $\implies$ **PRUNE**.
     - **Exclude $w_3 = 5$ ($s = 2$):**
       - Try $w_4 = 6$: $s + w_4 = 2 + 6 = 8 \le 10$.
         - Try $w_5 = 8$: $s + w_5 = 8 + 8 = 16 > 10$ $\implies$ **PRUNE**.
         - Exclude $w_5$: remaining $w_6 = 10 \implies 8 + 10 = 18 > 10 \implies$ **PRUNE**.
       - Exclude $w_4$:
         - **Include $w_5 = 8$:** $s = 2 + 8 = 10 = M$.
           $$\mathbf{\text{SOLUTION 2 FOUND: } \{2, 8\}}, \quad X = (1, 0, 0, 0, 1, 0)$$
         - Exclude $w_5$:
           - Try $w_6 = 10$: $s + w_6 = 2 + 10 = 12 > 10 \implies$ **PRUNE**.

2. **Path with $w_1 = 2$ excluded ($s = 0$):**
   - **Include $w_2 = 3$ ($s = 3$):**
     - Include $w_3 = 5$ ($s = 8$):
       - Try $w_4 = 6$: $8 + 6 = 14 > 10 \implies$ **PRUNE**.
       - Exclude $w_4$: remaining $r - w_4 = 34 - 2 - 3 - 5 - 6 = 18$. Try $w_5=8$: $8 + 8 = 16 > 10 \implies$ **PRUNE**.
     - Exclude $w_3 = 5$ ($s = 3$):
       - Try $w_4 = 6$ ($s = 9$):
         - Try $w_5 = 8$: $9 + 8 = 17 > 10 \implies$ **PRUNE**.
       - Exclude $w_4$:
         - Try $w_5 = 8$: $3 + 8 = 11 > 10 \implies$ **PRUNE**.
   - **Exclude $w_2 = 3$ ($s = 0$):**
     - Include $w_3 = 5$ ($s = 5$):
       - Try $w_4 = 6$: $5 + 6 = 11 > 10 \implies$ **PRUNE**.
       - Try $w_5 = 8$: $5 + 8 = 13 > 10 \implies$ **PRUNE**.
     - Exclude $w_3 = 5$ ($s = 0$):
       - Include $w_4 = 6$ ($s = 6$):
         - Try $w_5 = 8$: $6 + 8 = 14 > 10 \implies$ **PRUNE**.
       - Exclude $w_4 = 6$ ($s = 0$):
         - Include $w_5 = 8$ ($s = 8$):
           - Try $w_6 = 10$: $8 + 10 = 18 > 10 \implies$ **PRUNE**.
         - Exclude $w_5 = 8$ ($s = 0$):
           - **Include $w_6 = 10$:** $s = 0 + 10 = 10 = M$.
             $$\mathbf{\text{SOLUTION 3 FOUND: } \{10\}}, \quad X = (0, 0, 0, 0, 0, 1)$$

---

#### Step 3: Summary of Solutions for Instance A
Three valid subsets sum to $M = 10$:
1. **Subset 1:** $\{2, 3, 5\} \implies 2 + 3 + 5 = 10$, Vector: $(1, 1, 1, 0, 0, 0)$
2. **Subset 2:** $\{2, 8\} \implies 2 + 8 = 10$, Vector: $(1, 0, 0, 0, 1, 0)$
3. **Subset 3:** $\{10\} \implies 10 = 10$, Vector: $(0, 0, 0, 0, 0, 1)$

---

### Instance B: SPPU May/June 2024 & 2024/2025 [10 Marks]

#### Problem Statement
> Solve the Sum of Subsets problem using Backtracking:
> - Set: $A = \{5, 10, 15, 20, 25\}$
> - Target Sum: $M = 30$
>
> Construct the complete state-space tree and output all feasible subsets.

---

#### Step 1: Input Setup & Parameter Checks
- $w = \langle 5, 10, 15, 20, 25 \rangle$, $n = 5$, $M = 30$.
- Total sum: $r = 5 + 10 + 15 + 20 + 25 = 75$.
- Check: $75 \ge 30$, and $w_1 = 5 \le 30$. Search begins.

---

#### Step 2: Step-by-Step Backtracking Tree Exploration

```
                                    Root (s=0, r=75)
                                   /                \
                       Include w1=5                  Exclude w1=5
                       (s=5, r=70)                   (s=0, r=70)
                      /           \                 /           \
                 Inc w2=10      Exc w2=10      Inc w2=10      Exc w2=10
                (s=15, r=60)   (s=5, r=60)    (s=10, r=60)   (s=0, r=60)
                /          \
           Inc w3=15     Exc w3=15
          s=30 [SOL 1]  (s=15, r=45)
```

1. **Subtree 1: Include $w_1 = 5$ ($s = 5$):**
   - **Include $w_2 = 10$ ($s = 15$):**
     - **Include $w_3 = 15$:** $s = 15 + 15 = 30 = M$.
       $$\mathbf{\text{SOLUTION 1: } \{5, 10, 15\}}, \quad X = (1, 1, 1, 0, 0)$$
     - **Exclude $w_3 = 15$ ($s = 15$):**
       - Try $w_4 = 20$: $s + w_4 = 15 + 20 = 35 > 30 \implies$ **PRUNE**.
   - **Exclude $w_2 = 10$ ($s = 5$):**
     - **Include $w_3 = 15$ ($s = 20$):**
       - Try $w_4 = 20$: $20 + 20 = 40 > 30 \implies$ **PRUNE**.
     - **Exclude $w_3 = 15$ ($s = 5$):**
       - **Include $w_4 = 20$ ($s = 25$):**
         - Try $w_5 = 25$: $25 + 25 = 50 > 30 \implies$ **PRUNE**.
       - **Exclude $w_4 = 20$ ($s = 5$):**
         - **Include $w_5 = 25$:** $s = 5 + 25 = 30 = M$.
           $$\mathbf{\text{SOLUTION 2: } \{5, 25\}}, \quad X = (1, 0, 0, 0, 1)$$

2. **Subtree 2: Exclude $w_1 = 5$ ($s = 0$):**
   - **Include $w_2 = 10$ ($s = 10$):**
     - **Include $w_3 = 15$ ($s = 25$):**
       - Try $w_4 = 20$: $25 + 20 = 45 > 30 \implies$ **PRUNE**.
     - **Exclude $w_3 = 15$ ($s = 10$):**
       - **Include $w_4 = 20$:** $s = 10 + 20 = 30 = M$.
         $$\mathbf{\text{SOLUTION 3: } \{10, 20\}}, \quad X = (0, 1, 0, 1, 0)$$
       - **Exclude $w_4 = 20$ ($s = 10$):**
         - Try $w_5 = 25$: $10 + 25 = 35 > 30 \implies$ **PRUNE**.
   - **Exclude $w_2 = 10$ ($s = 0$):**
     - Include $w_3 = 15$ ($s = 15$):
       - Try $w_4 = 20$: $15 + 20 = 35 > 30 \implies$ **PRUNE**.
     - Exclude $w_3 = 15$ ($s = 0$):
       - Include $w_4 = 20$ ($s = 20$):
         - Try $w_5 = 25$: $20 + 25 = 45 > 30 \implies$ **PRUNE**.
       - Exclude $w_4 = 20$ ($s = 0$):
         - Include $w_5 = 25$: $s = 25 < 30$, no elements left $\implies$ **TERMINATE**.

---

#### Step 3: Summary of Solutions for Instance B
Three valid subsets sum to $M = 30$:
1. **Subset 1:** $\{5, 10, 15\} \implies 5 + 10 + 15 = 30$, Solution vector: $(1, 1, 1, 0, 0)$
2. **Subset 2:** $\{5, 25\} \implies 5 + 25 = 30$, Solution vector: $(1, 0, 0, 0, 1)$
3. **Subset 3:** $\{10, 20\} \implies 10 + 20 = 30$, Solution vector: $(0, 1, 0, 1, 0)$
