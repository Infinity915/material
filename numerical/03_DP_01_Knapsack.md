# Topic 03: 0/1 Knapsack Problem (Dynamic Programming)

This document provides complete, step-by-step mathematical solutions for every **0/1 Knapsack (Dynamic Programming)** numerical problem appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Recurrence Relation

### 1.1 Problem Definition
Given $n$ items, each with:
- A weight $w_i \in \mathbb{Z}^+$
- A profit/value $p_i \in \mathbb{Z}^+$

And a knapsack of maximum weight capacity $W \in \mathbb{Z}^+$. The objective is to select a subset of items to maximize the total profit without exceeding the capacity $W$. Each item can either be included once ($x_i = 1$) or not included ($x_i = 0$):

$$\text{Maximize } \sum_{i=1}^n p_i x_i \quad \text{subject to } \sum_{i=1}^n w_i x_i \le W, \quad x_i \in \{0, 1\}$$

### 1.2 Dynamic Programming Recurrence
Let $V[i, w]$ be the maximum profit obtainable using a subset of the first $i$ items ($1 \le i \le n$) and a maximum weight limit $w$ ($0 \le w \le W$).

The Bellman recurrence equation is:
$$V[i, w] = \begin{cases} 
0 & \text{if } i = 0 \text{ or } w = 0 \\
V[i-1, w] & \text{if } w < w_i \quad (\text{item } i \text{ exceeds current capacity}) \\
\max\Big(V[i-1, w],\; V[i-1, w - w_i] + p_i\Big) & \text{if } w \ge w_i \quad (\text{item } i \text{ can be included})
\end{cases}$$

### 1.3 Backtracking to Find Selected Items
Starting from $i = n$ and $w = W$:
1. If $V[i, w] = V[i-1, w]$, item $i$ was **not included** ($x_i = 0$). Move to $V[i-1, w]$.
2. If $V[i, w] \ne V[i-1, w]$, item $i$ was **included** ($x_i = 1$). Move to $V[i-1, w - w_i]$ and decrease current weight $w$ by $w_i$.
3. Repeat until $i = 0$ or $w = 0$.

### 1.4 Complexity Analysis
- **Time Complexity:** $\Theta(n \cdot W)$ pseudo-polynomial time.
- **Space Complexity:** $\Theta(n \cdot W)$ for the 2D DP table (can be optimized to $\Theta(W)$ using 1D rolling array if item identification is not needed).

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU May/June 2023 & Nov/Dec 2023 [10 Marks]

#### Problem Statement
> Solve the following 0/1 Knapsack problem using Dynamic Programming:
> - Capacity of Knapsack: $W = 5$
> - Number of items: $n = 4$
> - Item weights and profits:
>   - Item 1: $w_1 = 2, p_1 = 12$
>   - Item 2: $w_2 = 1, p_2 = 10$
>   - Item 3: $w_3 = 3, p_3 = 20$
>   - Item 4: $w_4 = 2, p_4 = 15$
>
> Construct the DP table, find the maximum profit, and determine the exact items included.

---

#### Step 1: Initialize Table Dimensions
Table size: $(n + 1) \times (W + 1) = 5 \times 6$, where rows $i \in \{0, 1, 2, 3, 4\}$ and columns $w \in \{0, 1, 2, 3, 4, 5\}$.

Base row: $V[0, w] = 0$ for all $w \in \{0, 1, 2, 3, 4, 5\}$.
Base column: $V[i, 0] = 0$ for all $i \in \{0, 1, 2, 3, 4\}$.

---

#### Step 2: Row-by-Row Cell Computations

**Row 1: Item 1 ($w_1 = 2, p_1 = 12$)**
- $w = 0$: $V[1, 0] = 0$
- $w = 1$: $1 < 2 \implies V[1, 1] = V[0, 1] = 0$
- $w = 2$: $2 \ge 2 \implies \max(V[0, 2], V[0, 0] + 12) = \max(0, 12) = 12$
- $w = 3$: $3 \ge 2 \implies \max(V[0, 3], V[0, 1] + 12) = \max(0, 12) = 12$
- $w = 4$: $4 \ge 2 \implies \max(V[0, 4], V[0, 2] + 12) = \max(0, 12) = 12$
- $w = 5$: $5 \ge 2 \implies \max(V[0, 5], V[0, 3] + 12) = \max(0, 12) = 12$

**Row 2: Item 2 ($w_2 = 1, p_2 = 10$)**
- $w = 0$: $V[2, 0] = 0$
- $w = 1$: $1 \ge 1 \implies \max(V[1, 1], V[1, 0] + 10) = \max(0, 10) = 10$
- $w = 2$: $2 \ge 1 \implies \max(V[1, 2], V[1, 1] + 10) = \max(12, 0 + 10) = 12$
- $w = 3$: $3 \ge 1 \implies \max(V[1, 3], V[1, 2] + 10) = \max(12, 12 + 10) = 22$
- $w = 4$: $4 \ge 1 \implies \max(V[1, 4], V[1, 3] + 10) = \max(12, 12 + 10) = 22$
- $w = 5$: $5 \ge 1 \implies \max(V[1, 5], V[1, 4] + 10) = \max(12, 12 + 10) = 22$

**Row 3: Item 3 ($w_3 = 3, p_3 = 20$)**
- $w = 0$: $V[3, 0] = 0$
- $w = 1$: $1 < 3 \implies V[3, 1] = V[2, 1] = 10$
- $w = 2$: $2 < 3 \implies V[3, 2] = V[2, 2] = 12$
- $w = 3$: $3 \ge 3 \implies \max(V[2, 3], V[2, 0] + 20) = \max(22, 0 + 20) = 22$
- $w = 4$: $4 \ge 3 \implies \max(V[2, 4], V[2, 1] + 20) = \max(22, 10 + 20) = 30$
- $w = 5$: $5 \ge 3 \implies \max(V[2, 5], V[2, 2] + 20) = \max(22, 12 + 20) = 32$

**Row 4: Item 4 ($w_4 = 2, p_4 = 15$)**
- $w = 0$: $V[4, 0] = 0$
- $w = 1$: $1 < 2 \implies V[4, 1] = V[3, 1] = 10$
- $w = 2$: $2 \ge 2 \implies \max(V[3, 2], V[3, 0] + 15) = \max(12, 0 + 15) = 15$
- $w = 3$: $3 \ge 2 \implies \max(V[3, 3], V[3, 1] + 15) = \max(22, 10 + 15) = 25$
- $w = 4$: $4 \ge 2 \implies \max(V[3, 4], V[3, 2] + 15) = \max(30, 12 + 15) = 30$
- $w = 5$: $5 \ge 2 \implies \max(V[3, 5], V[3, 3] + 15) = \max(32, 22 + 15) = 37$

---

#### Step 3: Complete Dynamic Programming Table

| $i \ \backslash \ w$ | $w = 0$ | $w = 1$ | $w = 2$ | $w = 3$ | $w = 4$ | $w = 5$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0 (None)** | 0 | 0 | 0 | 0 | 0 | 0 |
| **1 ($w_1=2, p_1=12$)** | 0 | 0 | 12 | 12 | 12 | 12 |
| **2 ($w_2=1, p_2=10$)** | 0 | 10 | 12 | 22 | 22 | 22 |
| **3 ($w_3=3, p_3=20$)** | 0 | 10 | 12 | 22 | 30 | 32 |
| **4 ($w_4=2, p_4=15$)** | 0 | 10 | 15 | 25 | 30 | **37** |

---

#### Step 4: Backtracking to Identify Selected Items
1. Start at cell $V[4, 5] = 37$:
   - Compare with $V[3, 5] = 32$.
   - Since $V[4, 5] \ne V[3, 5]$, **Item 4 is SELECTED** ($x_4 = 1$).
   - Remaining capacity $= 5 - w_4 = 5 - 2 = 3$.
2. Inspect cell $V[3, 3] = 22$:
   - Compare with $V[2, 3] = 22$.
   - Since $V[3, 3] = V[2, 3]$, **Item 3 is NOT SELECTED** ($x_3 = 0$).
   - Remaining capacity remains $3$.
3. Inspect cell $V[2, 3] = 22$:
   - Compare with $V[1, 3] = 12$.
   - Since $V[2, 3] \ne V[1, 3]$, **Item 2 is SELECTED** ($x_2 = 1$).
   - Remaining capacity $= 3 - w_2 = 3 - 1 = 2$.
4. Inspect cell $V[1, 2] = 12$:
   - Compare with $V[0, 2] = 0$.
   - Since $V[1, 2] \ne V[0, 2]$, **Item 1 is SELECTED** ($x_1 = 1$).
   - Remaining capacity $= 2 - w_1 = 2 - 2 = 0$.
5. Capacity reached $0$. Terminate backtracking.

#### Step 5: Final Result
- **Optimal Items Selected:** $\{1, 2, 4\}$
- **Solution Vector:** $(x_1, x_2, x_3, x_4) = (1, 1, 0, 1)$
- **Total Weight:** $w_1 + w_2 + w_4 = 2 + 1 + 2 = 5 \le 5$
- **Maximum Profit:**
  $$\text{Total Profit} = p_1 + p_2 + p_4 = 12 + 10 + 15 = \mathbf{37}$$

---

### Instance B: SPPU Nov/Dec 2024 [10 Marks]

#### Problem Statement
> Given a knapsack of capacity $W = 7$ and four items with the following weights and values:
> - Item A: $w_A = 1, v_A = 1$
> - Item B: $w_B = 3, v_B = 4$
> - Item C: $w_C = 4, v_C = 5$
> - Item D: $w_D = 5, v_D = 7$
>
> Solve using Dynamic Programming. Construct the DP table and trace the optimal solution.

---

#### Step 1: Complete DP Table Computation

Table dimensions: $(4 + 1) \times (7 + 1) = 5 \times 8$.

- **Row 0:** $V[0, w] = 0$ for all $w \in [0, 7]$.
- **Row 1 (A: $w=1, v=1$):**
  - For $w = 0$: $0$
  - For $w = 1..7$: $\max(0, 0 + 1) = 1$
- **Row 2 (B: $w=3, v=4$):**
  - $w < 3$: copy from Row 1 $\implies [0, 1, 1]$
  - $w = 3$: $\max(1, 0 + 4) = 4$
  - $w = 4$: $\max(1, 1 + 4) = 5$
  - $w = 5$: $\max(1, 1 + 4) = 5$
  - $w = 6$: $\max(1, 1 + 4) = 5$
  - $w = 7$: $\max(1, 1 + 4) = 5$
- **Row 3 (C: $w=4, v=5$):**
  - $w < 4$: copy from Row 2 $\implies [0, 1, 1, 4]$
  - $w = 4$: $\max(5, V[2, 0] + 5) = \max(5, 5) = 5$
  - $w = 5$: $\max(5, V[2, 1] + 5) = \max(5, 1 + 5) = 6$
  - $w = 6$: $\max(5, V[2, 2] + 5) = \max(5, 1 + 5) = 6$
  - $w = 7$: $\max(5, V[2, 3] + 5) = \max(5, 4 + 5) = 9$
- **Row 4 (D: $w=5, v=7$):**
  - $w < 5$: copy from Row 3 $\implies [0, 1, 1, 4, 5]$
  - $w = 5$: $\max(6, V[3, 0] + 7) = \max(6, 7) = 7$
  - $w = 6$: $\max(6, V[3, 1] + 7) = \max(6, 1 + 7) = 8$
  - $w = 7$: $\max(9, V[3, 2] + 7) = \max(9, 1 + 7) = 9$

| $i \ \backslash \ w$ | $0$ | $1$ | $2$ | $3$ | $4$ | $5$ | $6$ | $7$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0 (None)** | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **1 (A: $w=1, v=1$)** | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| **2 (B: $w=3, v=4$)** | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 |
| **3 (C: $w=4, v=5$)** | 0 | 1 | 1 | 4 | 5 | 6 | 6 | **9** |
| **4 (D: $w=5, v=7$)** | 0 | 1 | 1 | 4 | 5 | 7 | 8 | **9** |

---

#### Step 2: Backtracking Trace
1. At $V[4, 7] = 9$:
   - $V[4, 7] = V[3, 7] = 9 \implies$ **Item D is NOT selected** ($x_D = 0$).
   - Remaining capacity $= 7$.
2. At $V[3, 7] = 9$:
   - $V[3, 7] \ne V[2, 7] (5) \implies$ **Item C is SELECTED** ($x_C = 1$).
   - Remaining capacity $= 7 - w_C = 7 - 4 = 3$.
3. At $V[2, 3] = 4$:
   - $V[2, 3] \ne V[1, 3] (1) \implies$ **Item B is SELECTED** ($x_B = 1$).
   - Remaining capacity $= 3 - w_B = 3 - 3 = 0$.
4. At $w = 0$:
   - Item A is NOT selected ($x_A = 0$).

#### Step 3: Final Result
- **Selected Items:** $\{B, C\}$
- **Total Weight:** $3 + 4 = 7 \le 7$
- **Maximum Value:**
  $$\text{Total Value} = 4 + 5 = \mathbf{9}$$
