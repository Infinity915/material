# Design and Analysis of Algorithms (DAA)
## Complete Step-by-Step Solutions to All SPPU PYQ Numericals (2022 – 2025)
### Savitribai Phule Pune University (SPPU) | Course Code: 410241 (2019 Pattern)

---

## Executive Summary & Examination Blueprint
This master document provides an exhaustive, mathematically verified, step-by-step repository of **every numerical problem** that appeared in the Savitribai Phule Pune University (SPPU) Final Theory Examinations for **Design and Analysis of Algorithms (DAA)** from **2022 to 2025**.

### Master Table of Contents
1. [Topic 01: Job Sequencing with Deadlines (Greedy Strategy)](#topic-01-job-sequencing-with-deadlines-greedy-strategy)
2. [Topic 02: Activity Selection Problem (Greedy Strategy)](#topic-02-activity-selection-problem-greedy-strategy)
3. [Topic 03: 0/1 Knapsack Problem (Dynamic Programming)](#topic-03-01-knapsack-problem-dynamic-programming)
4. [Topic 04: Matrix Chain Multiplication (Dynamic Programming)](#topic-04-matrix-chain-multiplication-dynamic-programming)
5. [Topic 05: Optimal Binary Search Tree (Dynamic Programming)](#topic-05-optimal-binary-search-tree-dynamic-programming)
6. [Topic 06: Binomial Coefficients (Dynamic Programming)](#topic-06-binomial-coefficients-dynamic-programming)
7. [Topic 07: Sum of Subsets Problem (Backtracking)](#topic-07-sum-of-subsets-problem-backtracking)
8. [Topic 08: Graph Coloring Problem (Backtracking)](#topic-08-graph-coloring-problem-backtracking)
9. [Topic 09: 0/1 Knapsack Problem (Least-Cost Branch & Bound)](#topic-09-01-knapsack-problem-least-cost-branch--bound)
10. [Topic 10: Traveling Salesman Problem (Branch & Bound Matrix Reduction)](#topic-10-traveling-salesman-problem-branch--bound-matrix-reduction)
11. [Topic 11: Amortized Analysis Derivations (Stack & Binary Counter)](#topic-11-amortized-analysis-derivations-stack--binary-counter)
12. [Topic 12: String Matching Algorithms (Rabin-Karp & Naive)](#topic-12-string-matching-algorithms-rabin-karp--naive)
13. [Topic 13: Distributed Breadth-First Search (Distributed BFS)](#topic-13-distributed-breadth-first-search-distributed-bfs)

---



<div style="page-break-after: always;"></div>

---

# Topic 01: Job Sequencing with Deadlines (Greedy Strategy)

This document provides complete, step-by-step mathematical solutions for every **Job Sequencing with Deadlines** numerical problem that appeared in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition
Given a set of $n$ jobs where each job $i$ has:
- A positive deadline $d_i \ge 1$
- An associated profit $p_i > 0$

Each job takes exactly $1$ unit of processing time on a single CPU/machine. Only one job can be processed at any given time slot $[t-1, t]$. A job yields its profit $p_i$ if and only if it finishes on or before its deadline $d_i$ (i.e., scheduled in time slot $t \le d_i$). The objective is to find a feasible subset of jobs and a processing schedule that maximizes total profit.

### 1.2 Mathematical Formulation
Maximize:
$$\text{Total Profit} = \sum_{i \in S} p_i$$
Subject to:
1. $S \subseteq \{1, 2, \dots, n\}$
2. Every job $i \in S$ is assigned to a distinct integer unit time slot $t \in \{1, 2, \dots, d_{\max}\}$ such that $t \le d_i$, where $d_{\max} = \max_{1 \le i \le n}(d_i)$.

### 1.3 Greedy Strategy
1. **Sort:** Arrange all $n$ jobs in non-increasing (descending) order of their profits:
   $$p_1 \ge p_2 \ge p_3 \ge \dots \ge p_n$$
2. **Initialize:** Find the maximum deadline $d_{\max} = \max(d_i)$. Create a timeline with $d_{\max}$ unit time slots:
   $$\text{Slot } 1: [0, 1], \quad \text{Slot } 2: [1, 2], \quad \dots, \quad \text{Slot } d_{\max}: [d_{\max}-1, d_{\max}]$$
   Initialize all slots as empty (`NULL`). Set $\text{Total Profit} = 0$.
3. **Slot Assignment (Greedy Choice):** For each job $J_i$ in sorted order:
   - Check available time slots from $\min(d_{\max}, d_i)$ backwards down to $1$.
   - If an empty slot $k$ ($1 \le k \le d_i$) is found, assign job $J_i$ to slot $k$, mark slot $k$ as occupied, and add $p_i$ to total profit.
   - If all slots from $d_i$ down to $1$ are already occupied, reject job $J_i$.
4. **Complexity:**
   - Sorting: $O(n \log n)$
   - Slot searching: $O(n \cdot d_{\max})$ using a simple array, or $O(n \alpha(d_{\max}))$ using Disjoint Set Union (DSU / Find-Union).
   - Space Complexity: $O(d_{\max})$ for the scheduling array.

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU Nov/Dec 2023 [9 Marks]

#### Problem Statement
> Given $n = 5$ jobs with their profits $P = \{20, 15, 10, 5, 1\}$ and deadlines $D = \{2, 2, 1, 3, 3\}$. Find the optimal schedule and maximum profit using greedy job sequencing with deadlines.

#### Step 1: Input Tabulation & Verification
The jobs are already sorted in non-increasing order of profits:

| Job ($J_i$) | Profit ($p_i$) | Deadline ($d_i$) |
| :---: | :---: | :---: |
| $J_1$ | 20 | 2 |
| $J_2$ | 15 | 2 |
| $J_3$ | 10 | 1 |
| $J_4$ | 5 | 3 |
| $J_5$ | 1 | 3 |

- Number of jobs: $n = 5$
- Maximum deadline: $d_{\max} = \max(2, 2, 1, 3, 3) = 3$
- Number of available unit time slots: $3$
  - Slot 1: $[0, 1]$
  - Slot 2: $[1, 2]$
  - Slot 3: $[2, 3]$

#### Step 2: Step-by-Step Execution Trace

1. **Consider $J_1$ ($p_1 = 20, d_1 = 2$):**
   - Latest allowable slot is $\min(3, 2) = 2$ (interval $[1, 2]$).
   - Slot 2 is empty.
   - **Action:** Assign $J_1$ to Slot 2 $[1, 2]$.
   - Schedule status: `[Slot 1: Empty, Slot 2: J1, Slot 3: Empty]`
   - Current Profit $= 20$.

2. **Consider $J_2$ ($p_2 = 15, d_2 = 2$):**
   - Latest allowable slot is $\min(3, 2) = 2$ (interval $[1, 2]$).
   - Slot 2 is occupied by $J_1$.
   - Search backwards: Check Slot 1 (interval $[0, 1]$).
   - Slot 1 is empty.
   - **Action:** Assign $J_2$ to Slot 1 $[0, 1]$.
   - Schedule status: `[Slot 1: J2, Slot 2: J1, Slot 3: Empty]`
   - Current Profit $= 20 + 15 = 35$.

3. **Consider $J_3$ ($p_3 = 10, d_3 = 1$):**
   - Latest allowable slot is $\min(3, 1) = 1$ (interval $[0, 1]$).
   - Slot 1 is occupied by $J_2$.
   - Search backwards: No earlier slot exists ($k < 1$).
   - **Action:** Reject $J_3$.
   - Schedule status: Unchanged.
   - Current Profit $= 35$.

4. **Consider $J_4$ ($p_4 = 5, d_4 = 3$):**
   - Latest allowable slot is $\min(3, 3) = 3$ (interval $[2, 3]$).
   - Slot 3 is empty.
   - **Action:** Assign $J_4$ to Slot 3 $[2, 3]$.
   - Schedule status: `[Slot 1: J2, Slot 2: J1, Slot 3: J4]`
   - Current Profit $= 35 + 5 = 40$.

5. **Consider $J_5$ ($p_5 = 1, d_5 = 3$):**
   - Latest allowable slot is $\min(3, 3) = 3$.
   - Slot 3 is occupied ($J_4$), Slot 2 is occupied ($J_1$), Slot 1 is occupied ($J_2$).
   - No free slots available $\le 3$.
   - **Action:** Reject $J_5$.

#### Step 3: Gantt Chart & Final Schedule

```
Timeline:  0 --------- 1 --------- 2 --------- 3
Slots:        Slot 1       Slot 2       Slot 3
Assigned:     [ J2 ]       [ J1 ]       [ J4 ]
Profit:        +15          +20          +5
```

- **Selected Feasible Set of Jobs:** $\{J_1, J_2, J_4\}$
- **Optimal Schedule (Execution Order):** $J_2 \to J_1 \to J_4$
- **Total Maximum Profit:**
  $$\text{Profit} = p_2 + p_1 + p_4 = 15 + 20 + 5 = \mathbf{40}$$

---

### Instance B: SPPU May/June 2023 [12 Marks]

#### Problem Statement
> Solve the job sequencing with deadlines problem for $n = 5$ jobs with profits and deadlines:
> $J_1: (p_1 = 60, d_1 = 2)$, $J_2: (p_2 = 100, d_2 = 1)$, $J_3: (p_3 = 20, d_3 = 3)$, $J_4: (p_4 = 40, d_4 = 2)$, $J_5: (p_5 = 20, d_5 = 1)$.
> Determine the optimal job sequence and maximum profit.

#### Step 1: Sorting Jobs by Profit (Descending)
Sorting in non-increasing order of profits:

| Rank | Job ($J_i$) | Profit ($p_i$) | Deadline ($d_i$) |
| :---: | :---: | :---: | :---: |
| 1 | $J_2$ | 100 | 1 |
| 2 | $J_1$ | 60 | 2 |
| 3 | $J_4$ | 40 | 2 |
| 4 | $J_3$ | 20 | 3 |
| 5 | $J_5$ | 20 | 1 |

- $d_{\max} = \max(1, 2, 2, 3, 1) = 3$ slots: Slot 1 $[0, 1]$, Slot 2 $[1, 2]$, Slot 3 $[2, 3]$.

#### Step 2: Step-by-Step Execution Trace

1. **Consider $J_2$ ($p_2 = 100, d_2 = 1$):**
   - Target slot is $\min(3, 1) = 1$. Slot 1 is free.
   - **Action:** Assign $J_2$ to Slot 1 $[0, 1]$.
   - Slots: `[Slot 1: J2, Slot 2: Empty, Slot 3: Empty]`
   - Profit $= 100$.

2. **Consider $J_1$ ($p_1 = 60, d_1 = 2$):**
   - Target slot is $\min(3, 2) = 2$. Slot 2 is free.
   - **Action:** Assign $J_1$ to Slot 2 $[1, 2]$.
   - Slots: `[Slot 1: J2, Slot 2: J1, Slot 3: Empty]`
   - Profit $= 100 + 60 = 160$.

3. **Consider $J_4$ ($p_4 = 40, d_4 = 2$):**
   - Target slot is $\min(3, 2) = 2$. Slot 2 is occupied ($J_1$).
   - Backtrack to Slot 1: Occupied ($J_2$).
   - No free slot $\le 2$.
   - **Action:** Reject $J_4$.

4. **Consider $J_3$ ($p_3 = 20, d_3 = 3$):**
   - Target slot is $\min(3, 3) = 3$. Slot 3 is free.
   - **Action:** Assign $J_3$ to Slot 3 $[2, 3]$.
   - Slots: `[Slot 1: J2, Slot 2: J1, Slot 3: J3]`
   - Profit $= 160 + 20 = 180$.

5. **Consider $J_5$ ($p_5 = 20, d_5 = 1$):**
   - Target slot is Slot 1. Slot 1 is occupied ($J_2$).
   - No slot available $\le 1$.
   - **Action:** Reject $J_5$.

#### Step 3: Final Schedule & Output
- **Gantt Chart:**
  `[0 to 1]: J2 | [1 to 2]: J1 | [2 to 3]: J3`
- **Optimal Schedule:** $J_2 \to J_1 \to J_3$
- **Total Maximum Profit:**
  $$\text{Profit} = 100 + 60 + 20 = \mathbf{180}$$

---

### Instance C: SPPU May/June 2023 (Page 6) [8 Marks]

This paper presented two distinct sets of job scheduling problems.

#### Problem Set 1 (4 Jobs)
> Jobs: $a(p=20, d=4)$, $b(p=10, d=1)$, $c(p=40, d=1)$, $d(p=30, d=1)$.

1. **Sort by profit descending:**
   1. $c: p=40, d=1$
   2. $d: p=30, d=1$
   3. $a: p=20, d=4$
   4. $b: p=10, d=1$
2. **Parameters:** $d_{\max} = \max(4, 1, 1, 1) = 4$ slots: $[0,1], [1,2], [2,3], [3,4]$.
3. **Execution Trace:**
   - **Job $c$ ($40, 1$):** Slot 1 is empty. Assign $c$ to Slot 1 $[0, 1]$.
   - **Job $d$ ($30, 1$):** Slot 1 occupied. No slots $\le 1$. Reject $d$.
   - **Job $a$ ($20, 4$):** Slot 4 is empty. Assign $a$ to Slot 4 $[3, 4]$.
   - **Job $b$ ($10, 1$):** Slot 1 occupied. Reject $b$.
4. **Schedule:** Slot 1: $c$, Slot 2: Idle, Slot 3: Idle, Slot 4: $a$.
5. **Optimal Subset:** $\{c, a\}$
6. **Maximum Profit:**
   $$\text{Profit} = 40 + 20 = \mathbf{60}$$

---

#### Problem Set 2 (5 Jobs)
> Jobs: $a(p=100, d=2)$, $b(p=19, d=1)$, $c(p=27, d=2)$, $d(p=25, d=1)$, $e(p=15, d=3)$.

1. **Sort by profit descending:**
   1. $a: p=100, d=2$
   2. $c: p=27, d=2$
   3. $d: p=25, d=1$
   4. $b: p=19, d=1$
   5. $e: p=15, d=3$
2. **Parameters:** $d_{\max} = 3$ slots: $[0, 1], [1, 2], [2, 3]$.
3. **Execution Trace:**
   - **Job $a$ ($100, 2$):** Slot 2 is empty. Assign $a$ to Slot 2 $[1, 2]$.
   - **Job $c$ ($27, 2$):** Slot 2 is occupied ($a$). Backtrack to Slot 1. Slot 1 is empty. Assign $c$ to Slot 1 $[0, 1]$.
   - **Job $d$ ($25, 1$):** Slot 1 is occupied ($c$). Reject $d$.
   - **Job $b$ ($19, 1$):** Slot 1 is occupied ($c$). Reject $b$.
   - **Job $e$ ($15, 3$):** Slot 3 is empty. Assign $e$ to Slot 3 $[2, 3]$.
4. **Schedule:** Slot 1: $c$, Slot 2: $a$, Slot 3: $e$.
5. **Optimal Subset:** $\{c, a, e\}$
6. **Maximum Profit:**
   $$\text{Profit} = 27 + 100 + 15 = \mathbf{142}$$

---

### Instance D: SPPU 2024/2025 [8 Marks]

#### Problem Statement
> Given 4 jobs with profits and deadlines:
> - Job $A$: Profit $= 75$, Deadline $= 2$
> - Job $B$: Profit $= 40$, Deadline $= 1$
> - Job $C$: Profit $= 50$, Deadline $= 1$
> - Job $D$: Profit $= 35$, Deadline $= 2$
>
> Find the optimal schedule and total profit using Greedy Job Sequencing.

#### Step 1: Sorting Jobs
Sort descending by profit:

| Rank | Job | Profit ($p$) | Deadline ($d$) |
| :---: | :---: | :---: | :---: |
| 1 | $A$ | 75 | 2 |
| 2 | $C$ | 50 | 1 |
| 3 | $B$ | 40 | 1 |
| 4 | $D$ | 35 | 2 |

- $d_{\max} = \max(2, 1, 1, 2) = 2$ slots: Slot 1 $[0, 1]$, Slot 2 $[1, 2]$.

#### Step 2: Slot Allocation Trace
1. **Job $A$ ($p=75, d=2$):** Slot 2 is empty. Assign $A$ to Slot 2 $[1, 2]$.
2. **Job $C$ ($p=50, d=1$):** Slot 1 is empty. Assign $C$ to Slot 1 $[0, 1]$.
3. **Job $B$ ($p=40, d=1$):** Slot 1 is occupied by $C$. Reject $B$.
4. **Job $D$ ($p=35, d=2$):** Slot 2 is occupied by $A$, Slot 1 is occupied by $C$. Both slots $\le 2$ are filled. Reject $D$.

#### Step 3: Final Schedule
- **Schedule:**
  - Slot 1 $[0, 1]$: Job $C$
  - Slot 2 $[1, 2]$: Job $A$
- **Selected Jobs:** $\{C, A\}$
- **Maximum Profit:**
  $$\text{Profit} = 50 + 75 = \mathbf{125}$$

---

## 3. Summary of Job Sequencing Results

| Exam Paper | Given Jobs | Optimal Sequence | Selected Jobs | Max Profit |
| :--- | :--- | :--- | :--- | :---: |
| **Nov/Dec 2023** | $P=\{20,15,10,5,1\}, D=\{2,2,1,3,3\}$ | $J_2 \to J_1 \to J_4$ | $\{J_1, J_2, J_4\}$ | **40** |
| **May/June 2023** | $J_1(60,2), J_2(100,1), J_3(20,3), J_4(40,2), J_5(20,1)$ | $J_2 \to J_1 \to J_3$ | $\{J_1, J_2, J_3\}$ | **180** |
| **May/June 2023 (Set 1)** | $a(20,4), b(10,1), c(40,1), d(30,1)$ | $c \to \text{idle} \to \text{idle} \to a$ | $\{c, a\}$ | **60** |
| **May/June 2023 (Set 2)** | $a(100,2), b(19,1), c(27,2), d(25,1), e(15,3)$ | $c \to a \to e$ | $\{c, a, e\}$ | **142** |
| **2024/2025** | $A(75,2), B(40,1), C(50,1), D(35,2)$ | $C \to A$ | $\{C, A\}$ | **125** |



<div style="page-break-after: always;"></div>

---

# Topic 02: Activity Selection Problem (Greedy Strategy)

This document provides a complete, step-by-step mathematical solution for the **Activity Selection Problem** from the SPPU DAA Examination papers (specifically Nov/Dec 2024, 9 Marks).

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition
Given a set of $n$ activities $S = \{A_1, A_2, \dots, A_n\}$ that wish to use a shared resource (such as a single lecture hall, processor, or conference room). Each activity $A_i$ has:
- A start time $s_i$
- A finish time $f_i$ where $0 \le s_i < f_i < \infty$

If selected, activity $A_i$ occupies the resource during the half-open time interval $[s_i, f_i)$. Two activities $A_i$ and $A_j$ are **compatible** (mutually non-conflicting) if their time intervals do not overlap, i.e.:
$$s_i \ge f_j \quad \text{or} \quad s_j \ge f_i$$

The objective is to select a **maximum-size subset of mutually compatible activities**.

### 1.2 The Greedy Choice Property
The optimal greedy strategy is to always pick the activity that **finishes earliest**, leaving as much time as possible for the remaining activities:
1. **Sort:** Order all activities in non-decreasing order of their finish times:
   $$f_1 \le f_2 \le f_3 \le \dots \le f_n$$
2. **First Choice:** Always select the first activity $A_1$ (the one with the earliest finish time $f_1$).
3. **Iterative Selection:** For every subsequent activity $A_i$ ($i = 2, \dots, n$):
   - Compare its start time $s_i$ with the finish time of the last selected activity $f_{\text{last}}$.
   - If $s_i \ge f_{\text{last}}$, select $A_i$ and update $f_{\text{last}} = f_i$.
   - Otherwise, reject $A_i$ (it conflicts with the currently scheduled activity).

### 1.3 Complexity Analysis
- **Sorting Time:** $O(n \log n)$ to sort the activities by finish time.
- **Scanning Time:** $O(n)$ single pass over the sorted list.
- **Total Time Complexity:** $O(n \log n)$ (or $O(n)$ if activities are already sorted).
- **Space Complexity:** $O(1)$ auxiliary space (in-place) or $O(n)$ to store the selected subset.

---

## 2. SPPU Nov/Dec 2024 PYQ Numerical [9 Marks]

### Problem Statement
> Explain Greedy Strategy. Find the optimal solution for the following Activity Selection Problem using Greedy method:
>
> | Activity | Start Time ($s$) | Finish Time ($f$) |
> | :---: | :---: | :---: |
> | **A** | 1 | 3 |
> | **B** | 3 | 4 |
> | **C** | 2 | 5 |
> | **D** | 0 | 7 |
> | **E** | 5 | 9 |
> | **F** | 8 | 10 |
> | **G** | 11 | 12 |

---

### Step 1: Verification of Sorting by Finish Time
Activities are already arranged in non-decreasing order of their finish times $f$:
$$f_A(3) \le f_B(4) \le f_C(5) \le f_D(7) \le f_E(9) \le f_F(10) \le f_G(12)$$

Let the list of candidates be:
$$L = \langle A(1, 3), B(3, 4), C(2, 5), D(0, 7), E(5, 9), F(8, 10), G(11, 12) \rangle$$

---

### Step 2: Step-by-Step Selection Trace

Initialize:
- Selected Activities Subset: $\mathcal{S} = \emptyset$
- Last Activity Finish Time: $f_{\text{last}} = -\infty$

| Step | Activity Evaluated | Start Time ($s_i$) | Finish Time ($f_i$) | Compatibility Check ($s_i \ge f_{\text{last}}$) | Decision | Updated $f_{\text{last}}$ | Current Selected Set $\mathcal{S}$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | **A** | 1 | 3 | $1 \ge -\infty$ (**True**) | **SELECT A** | 3 | $\{A\}$ |
| **2** | **B** | 3 | 4 | $3 \ge 3$ (**True**) | **SELECT B** | 4 | $\{A, B\}$ |
| **3** | **C** | 2 | 5 | $2 \ge 4$ (**False**, conflicts with B) | **REJECT C** | 4 | $\{A, B\}$ |
| **4** | **D** | 0 | 7 | $0 \ge 4$ (**False**, conflicts with B) | **REJECT D** | 4 | $\{A, B\}$ |
| **5** | **E** | 5 | 9 | $5 \ge 4$ (**True**) | **SELECT E** | 9 | $\{A, B, E\}$ |
| **6** | **F** | 8 | 10 | $8 \ge 9$ (**False**, conflicts with E) | **REJECT F** | 9 | $\{A, B, E\}$ |
| **7** | **G** | 11 | 12 | $11 \ge 9$ (**True**) | **SELECT G** | 12 | $\{A, B, E, G\}$ |

---

### Step 3: Graphical Timeline Representation

```
Time:     0   1   2   3   4   5   6   7   8   9  10  11  12
          |---|---|---|---|---|---|---|---|---|---|---|---|
A [1-3]       [=======]                                     --> SELECTED
B [3-4]               [===]                                 --> SELECTED
C [2-5]           x-------x                                 --> REJECTED (overlaps B)
D [0-7]   x-------------------x                             --> REJECTED (overlaps A, B)
E [5-9]                       [===============]             --> SELECTED
F [8-10]                                  x-------x         --> REJECTED (overlaps E)
G [11-12]                                         [===]     --> SELECTED
```

---

### Step 4: Final Conclusion & Results

1. **Optimal Compatible Subset of Activities:**
   $$\mathcal{S}^* = \{A, B, E, G\}$$
2. **Maximum Number of Activities Scheduled:**
   $$|\mathcal{S}^*| = \mathbf{4}$$
3. **Execution Time Windows:**
   - Activity $A$: from time $1$ to $3$
   - Activity $B$: from time $3$ to $4$
   - Activity $E$: from time $5$ to $9$
   - Activity $G$: from time $11$ to $12$
   All chosen intervals are strictly non-overlapping.



<div style="page-break-after: always;"></div>

---

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



<div style="page-break-after: always;"></div>

---

# Topic 04: Matrix Chain Multiplication (Dynamic Programming)

This document provides a comprehensive, step-by-step mathematical solution for the **Matrix Chain Multiplication (MCM)** problem from the SPPU DAA Endsem Examination (specifically Dec 2022/Jan 2023, 10 Marks).

---

## 1. Algorithmic Foundation & Recurrence Relation

### 1.1 Problem Definition
Given a sequence (chain) of $n$ matrices $\langle A_1, A_2, \dots, A_n \rangle$, where matrix $A_i$ has dimension $p_{i-1} \times p_i$. The objective is to find the most efficient order (parenthesization) of multiplying these matrices together such that the total number of scalar multiplications is minimized.

Because matrix multiplication is associative:
$$(A_1 A_2) A_3 = A_1 (A_2 A_3)$$
The parenthesization does not change the resulting product matrix, but it drastically impacts the scalar computational cost.

Multiplying matrix $A$ of dimension $p \times q$ by matrix $B$ of dimension $q \times r$ takes:
$$\text{Cost} = p \cdot q \cdot r \quad \text{scalar multiplications}$$

### 1.2 Dynamic Programming Recurrence
Let $m[i, j]$ denote the minimum number of scalar multiplications needed to compute the matrix product $A_i \dots A_j$ for $1 \le i \le j \le n$.

The recurrence relation is:
$$m[i, j] = \begin{cases} 
0 & \text{if } i = j \\
\min\limits_{i \le k < j} \Big\{ m[i, k] + m[k+1, j] + p_{i-1} p_k p_j \Big\} & \text{if } i < j
\end{cases}$$

Where $k$ is the index of the optimal split point between $A_k$ and $A_{k+1}$.
The table $s[i, j]$ stores the optimal split index $k$ that achieves this minimum:
$$s[i, j] = \arg\min_{i \le k < j} \Big\{ m[i, k] + m[k+1, j] + p_{i-1} p_k p_j \Big\}$$

### 1.3 Complexity Analysis
- **Time Complexity:** $\Theta(n^3)$ — computing $O(n^2)$ table entries, each taking $O(n)$ search over split points $k$.
- **Space Complexity:** $\Theta(n^2)$ to store the $m$ and $s$ tables.

---

## 2. SPPU Dec 2022/Jan 2023 PYQ Numerical [10 Marks]

### Problem Statement
> Find the minimum number of operations required for the following matrix chain multiplication using Dynamic Programming:
> - $A_1: 10 \times 20$
> - $A_2: 20 \times 5$
> - $A_3: 5 \times 15$
> - $A_4: 15 \times 50$
> - $A_5: 50 \times 10$
> - $A_6: 10 \times 15$
>
> Construct the complete cost table $m$ and split table $s$. Trace and display the optimal parenthesization.

---

### Step 1: Dimension Vector Formulation
From the given matrix dimensions:
$$p_0 = 10, \quad p_1 = 20, \quad p_2 = 5, \quad p_3 = 15, \quad p_4 = 50, \quad p_5 = 10, \quad p_6 = 15$$

The dimension vector is:
$$P = \langle 10, 20, 5, 15, 50, 10, 15 \rangle, \quad n = 6$$

---

### Step 2: Step-by-Step Chain Length Computations

#### Chain Length $L = 1$ (Base Cases, $i = j$):
$$m[1, 1] = m[2, 2] = m[3, 3] = m[4, 4] = m[5, 5] = m[6, 6] = 0$$

---

#### Chain Length $L = 2$ (Chains of 2 matrices, $j = i + 1$):

1. **$m[1, 2]$ ($k = 1$):**
   $$m[1, 2] = m[1, 1] + m[2, 2] + p_0 p_1 p_2 = 0 + 0 + (10 \times 20 \times 5) = \mathbf{1000}, \quad s[1, 2] = 1$$

2. **$m[2, 3]$ ($k = 2$):**
   $$m[2, 3] = m[2, 2] + m[3, 3] + p_1 p_2 p_3 = 0 + 0 + (20 \times 5 \times 15) = \mathbf{1500}, \quad s[2, 3] = 2$$

3. **$m[3, 4]$ ($k = 3$):**
   $$m[3, 4] = m[3, 3] + m[4, 4] + p_2 p_3 p_4 = 0 + 0 + (5 \times 15 \times 50) = \mathbf{3750}, \quad s[3, 4] = 3$$

4. **$m[4, 5]$ ($k = 4$):**
   $$m[4, 5] = m[4, 4] + m[5, 5] + p_3 p_4 p_5 = 0 + 0 + (15 \times 50 \times 10) = \mathbf{7500}, \quad s[4, 5] = 4$$

5. **$m[5, 6]$ ($k = 5$):**
   $$m[5, 6] = m[5, 5] + m[6, 6] + p_4 p_5 p_6 = 0 + 0 + (50 \times 10 \times 15) = \mathbf{7500}, \quad s[5, 6] = 5$$

---

#### Chain Length $L = 3$ (Chains of 3 matrices, $j = i + 2$):

1. **$m[1, 3]$ ($k \in \{1, 2\}$):**
   - $k = 1$: $m[1, 1] + m[2, 3] + p_0 p_1 p_3 = 0 + 1500 + (10 \times 20 \times 15) = 1500 + 3000 = 4500$
   - $k = 2$: $m[1, 2] + m[3, 3] + p_0 p_2 p_3 = 1000 + 0 + (10 \times 5 \times 15) = 1000 + 750 = 1750$
   $$\mathbf{m[1, 3] = 1750}, \quad s[1, 3] = 2$$

2. **$m[2, 4]$ ($k \in \{2, 3\}$):**
   - $k = 2$: $m[2, 2] + m[3, 4] + p_1 p_2 p_4 = 0 + 3750 + (20 \times 5 \times 50) = 3750 + 5000 = 8750$
   - $k = 3$: $m[2, 3] + m[4, 4] + p_1 p_3 p_4 = 1500 + 0 + (20 \times 15 \times 50) = 1500 + 15000 = 16500$
   $$\mathbf{m[2, 4] = 8750}, \quad s[2, 4] = 2$$

3. **$m[3, 5]$ ($k \in \{3, 4\}$):**
   - $k = 3$: $m[3, 3] + m[4, 5] + p_2 p_3 p_5 = 0 + 7500 + (5 \times 15 \times 10) = 7500 + 750 = 8250$
   - $k = 4$: $m[3, 4] + m[5, 5] + p_2 p_4 p_5 = 3750 + 0 + (5 \times 50 \times 10) = 3750 + 2500 = 6250$
   $$\mathbf{m[3, 5] = 6250}, \quad s[3, 5] = 4$$

4. **$m[4, 6]$ ($k \in \{4, 5\}$):**
   - $k = 4$: $m[4, 4] + m[5, 6] + p_3 p_4 p_6 = 0 + 7500 + (15 \times 50 \times 15) = 7500 + 11250 = 18750$
   - $k = 5$: $m[4, 5] + m[6, 6] + p_3 p_5 p_6 = 7500 + 0 + (15 \times 10 \times 15) = 7500 + 2250 = 9750$
   $$\mathbf{m[4, 6] = 9750}, \quad s[4, 6] = 5$$

---

#### Chain Length $L = 4$ (Chains of 4 matrices, $j = i + 3$):

1. **$m[1, 4]$ ($k \in \{1, 2, 3\}$):**
   - $k = 1$: $m[1, 1] + m[2, 4] + p_0 p_1 p_4 = 0 + 8750 + (10 \times 20 \times 50) = 8750 + 10000 = 18750$
   - $k = 2$: $m[1, 2] + m[3, 4] + p_0 p_2 p_4 = 1000 + 3750 + (10 \times 5 \times 50) = 4750 + 2500 = 7250$
   - $k = 3$: $m[1, 3] + m[4, 4] + p_0 p_3 p_4 = 1750 + 0 + (10 \times 15 \times 50) = 1750 + 7500 = 9250$
   $$\mathbf{m[1, 4] = 7250}, \quad s[1, 4] = 2$$

2. **$m[2, 5]$ ($k \in \{2, 3, 4\}$):**
   - $k = 2$: $m[2, 2] + m[3, 5] + p_1 p_2 p_5 = 0 + 6250 + (20 \times 5 \times 10) = 6250 + 1000 = 7250$
   - $k = 3$: $m[2, 3] + m[4, 5] + p_1 p_3 p_5 = 1500 + 7500 + (20 \times 15 \times 10) = 9000 + 3000 = 12000$
   - $k = 4$: $m[2, 4] + m[5, 5] + p_1 p_4 p_5 = 8750 + 0 + (20 \times 50 \times 10) = 8750 + 10000 = 18750$
   $$\mathbf{m[2, 5] = 7250}, \quad s[2, 5] = 2$$

3. **$m[3, 6]$ ($k \in \{3, 4, 5\}$):**
   - $k = 3$: $m[3, 3] + m[4, 6] + p_2 p_3 p_6 = 0 + 9750 + (5 \times 15 \times 15) = 9750 + 1125 = 10875$
   - $k = 4$: $m[3, 4] + m[5, 6] + p_2 p_4 p_6 = 3750 + 7500 + (5 \times 50 \times 15) = 11250 + 3750 = 15000$
   - $k = 5$: $m[3, 5] + m[6, 6] + p_2 p_5 p_6 = 6250 + 0 + (5 \times 10 \times 15) = 6250 + 750 = 7000$
   $$\mathbf{m[3, 6] = 7000}, \quad s[3, 6] = 5$$

---

#### Chain Length $L = 5$ (Chains of 5 matrices, $j = i + 4$):

1. **$m[1, 5]$ ($k \in \{1, 2, 3, 4\}$):**
   - $k = 1$: $m[1, 1] + m[2, 5] + p_0 p_1 p_5 = 0 + 7250 + (10 \times 20 \times 10) = 7250 + 2000 = 9250$
   - $k = 2$: $m[1, 2] + m[3, 5] + p_0 p_2 p_5 = 1000 + 6250 + (10 \times 5 \times 10) = 7250 + 500 = 7750$
   - $k = 3$: $m[1, 3] + m[4, 5] + p_0 p_3 p_5 = 1750 + 7500 + (10 \times 15 \times 10) = 9250 + 1500 = 10750$
   - $k = 4$: $m[1, 4] + m[5, 5] + p_0 p_4 p_5 = 7250 + 0 + (10 \times 50 \times 10) = 7250 + 5000 = 12250$
   $$\mathbf{m[1, 5] = 7750}, \quad s[1, 5] = 2$$

2. **$m[2, 6]$ ($k \in \{2, 3, 4, 5\}$):**
   - $k = 2$: $m[2, 2] + m[3, 6] + p_1 p_2 p_6 = 0 + 7000 + (20 \times 5 \times 15) = 7000 + 1500 = 8500$
   - $k = 3$: $m[2, 3] + m[4, 6] + p_1 p_3 p_6 = 1500 + 9750 + (20 \times 15 \times 15) = 11250 + 4500 = 15750$
   - $k = 4$: $m[2, 4] + m[5, 6] + p_1 p_4 p_6 = 8750 + 7500 + (20 \times 50 \times 15) = 16250 + 15000 = 31250$
   - $k = 5$: $m[2, 5] + m[6, 6] + p_1 p_5 p_6 = 7250 + 0 + (20 \times 10 \times 15) = 7250 + 3000 = 10250$
   $$\mathbf{m[2, 6] = 8500}, \quad s[2, 6] = 2$$

---

#### Chain Length $L = 6$ (Full chain $A_1 \dots A_6$, $i = 1, j = 6$):

Evaluate all split points $k \in \{1, 2, 3, 4, 5\}$:
- $k = 1$: $m[1, 1] + m[2, 6] + p_0 p_1 p_6 = 0 + 8500 + (10 \times 20 \times 15) = 8500 + 3000 = 11500$
- $k = 2$: $m[1, 2] + m[3, 6] + p_0 p_2 p_6 = 1000 + 7000 + (10 \times 5 \times 15) = 8000 + 750 = \mathbf{8750}$  $\leftarrow \text{MINIMUM}$
- $k = 3$: $m[1, 3] + m[4, 6] + p_0 p_3 p_6 = 1750 + 9750 + (10 \times 15 \times 15) = 11500 + 2250 = 13750$
- $k = 4$: $m[1, 4] + m[5, 6] + p_0 p_4 p_6 = 7250 + 7500 + (10 \times 50 \times 15) = 14750 + 7500 = 22250$
- $k = 5$: $m[1, 5] + m[6, 6] + p_0 p_5 p_6 = 7750 + 0 + (10 \times 10 \times 15) = 7750 + 1500 = 9250$

$$\mathbf{m[1, 6] = 8750}, \quad s[1, 6] = 2$$

---

### Step 3: Complete DP Matrices ($m$ and $s$)

#### Cost Table $m[1..6, 1..6]$:
| $i \ \backslash \ j$ | 1 | 2 | 3 | 4 | 5 | 6 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | 0 | 1000 | 1750 | 7250 | 7750 | **8750** |
| **2** | - | 0 | 1500 | 8750 | 7250 | 8500 |
| **3** | - | - | 0 | 3750 | 6250 | 7000 |
| **4** | - | - | - | 0 | 7500 | 9750 |
| **5** | - | - | - | - | 0 | 7500 |
| **6** | - | - | - | - | - | 0 |

#### Split Table $s[1..5, 2..6]$:
| $i \ \backslash \ j$ | 2 | 3 | 4 | 5 | 6 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | 1 | 2 | 2 | 2 | **2** |
| **2** | - | 2 | 2 | 2 | 2 |
| **3** | - | - | 3 | 4 | 5 |
| **4** | - | - | - | 4 | 5 |
| **5** | - | - | - | - | 5 |

---

### Step 4: Tracing Optimal Parenthesization
Using recursive reconstruction with table $s$:
1. $s[1, 6] = 2 \implies (A_1 \dots A_2) \times (A_3 \dots A_6)$
2. Left side $(1, 2)$: $s[1, 2] = 1 \implies (A_1 A_2)$
3. Right side $(3, 6)$: $s[3, 6] = 5 \implies (A_3 \dots A_5) \times (A_6)$
4. Sub-chain $(3, 5)$: $s[3, 5] = 4 \implies (A_3 \dots A_4) \times (A_5)$
5. Sub-chain $(3, 4)$: $s[3, 4] = 3 \implies (A_3 A_4)$

Combining all brackets gives the optimal parenthesization:
$$\Big( (A_1 A_2) \Big( ((A_3 A_4) A_5) A_6 \Big) \Big)$$

---

### Step 5: Verification of Multiplications Cost
1. $M_{12} = (A_1 A_2)$: Cost $= 10 \times 20 \times 5 = 1000$ (Size: $10 \times 5$)
2. $M_{34} = (A_3 A_4)$: Cost $= 5 \times 15 \times 50 = 3750$ (Size: $5 \times 50$)
3. $M_{345} = (M_{34} A_5)$: Cost $= 3750 + (5 \times 50 \times 10) = 3750 + 2500 = 6250$ (Size: $5 \times 10$)
4. $M_{3456} = (M_{345} A_6)$: Cost $= 6250 + (5 \times 10 \times 15) = 6250 + 750 = 7000$ (Size: $5 \times 15$)
5. Total Cost $= M_{12} \times M_{3456}$: Cost $= 1000 + 7000 + (10 \times 5 \times 15) = 8000 + 750 = \mathbf{8750}$

- **Minimum Scalar Multiplications:** $\mathbf{8750}$
- **Optimal Parenthesization:** $\mathbf{((A_1 A_2) (((A_3 A_4) A_5) A_6))}$



<div style="page-break-after: always;"></div>

---

# Topic 05: Optimal Binary Search Tree (Dynamic Programming)

This document provides complete, step-by-step mathematical solutions for every **Optimal Binary Search Tree (OBST)** numerical problem appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Recurrence Relation

### 1.1 Problem Definition
Given a sequence of $n$ distinct sorted keys $K = \langle k_1, k_2, \dots, k_n \rangle$ where $k_1 < k_2 < \dots < k_n$, and a sequence of probabilities (or search frequencies) $P = \langle p_1, p_2, \dots, p_n \rangle$ where $p_i$ is the probability of searching for key $k_i$.

The objective is to construct a Binary Search Tree (BST) containing all $n$ keys such that the **expected search cost** is minimized:
$$\text{Expected Cost} = \sum_{i=1}^n p_i \cdot \text{depth}_T(k_i)$$
where $\text{depth}_T(k_i)$ is the number of nodes visited (root at depth $1$).

### 1.2 Dynamic Programming Recurrence
Let:
- $w[i, j]$ denote the sum of probabilities of keys from $k_i$ to $k_j$:
  $$w[i, j] = \sum_{k=i}^j p_k = w[i, j-1] + p_j$$
- $e[i, j]$ denote the expected search cost of an optimal binary search tree containing keys $k_i \dots k_j$.
- $root[i, j]$ denote the root key of the optimal subtree spanning $k_i \dots k_j$.

**Recurrence Relation:**
$$e[i, j] = \begin{cases} 
0 & \text{if } j = i - 1 \quad (\text{empty tree}) \\
\min\limits_{i \le r \le j} \Big\{ e[i, r-1] + e[r+1, j] + w[i, j] \Big\} & \text{if } i \le j
\end{cases}$$

When key $k_r$ is chosen as the root of subtree $k_i \dots k_j$:
- Keys $k_i \dots k_{r-1}$ form the left subtree with cost $e[i, r-1]$.
- Keys $k_{r+1} \dots k_j$ form the right subtree with cost $e[r+1, j]$.
- Every key in the subtree increases its depth by $1$, adding $w[i, j]$ to the total cost.

### 1.3 Complexity Analysis
- **Time Complexity:** $O(n^3)$ standard DP (reducible to $O(n^2)$ using Knuth's optimization since $root[i, j-1] \le root[i, j] \le root[i+1, j]$).
- **Space Complexity:** $O(n^2)$ for the $w$, $e$, and $root$ tables.

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU Dec 2022/Jan 2023 [10 Marks]

#### Problem Statement
> Consider four keys $A, B, C, D$ with probabilities:
> $$p_1 = 0.1, \quad p_2 = 0.2, \quad p_3 = 0.4, \quad p_4 = 0.3$$
> Find the optimal binary search tree using Dynamic Programming. Construct the weight, cost, and root tables, and draw the final tree.

---

#### Step 1: Base Case Initialization (Length $L = 0$)
For all $i \in \{1, 2, 3, 4, 5\}$:
$$e[i, i-1] = 0, \quad w[i, i-1] = 0$$

---

#### Step 2: Subtree Computations by Chain Length

##### Chain Length $L = 1$ (Single Keys, $j = i$):
For a single key, $w[i, i] = p_i$, $e[i, i] = p_i$, and $root[i, i] = i$:
- $i = 1$: $w[1, 1] = 0.1, \quad e[1, 1] = 0.1, \quad root[1, 1] = 1 \; (A)$
- $i = 2$: $w[2, 2] = 0.2, \quad e[2, 2] = 0.2, \quad root[2, 2] = 2 \; (B)$
- $i = 3$: $w[3, 3] = 0.4, \quad e[3, 3] = 0.4, \quad root[3, 3] = 3 \; (C)$
- $i = 4$: $w[4, 4] = 0.3, \quad e[4, 4] = 0.3, \quad root[4, 4] = 4 \; (D)$

---

##### Chain Length $L = 2$ ($j = i + 1$):

1. **Subtree $(1, 2)$ (Keys $A, B$):**
   - $w[1, 2] = p_1 + p_2 = 0.1 + 0.2 = 0.3$
   - $r = 1$: $e[1, 0] + e[2, 2] + w[1, 2] = 0 + 0.2 + 0.3 = 0.5$
   - $r = 2$: $e[1, 1] + e[3, 2] + w[1, 2] = 0.1 + 0 + 0.3 = \mathbf{0.4}$ $\leftarrow \min$
   $$\mathbf{e[1, 2] = 0.4}, \quad root[1, 2] = 2 \; (B)$$

2. **Subtree $(2, 3)$ (Keys $B, C$):**
   - $w[2, 3] = p_2 + p_3 = 0.2 + 0.4 = 0.6$
   - $r = 2$: $e[2, 1] + e[3, 3] + w[2, 3] = 0 + 0.4 + 0.6 = 1.0$
   - $r = 3$: $e[2, 2] + e[4, 3] + w[2, 3] = 0.2 + 0 + 0.6 = \mathbf{0.8}$ $\leftarrow \min$
   $$\mathbf{e[2, 3] = 0.8}, \quad root[2, 3] = 3 \; (C)$$

3. **Subtree $(3, 4)$ (Keys $C, D$):**
   - $w[3, 4] = p_3 + p_4 = 0.4 + 0.3 = 0.7$
   - $r = 3$: $e[3, 2] + e[4, 4] + w[3, 4] = 0 + 0.3 + 0.7 = \mathbf{1.0}$ $\leftarrow \min$
   - $r = 4$: $e[3, 3] + e[5, 4] + w[3, 4] = 0.4 + 0 + 0.7 = 1.1$
   $$\mathbf{e[3, 4] = 1.0}, \quad root[3, 4] = 3 \; (C)$$

---

##### Chain Length $L = 3$ ($j = i + 2$):

1. **Subtree $(1, 3)$ (Keys $A, B, C$):**
   - $w[1, 3] = w[1, 2] + p_3 = 0.3 + 0.4 = 0.7$
   - $r = 1$: $e[1, 0] + e[2, 3] + w[1, 3] = 0 + 0.8 + 0.7 = 1.5$
   - $r = 2$: $e[1, 1] + e[3, 3] + w[1, 3] = 0.1 + 0.4 + 0.7 = 1.2$
   - $r = 3$: $e[1, 2] + e[4, 3] + w[1, 3] = 0.4 + 0 + 0.7 = \mathbf{1.1}$ $\leftarrow \min$
   $$\mathbf{e[1, 3] = 1.1}, \quad root[1, 3] = 3 \; (C)$$

2. **Subtree $(2, 4)$ (Keys $B, C, D$):**
   - $w[2, 4] = w[2, 3] + p_4 = 0.6 + 0.3 = 0.9$
   - $r = 2$: $e[2, 1] + e[3, 4] + w[2, 4] = 0 + 1.0 + 0.9 = 1.9$
   - $r = 3$: $e[2, 2] + e[4, 4] + w[2, 4] = 0.2 + 0.3 + 0.9 = \mathbf{1.4}$ $\leftarrow \min$
   - $r = 4$: $e[2, 3] + e[5, 4] + w[2, 4] = 0.8 + 0 + 0.9 = 1.7$
   $$\mathbf{e[2, 4] = 1.4}, \quad root[2, 4] = 3 \; (C)$$

---

##### Chain Length $L = 4$ (Full Tree $1..4$, Keys $A, B, C, D$):
- $w[1, 4] = w[1, 3] + p_4 = 0.7 + 0.3 = 1.0$
- Evaluate all possible roots $r \in \{1, 2, 3, 4\}$:
  - $r = 1 \; (A)$: $e[1, 0] + e[2, 4] + w[1, 4] = 0 + 1.4 + 1.0 = 2.4$
  - $r = 2 \; (B)$: $e[1, 1] + e[3, 4] + w[1, 4] = 0.1 + 1.0 + 1.0 = 2.1$
  - $r = 3 \; (C)$: $e[1, 2] + e[4, 4] + w[1, 4] = 0.4 + 0.3 + 1.0 = \mathbf{1.7}$ $\leftarrow \text{MINIMUM}$
  - $r = 4 \; (D)$: $e[1, 3] + e[5, 4] + w[1, 4] = 1.1 + 0 + 1.0 = 2.1$

$$\mathbf{e[1, 4] = 1.7}, \quad root[1, 4] = 3 \; (C)$$

---

#### Step 3: Complete DP Matrices

##### Cost Table $e[i, j]$:
| $i \ \backslash \ j$ | 0 | 1 | 2 | 3 | 4 |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | 0 | 0.1 | 0.4 | 1.1 | **1.7** |
| **2** | - | 0 | 0.2 | 0.8 | 1.4 |
| **3** | - | - | 0 | 0.4 | 1.0 |
| **4** | - | - | - | 0 | 0.3 |
| **5** | - | - | - | - | 0 |

##### Root Table $root[i, j]$:
| $i \ \backslash \ j$ | 1 | 2 | 3 | 4 |
| :---: | :---: | :---: | :---: | :---: |
| **1** | 1 (A) | 2 (B) | 3 (C) | **3 (C)** |
| **2** | - | 2 (B) | 3 (C) | 3 (C) |
| **3** | - | - | 3 (C) | 3 (C) |
| **4** | - | - | - | 4 (D) |

---

#### Step 4: Tree Reconstruction & Diagram
1. Root of $(1, 4)$ is $k_3 = \mathbf{C}$.
2. Left subtree of $C$ is $(1, 2)$:
   - Root of $(1, 2)$ is $k_2 = \mathbf{B}$.
   - Left of $B$ is $(1, 1) \implies \mathbf{A}$.
   - Right of $B$ is $(3, 2) \implies \text{Empty}$.
3. Right subtree of $C$ is $(4, 4) \implies \mathbf{D}$.

```
             [ C ]   (Depth 1, Cost = 0.4 * 1 = 0.4)
            /     \
         [ B ]   [ D ] (Depth 2, Cost = 0.2 * 2 + 0.3 * 2 = 1.0)
         /
       [ A ]           (Depth 3, Cost = 0.1 * 3 = 0.3)
```

**Verification:**
$$\text{Total Cost} = (1 \times 0.4) + (2 \times 0.2) + (3 \times 0.1) + (2 \times 0.3) = 0.4 + 0.4 + 0.3 + 0.6 = \mathbf{1.7}$$

- **Minimum Expected Search Cost:** $\mathbf{1.7}$
- **Optimal Tree Root:** Key $C$

---

### Instance B: SPPU May/June 2023 [10 Marks]

#### Problem Statement
> Construct an Optimal Binary Search Tree using Dynamic Programming for the following keys and search frequencies:
> - Keys: $10, 20, 30, 40$
> - Frequencies: $f_1 = 4, f_2 = 2, f_3 = 6, f_4 = 3$

---

#### Step 1: Base Case Initialization
- $w[i, i-1] = 0, \quad e[i, i-1] = 0 \quad (\forall i \in [1, 5])$

---

#### Step 2: Step-by-Step Chain Length Computations

##### Length $L = 1$ ($j = i$):
- $i=1$: $w[1, 1] = 4, \quad e[1, 1] = 4, \quad root[1, 1] = 1 \; (10)$
- $i=2$: $w[2, 2] = 2, \quad e[2, 2] = 2, \quad root[2, 2] = 2 \; (20)$
- $i=3$: $w[3, 3] = 6, \quad e[3, 3] = 6, \quad root[3, 3] = 3 \; (30)$
- $i=4$: $w[4, 4] = 3, \quad e[4, 4] = 3, \quad root[4, 4] = 4 \; (40)$

---

##### Length $L = 2$ ($j = i + 1$):
1. **Subtree $(1, 2)$ (Keys 10, 20):**
   - $w[1, 2] = 4 + 2 = 6$
   - $r = 1$: $0 + 2 + 6 = \mathbf{8}$ $\leftarrow \min$
   - $r = 2$: $4 + 0 + 6 = 10$
   $$\mathbf{e[1, 2] = 8}, \quad root[1, 2] = 1 \; (10)$$

2. **Subtree $(2, 3)$ (Keys 20, 30):**
   - $w[2, 3] = 2 + 6 = 8$
   - $r = 2$: $0 + 6 + 8 = 14$
   - $r = 3$: $2 + 0 + 8 = \mathbf{10}$ $\leftarrow \min$
   $$\mathbf{e[2, 3] = 10}, \quad root[2, 3] = 3 \; (30)$$

3. **Subtree $(3, 4)$ (Keys 30, 40):**
   - $w[3, 4] = 6 + 3 = 9$
   - $r = 3$: $0 + 3 + 9 = \mathbf{12}$ $\leftarrow \min$
   - $r = 4$: $6 + 0 + 9 = 15$
   $$\mathbf{e[3, 4] = 12}, \quad root[3, 4] = 3 \; (30)$$

---

##### Length $L = 3$ ($j = i + 2$):
1. **Subtree $(1, 3)$ (Keys 10, 20, 30):**
   - $w[1, 3] = 4 + 2 + 6 = 12$
   - $r = 1$: $0 + 10 + 12 = 22$
   - $r = 2$: $4 + 6 + 12 = 22$
   - $r = 3$: $8 + 0 + 12 = \mathbf{20}$ $\leftarrow \min$
   $$\mathbf{e[1, 3] = 20}, \quad root[1, 3] = 3 \; (30)$$

2. **Subtree $(2, 4)$ (Keys 20, 30, 40):**
   - $w[2, 4] = 2 + 6 + 3 = 11$
   - $r = 2$: $0 + 12 + 11 = 23$
   - $r = 3$: $2 + 3 + 11 = \mathbf{16}$ $\leftarrow \min$
   - $r = 4$: $10 + 0 + 11 = 21$
   $$\mathbf{e[2, 4] = 16}, \quad root[2, 4] = 3 \; (30)$$

---

##### Length $L = 4$ (Full Tree $1..4$, Keys 10, 20, 30, 40):
- $w[1, 4] = 4 + 2 + 6 + 3 = 15$
- Check roots $r \in \{1, 2, 3, 4\}$:
  - $r = 1$: $e[1, 0] + e[2, 4] + w = 0 + 16 + 15 = 31$
  - $r = 2$: $e[1, 1] + e[3, 4] + w = 4 + 12 + 15 = 31$
  - $r = 3$: $e[1, 2] + e[4, 4] + w = 8 + 3 + 15 = \mathbf{26}$ $\leftarrow \text{MINIMUM}$
  - $r = 4$: $e[1, 3] + e[5, 4] + w = 20 + 0 + 15 = 35$

$$\mathbf{e[1, 4] = 26}, \quad root[1, 4] = 3 \; (30)$$

---

#### Step 3: Resulting OBST Topology
1. Root of $(1, 4)$ is key $\mathbf{30}$.
2. Left subtree of 30 spans keys $\{10, 20\}$ with root $\mathbf{10}$:
   - Key $10$ has right child $\mathbf{20}$.
3. Right subtree of 30 spans key $\mathbf{40}$.

```
             [ 30 ]    (Depth 1, Weight = 6 * 1 = 6)
            /      \
        [ 10 ]    [ 40 ] (Depth 2, Weight = 4 * 2 + 3 * 2 = 14)
            \
            [ 20 ]       (Depth 3, Weight = 2 * 3 = 6)
```

**Verification:**
$$\text{Total Cost} = (1 \times 6) + (2 \times 4) + (2 \times 3) + (3 \times 2) = 6 + 8 + 6 + 6 = \mathbf{26}$$

- **Minimum Total Cost:** $\mathbf{26}$
- **Optimal Tree Root:** Key $30$



<div style="page-break-after: always;"></div>

---

# Topic 06: Binomial Coefficients (Dynamic Programming)

This document provides a complete, step-by-step mathematical solution for the **Binomial Coefficient DP numerical** problem from the SPPU DAA Endsem Examination (specifically 2024/2025 Paper, 10 Marks).

---

## 1. Algorithmic Foundation & Recurrence Relation

### 1.1 Problem Definition & Binomial Theorem
The binomial theorem states that for any non-negative integer $n$:
$$(x + y)^n = \sum_{k=0}^n \binom{n}{k} x^{n-k} y^k = \binom{n}{0} x^n + \binom{n}{1} x^{n-1} y + \binom{n}{2} x^{n-2} y^2 + \dots + \binom{n}{n} y^n$$

Where $\binom{n}{k} = C(n, k)$ represents the number of ways to choose $k$ items from $n$ items:
$$C(n, k) = \frac{n!}{k! \, (n - k)!}$$

Direct calculation using factorials suffers from arithmetic overflow for even moderately large values of $n$. Dynamic Programming evaluates $C(n, k)$ additively using Pascal's identity without factorials or division.

### 1.2 Dynamic Programming Recurrence (Pascal's Identity)
The recurrence relation based on Pascal's triangle is:
$$C(n, k) = \begin{cases} 
1 & \text{if } k = 0 \text{ or } k = n \\
C(n - 1, k - 1) + C(n - 1, k) & \text{if } 0 < k < n 
\end{cases}$$

**Subproblem Interpretation:**
To choose $k$ elements out of $n$ elements including a specific element $e$:
1. If element $e$ is **included**, we must choose $k - 1$ elements from the remaining $n - 1$ elements $\implies C(n - 1, k - 1)$.
2. If element $e$ is **excluded**, we must choose $k$ elements from the remaining $n - 1$ elements $\implies C(n - 1, k)$.
3. Total choices: $C(n, k) = C(n - 1, k - 1) + C(n - 1, k)$.

### 1.3 Complexity Analysis
- **Time Complexity:** $O(n \cdot k)$ additions. For full expansion of $(x + y)^n$, computing all entries up to row $n$ takes $\sum_{i=0}^n (i + 1) = \frac{(n + 1)(n + 2)}{2} = O(n^2)$ time.
- **Space Complexity:** 
  - Standard 2D table: $O(n \cdot k)$ or $O(n^2)$.
  - Optimized 1D rolling array: $O(k)$ space by filling from right to left.

---

## 2. SPPU 2024/2025 PYQ Numerical [10 Marks]

### Problem Statement
> Find the binomial coefficients of $(x + y)^5$ using Dynamic Programming.
> Construct the DP table, show all intermediate calculations, and state the final polynomial expansion.

---

### Step 1: DP Table Setup
We need to compute $C(n, k)$ for $n = 0, 1, 2, 3, 4, 5$ and $k = 0, 1, \dots, n$.

Initialize a 2D matrix $C[0..5, 0..5]$.

---

### Step 2: Row-by-Row Dynamic Programming Computation

#### Row $n = 0$:
- $k = 0$: $C(0, 0) = \mathbf{1}$ (Base case)

#### Row $n = 1$:
- $k = 0$: $C(1, 0) = \mathbf{1}$ (Base case)
- $k = 1$: $C(1, 1) = \mathbf{1}$ (Base case)

#### Row $n = 2$:
- $k = 0$: $C(2, 0) = \mathbf{1}$
- $k = 1$: $C(2, 1) = C(1, 0) + C(1, 1) = 1 + 1 = \mathbf{2}$
- $k = 2$: $C(2, 2) = \mathbf{1}$

#### Row $n = 3$:
- $k = 0$: $C(3, 0) = \mathbf{1}$
- $k = 1$: $C(3, 1) = C(2, 0) + C(2, 1) = 1 + 2 = \mathbf{3}$
- $k = 2$: $C(3, 2) = C(2, 1) + C(2, 2) = 2 + 1 = \mathbf{3}$
- $k = 3$: $C(3, 3) = \mathbf{1}$

#### Row $n = 4$:
- $k = 0$: $C(4, 0) = \mathbf{1}$
- $k = 1$: $C(4, 1) = C(3, 0) + C(3, 1) = 1 + 3 = \mathbf{4}$
- $k = 2$: $C(4, 2) = C(3, 1) + C(3, 2) = 3 + 3 = \mathbf{6}$
- $k = 3$: $C(4, 3) = C(3, 2) + C(3, 3) = 3 + 1 = \mathbf{4}$
- $k = 4$: $C(4, 4) = \mathbf{1}$

#### Row $n = 5$ (Target Coefficients):
- $k = 0$: $C(5, 0) = \mathbf{1}$
- $k = 1$: $C(5, 1) = C(4, 0) + C(4, 1) = 1 + 4 = \mathbf{5}$
- $k = 2$: $C(5, 2) = C(4, 1) + C(4, 2) = 4 + 6 = \mathbf{10}$
- $k = 3$: $C(5, 3) = C(4, 2) + C(4, 3) = 6 + 4 = \mathbf{10}$
- $k = 4$: $C(5, 4) = C(4, 3) + C(4, 4) = 4 + 1 = \mathbf{5}$
- $k = 5$: $C(5, 5) = \mathbf{1}$

---

### Step 3: Complete Dynamic Programming Table (Pascal's Matrix)

| $n \ \backslash \ k$ | $k = 0$ | $k = 1$ | $k = 2$ | $k = 3$ | $k = 4$ | $k = 5$ |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | **1** | - | - | - | - | - |
| **1** | **1** | **1** | - | - | - | - |
| **2** | **1** | **2** | **1** | - | - | - |
| **3** | **1** | **3** | **3** | **1** | - | - |
| **4** | **1** | **4** | **6** | **4** | **1** | - |
| **5** | **1** | **5** | **10** | **10** | **5** | **1** |

---

### Step 4: Optimized 1D Array Execution Trace
Using a 1D array `C[0..5]` initialized to `[1, 0, 0, 0, 0, 0]`:

```
i = 1: C[1] = C[1] + C[0] = 0 + 1 = 1           --> [1, 1, 0, 0, 0, 0]
i = 2: C[2] = C[2] + C[1] = 0 + 1 = 1
       C[1] = C[1] + C[0] = 1 + 1 = 2           --> [1, 2, 1, 0, 0, 0]
i = 3: C[3] = C[3] + C[2] = 0 + 1 = 1
       C[2] = C[2] + C[1] = 1 + 2 = 3
       C[1] = C[1] + C[0] = 2 + 1 = 3           --> [1, 3, 3, 1, 0, 0]
i = 4: C[4] = C[4] + C[3] = 0 + 1 = 1
       C[3] = C[3] + C[2] = 1 + 3 = 4
       C[2] = C[2] + C[1] = 3 + 3 = 6
       C[1] = C[1] + C[0] = 3 + 1 = 4           --> [1, 4, 6, 4, 1, 0]
i = 5: C[5] = C[5] + C[4] = 0 + 1 = 1
       C[4] = C[4] + C[3] = 1 + 4 = 5
       C[3] = C[3] + C[2] = 4 + 6 = 10
       C[2] = C[2] + C[1] = 6 + 4 = 10
       C[1] = C[1] + C[0] = 4 + 1 = 5           --> [1, 5, 10, 10, 5, 1]
```

---

### Step 5: Final Result & Polynomial Expansion
The computed binomial coefficients for $n = 5$ are:
$$\binom{5}{0} = 1, \quad \binom{5}{1} = 5, \quad \binom{5}{2} = 10, \quad \binom{5}{3} = 10, \quad \binom{5}{4} = 5, \quad \binom{5}{5} = 1$$

Substituting these coefficients into the binomial expansion formula:
$$(x + y)^5 = \mathbf{1x^5 + 5x^4y + 10x^3y^2 + 10x^2y^3 + 5xy^4 + 1y^5}$$
$$(x + y)^5 = \mathbf{x^5 + 5x^4y + 10x^3y^2 + 10x^2y^3 + 5xy^4 + y^5}$$



<div style="page-break-after: always;"></div>

---

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



<div style="page-break-after: always;"></div>

---

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



<div style="page-break-after: always;"></div>

---

# Topic 09: 0/1 Knapsack Problem (Least-Cost Branch & Bound)

This document provides complete, step-by-step mathematical solutions and state-space tree traces for the **Least-Cost Branch and Bound (LCBB) 0/1 Knapsack** numerical problems appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition & LCBB Concept
Given $n$ items, each with profit $p_i$ and weight $w_i$, and knapsack capacity $W$.
Unlike Backtracking (which traverses depth-first), **Branch and Bound** maintains a pool of active live nodes in a Priority Queue (min-heap) and always explores the most promising live node first (**Least-Cost Search / LCBB**).

Because Branch and Bound algorithms are traditionally formulated as **minimization problems**, we convert the maximization of profit into minimization of cost:
$$\text{Cost } c(x) = -\text{Profit}(x)$$

### 1.2 Bounding Functions
At any node $x$ at level $i$ in the state-space tree:
- **Current Weight:** $cw$
- **Current Profit:** $cp$
- **Remaining Capacity:** $rem = W - cw$

If $cw > W$, the node is **infeasible** and killed immediately.

To compute the lower bound on cost $\hat{c}(x)$ (or upper bound on profit $\hat{u}(x)$), we solve the **Fractional (Continuous) Knapsack Problem** on the remaining items $i+1 \dots n$:
1. Arrange remaining items in non-increasing order of profit-to-weight ratio:
   $$\frac{p_1}{w_1} \ge \frac{p_2}{w_2} \ge \dots \ge \frac{p_n}{w_n}$$
2. Greedily fill the remaining capacity $rem$ with whole items.
3. If capacity remains and item $k$ cannot be taken completely, take the fraction $\frac{rem}{w_k}$ of item $k$.
4. Upper Bound on Profit:
   $$\hat{u}(x) = cp + \sum_{j=i+1}^{k-1} p_j + \left(\frac{rem}{w_k}\right) \cdot p_k$$
5. Lower Bound on Cost:
   $$\hat{c}(x) = -\hat{u}(x)$$

Any node whose $\hat{c}(x) \ge \text{Upper Bound Cost } U$ is **pruned** because it cannot yield a solution better than the best solution found so far.

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU Dec 2022/Jan 2023 & Nov/Dec 2023 [9 Marks]

#### Problem Statement
> Explain the Branch and Bound approach. Solve the following 0/1 Knapsack problem using Least-Cost Branch and Bound (LCBB):
> - Knapsack Capacity: $W = 15\text{ kg}$
> - Items:
>   - Item A: Profit $= 18$, Weight $= 9\text{ kg}$
>   - Item B: Profit $= 10$, Weight $= 4\text{ kg}$
>   - Item C: Profit $= 12$, Weight $= 6\text{ kg}$
>   - Item D: Profit $= 10$, Weight $= 2\text{ kg}$
>
> Show the calculation of upper/lower bounds, priority queue progression, and draw the state-space tree.

---

#### Step 1: Sorting Items by Ratio $p_i / w_i$ (Descending)

| Original Item | Profit ($p_i$) | Weight ($w_i$) | Ratio ($p_i / w_i$) | New Index ($i$) |
| :---: | :---: | :---: | :---: | :---: |
| **D** | 10 | 2 | $10 / 2 = 5.0$ | **Item 1** |
| **B** | 10 | 4 | $10 / 4 = 2.5$ | **Item 2** |
| **A** | 18 | 9 | $18 / 9 = 2.0$ | **Item 3** |
| **C** | 12 | 6 | $12 / 6 = 2.0$ | **Item 4** |

Total capacity: $W = 15\text{ kg}$.

---

#### Step 2: Root Node Bound Computation (Node 1)
- At Node 1: $cw = 0, cp = 0, rem = 15$.
- Greedily pack items by ratio:
  - Add Item 1 (D): $cw = 0 + 2 = 2 \le 15, \quad cp = 0 + 10 = 10, \quad rem = 13$
  - Add Item 2 (B): $cw = 2 + 4 = 6 \le 15, \quad cp = 10 + 10 = 20, \quad rem = 9$
  - Add Item 3 (A): $cw = 6 + 9 = 15 \le 15, \quad cp = 20 + 18 = 38, \quad rem = 0$
- Capacity completely filled with whole items.
- Maximum Upper Bound Profit: $\hat{u}(1) = 38$.
- Cost: $\hat{c}(1) = -\hat{u}(1) = \mathbf{-38}$.
- Initial Best Profit: $U = 38$ (Cost $= -38$).

---

#### Step 3: Priority Queue & Tree Node Generation

##### Level 1: Branch on Item 1 (D, $w=2, p=10$)

1. **Left Child: Include Item 1 ($x_1 = 1$) $\implies$ Node 2:**
   - $cw = 2, cp = 10, rem = 15 - 2 = 13$.
   - Bound: Fill remaining $13$ with items 2 (B) and 3 (A):
     - Take Item 2 (B): $w = 4, p = 10 \implies rem = 9, cp = 20$.
     - Take Item 3 (A): $w = 9, p = 18 \implies rem = 0, cp = 38$.
   - $\hat{u}(2) = 38 \implies \mathbf{\hat{c}(2) = -38}$.

2. **Right Child: Exclude Item 1 ($x_1 = 0$) $\implies$ Node 3:**
   - $cw = 0, cp = 0, rem = 15$.
   - Bound with items 2, 3, 4:
     - Take Item 2 (B): $w = 4, p = 10 \implies rem = 11, cp = 10$.
     - Take Item 3 (A): $w = 9, p = 18 \implies rem = 2, cp = 28$.
     - Fractional Item 4 (C): take $\frac{2}{6} \times 12 = 4 \implies cp = 28 + 4 = 32$.
   - $\hat{u}(3) = 32 \implies \mathbf{\hat{c}(3) = -32}$.

- **Active Priority Queue:**
  `[ Node 2 (c = -38), Node 3 (c = -32) ]`

---

##### Level 2: Expand Node 2 ($\hat{c} = -38$, Branch on Item 2 (B, $w=4, p=10$))

1. **Left Child: Include Item 2 ($x_2 = 1$) $\implies$ Node 4:**
   - $cw = 2 + 4 = 6, \quad cp = 10 + 10 = 20, \quad rem = 15 - 6 = 9$.
   - Bound with items 3, 4:
     - Take Item 3 (A): $w = 9, p = 18 \implies rem = 0, cp = 38$.
   - $\hat{u}(4) = 38 \implies \mathbf{\hat{c}(4) = -38}$.

2. **Right Child: Exclude Item 2 ($x_2 = 0$) $\implies$ Node 5:**
   - $cw = 2, \quad cp = 10, \quad rem = 13$.
   - Bound with items 3, 4:
     - Take Item 3 (A): $w = 9, p = 18 \implies rem = 4, cp = 28$.
     - Fractional Item 4 (C): take $\frac{4}{6} \times 12 = 8 \implies cp = 28 + 8 = 36$.
   - $\hat{u}(5) = 36 \implies \mathbf{\hat{c}(5) = -36}$.

- **Active Priority Queue:**
  `[ Node 4 (c = -38), Node 5 (c = -36), Node 3 (c = -32) ]`

---

##### Level 3: Expand Node 4 ($\hat{c} = -38$, Branch on Item 3 (A, $w=9, p=18$))

1. **Left Child: Include Item 3 ($x_3 = 1$) $\implies$ Node 6:**
   - $cw = 6 + 9 = 15, \quad cp = 20 + 18 = 38, \quad rem = 0$.
   - Knapsack is full ($rem = 0$). Cannot take Item 4.
   - Feasible Integer Solution: Profit $= 38$, Cost $= \mathbf{-38}$.
   - Best solution found so far: $U = -38$.

2. **Right Child: Exclude Item 3 ($x_3 = 0$) $\implies$ Node 7:**
   - $cw = 6, \quad cp = 20, \quad rem = 9$.
   - Bound with Item 4 (C):
     - Take Item 4 (C): $w = 6, p = 12 \implies cw = 12, cp = 32$.
   - $\hat{u}(7) = 32 \implies \mathbf{\hat{c}(7) = -32}$.

- **Active Priority Queue:**
  - Node 5: $\hat{c} = -36 > -38 \implies$ **PRUNED!**
  - Node 7: $\hat{c} = -32 > -38 \implies$ **PRUNED!**
  - Node 3: $\hat{c} = -32 > -38 \implies$ **PRUNED!**

No active nodes remaining with cost $< -38$. Search terminates.

---

#### Step 4: State-Space Tree Diagram

```
                                  [ Node 1 ]
                              cw=0, cp=0, c=-38
                             /                 \
                  Include D /                   \ Exclude D
                           /                     \
                      [ Node 2 ]              [ Node 3 ]
                  cw=2, cp=10, c=-38      cw=0, cp=0, c=-32
                 /                  \            (PRUNED)
      Include B /                    \ Exclude B
               /                      \
          [ Node 4 ]               [ Node 5 ]
      cw=6, cp=20, c=-38       cw=2, cp=10, c=-36
     /                  \             (PRUNED)
Include A /          Exclude A \
         /                      \
    [ Node 6 ]               [ Node 7 ]
 cw=15, cp=38, c=-38     cw=6, cp=20, c=-32
   (OPTIMAL LEAF)             (PRUNED)
```

---

#### Step 5: Final Solution
- **Selected Items:** $\{D, B, A\}$ (Item 1, Item 2, Item 3)
- **Solution Vector:** $(x_D, x_B, x_A, x_C) = (1, 1, 1, 0)$
- **Total Weight:** $w_D + w_B + w_A = 2 + 4 + 9 = 15\text{ kg} \le 15\text{ kg}$
- **Maximum Profit:**
  $$\text{Total Profit} = p_D + p_B + p_A = 10 + 10 + 18 = \mathbf{38}$$

---

### Instance B: SPPU Nov/Dec 2024 [10 Marks]

#### Problem Statement
> Given three objects with weights and values:
> - Object $O_1$: Weight $= 5$, Value $= 6$
> - Object $O_2$: Weight $= 4$, Value $= 5$
> - Object $O_3$: Weight $= 3$, Value $= 4$
> - Knapsack Capacity: $m = 7$
>
> Solve this 0/1 Knapsack problem using Least-Cost Branch and Bound (LCBB).

---

#### Step 1: Sorting by Value-to-Weight Ratio

| Object | Weight ($w$) | Value ($v$) | Ratio ($v / w$) | New Index ($i$) |
| :---: | :---: | :---: | :---: | :---: |
| **$O_3$** | 3 | 4 | $4 / 3 \approx 1.33$ | 1 |
| **$O_2$** | 4 | 5 | $5 / 4 = 1.25$ | 2 |
| **$O_1$** | 5 | 6 | $6 / 5 = 1.20$ | 3 |

---

#### Step 2: Node Expansions & Bound Calculations

1. **Root Node 1 ($cw = 0, cv = 0, rem = 7$):**
   - Pack $O_3$: $w = 3, v = 4 \implies rem = 4$
   - Pack $O_2$: $w = 4, v = 5 \implies rem = 0$
   - $\hat{u}(1) = 4 + 5 = 9 \implies \mathbf{\hat{c}(1) = -9}$.

2. **Branch on $O_3$ ($w=3, v=4$):**
   - **Node 2 (Include $O_3$, $x_1 = 1$):**
     - $cw = 3, cv = 4, rem = 4$.
     - Bound with remaining items ($O_2, O_1$):
       - Pack $O_2$: $w = 4, v = 5 \implies rem = 0, cv = 9$.
     - $\hat{u}(2) = 9 \implies \mathbf{\hat{c}(2) = -9}$.
   - **Node 3 (Exclude $O_3$, $x_1 = 0$):**
     - $cw = 0, cv = 0, rem = 7$.
     - Bound with $O_2, O_1$:
       - Pack $O_2$: $w = 4, v = 5 \implies rem = 3, cv = 5$.
       - Fractional $O_1$: $\frac{3}{5} \times 6 = 3.6 \implies cv = 5 + 3.6 = 8.6$.
     - $\hat{u}(3) = 8.6 \implies \mathbf{\hat{c}(3) = -8.6}$.

3. **Expand Node 2 ($\hat{c} = -9$, Branch on $O_2$ ($w=4, v=5$)):**
   - **Node 4 (Include $O_2$, $x_2 = 1$):**
     - $cw = 3 + 4 = 7, \quad cv = 4 + 5 = 9, \quad rem = 0$.
     - Capacity full. Cannot add $O_1$ ($x_3 = 0$).
     - Feasible solution: Value $= 9$, Cost $= \mathbf{-9}$. Best cost $U = -9$.
   - **Node 5 (Exclude $O_2$, $x_2 = 0$):**
     - $cw = 3, cv = 4, rem = 4$.
     - Bound with $O_1$:
       - Fractional $O_1$: $\frac{4}{5} \times 6 = 4.8 \implies \hat{u} = 4 + 4.8 = 8.8$.
     - $\hat{u}(5) = 8.8 \implies \mathbf{\hat{c}(5) = -8.8}$.
     - Since $\hat{c}(5) = -8.8 > -9$, **Node 5 is PRUNED!**

4. **Pruning Remaining Nodes:**
   - Node 3 has $\hat{c}(3) = -8.6 > -9 \implies$ **Node 3 is PRUNED!**

---

#### Step 3: Final Output
- **Optimal Objects Selected:** $\{O_2, O_3\}$
- **Solution Vector:** $(x_{O_1}, x_{O_2}, x_{O_3}) = (0, 1, 1)$
- **Total Weight:** $4 + 3 = 7 \le 7$
- **Maximum Value:**
  $$\text{Total Value} = 5 + 4 = \mathbf{9}$$



<div style="page-break-after: always;"></div>

---

# Topic 10: Traveling Salesman Problem (Branch & Bound Matrix Reduction)

This document provides complete, step-by-step mathematical solutions, matrix reductions, and state-space tree traces for the **Traveling Salesman Problem (TSP) using Least-Cost Branch and Bound (LCBB)** appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition
Given a directed or undirected graph $G = (V, E)$ with $n$ vertices (cities) and an $n \times n$ cost matrix $C$, where $C[i, j]$ is the cost of traveling directly from city $i$ to city $j$. If no direct edge exists, $C[i, j] = \infty$. The diagonal entries $C[i, i] = \infty$.

The objective is to find a **simple closed tour** (Hamiltonian cycle) that visits every city exactly once and returns to the starting city with **minimum total travel cost**.

### 1.2 Matrix Reduction Technique & Lower Bound
A cost matrix is said to be **reduced** if and only if:
1. Every row contains at least one zero element.
2. Every column contains at least one zero element.

#### Reduction Procedure:
1. **Row Reduction:** For each row $i$, find the row minimum $r_i = \min_{1 \le j \le n} C[i, j]$ (ignoring $\infty$). If $0 < r_i < \infty$, subtract $r_i$ from every element in row $i$.
2. **Column Reduction:** In the row-reduced matrix, for each column $j$, find the column minimum $c_j = \min_{1 \le i \le n} C[i, j]$. If $0 < c_j < \infty$, subtract $c_j$ from every element in column $j$.
3. **Lower Bound $\hat{c}(\text{Root})$:** The sum of all row and column reduction constants represents the minimum cost any tour must incur:
   $$\hat{c}(\text{Root}) = \sum_{i=1}^n r_i + \sum_{j=1}^n c_j$$

### 1.3 State-Space Branching & Child Lower Bounds
Let Node $A$ have reduced matrix $M_A$ with lower bound $\hat{c}(A)$.
When branching along the directed edge $(i, j)$ to child Node $B$:
1. Add the edge cost: Look up $M_A[i, j]$.
2. Set row $i$ of $M_A$ to $\infty$ (cannot leave city $i$ again).
3. Set column $j$ of $M_A$ to $\infty$ (cannot enter city $j$ again).
4. Set $M_A[j, \text{start}]$ (or the edge closing a premature sub-tour) to $\infty$.
5. Reduce the resulting matrix by finding row reductions $r$ and column reductions $c$.
6. Child Lower Bound:
   $$\hat{c}(B) = \hat{c}(A) + M_A[i, j] + \sum r + \sum c$$

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU 2024/2025 Examination Numerical [10 Marks]

#### Problem Statement
> A traveling salesperson has to make a trip between three cities $P, Q, R$. The cost matrix is given below. No edge is indicated by $\infty$. The tour will start from city $P$. Solve this traveling salesperson problem using the Branch and Bound method.
>
> $$G = \begin{pmatrix} 
> \infty & 4 & 2 \\ 
> 3 & \infty & 4 \\ 
> 1 & 8 & \infty 
> \end{pmatrix}$$
>
> (City mapping: $1 = P, 2 = Q, 3 = R$)

---

#### Step 1: Root Node Matrix Reduction (Node 1, Starting at $P$)

Initial Cost Matrix $M_0$:
$$\begin{pmatrix} 
\infty & 4 & 2 \\ 
3 & \infty & 4 \\ 
1 & 8 & \infty 
\end{pmatrix}$$

##### 1. Row Reduction:
- **Row 1 ($P$):** Minimum non-$\infty$ element is $\min(4, 2) = \mathbf{2}$.
  Subtract 2: $[\infty, 4 - 2, 2 - 2] = [\infty, 2, 0]$
- **Row 2 ($Q$):** Minimum non-$\infty$ element is $\min(3, 4) = \mathbf{3}$.
  Subtract 3: $[3 - 3, \infty, 4 - 3] = [0, \infty, 1]$
- **Row 3 ($R$):** Minimum non-$\infty$ element is $\min(1, 8) = \mathbf{1}$.
  Subtract 1: $[1 - 1, 8 - 1, \infty] = [0, 7, \infty]$

Total Row Reduction:
$$R_1 = 2 + 3 + 1 = \mathbf{6}$$

Matrix after Row Reduction:
$$\begin{pmatrix} 
\infty & 2 & 0 \\ 
0 & \infty & 1 \\ 
0 & 7 & \infty 
\end{pmatrix}$$

##### 2. Column Reduction:
- **Col 1 ($P$):** Minimum is $\min(\infty, 0, 0) = \mathbf{0}$.
- **Col 2 ($Q$):** Minimum is $\min(2, \infty, 7) = \mathbf{2}$.
  Subtract 2: $[2 - 2, \infty, 7 - 2]^T = [0, \infty, 5]^T$
- **Col 3 ($R$):** Minimum is $\min(0, 1, \infty) = \mathbf{0}$.

Total Column Reduction:
$$C_1 = 0 + 2 + 0 = \mathbf{2}$$

##### 3. Root Lower Bound $\hat{c}(1)$:
$$\hat{c}(1) = R_1 + C_1 = 6 + 2 = \mathbf{8}$$

Reduced Matrix $A_1$ for Root Node 1:
$$A_1 = \begin{pmatrix} 
\infty & 0 & 0 \\ 
0 & \infty & 1 \\ 
0 & 5 & \infty 
\end{pmatrix}$$

---

#### Step 2: Branching from Root Node 1 (Start City $P$)

From city $P$ (City 1), we can visit either city $Q$ (City 2) or city $R$ (City 3).

---

##### Child 1: Path $P \to Q$ (Node 2, Edge $(1, 2)$)
- Base cost: $\hat{c}(1) = 8$
- Edge cost from $A_1$: $A_1[1, 2] = 0$
- Transformations to $A_1$:
  1. Set Row 1 ($P$) to $\infty$.
  2. Set Col 2 ($Q$) to $\infty$.
  3. Prevent premature cycle $Q \to P$: Set $A[2, 1] = \infty$.

Matrix before reduction:
$$\begin{pmatrix} 
\infty & \infty & \infty \\ 
\infty & \infty & 1 \\ 
0 & \infty & \infty 
\end{pmatrix}$$

- **Row Reduction:**
  - Row 1: All $\infty$ (skip)
  - Row 2: Minimum non-$\infty$ is $1$. Subtract 1: $[\infty, \infty, 0]$
  - Row 3: Minimum non-$\infty$ is $0$.
  - Row reduction sum $= 1$.
- **Col Reduction:**
  - Col 1: Minimum is $0$.
  - Col 2: All $\infty$ (skip)
  - Col 3: Minimum is $0$.
  - Col reduction sum $= 0$.

Total new reduction $= 1 + 0 = 1$.
$$\mathbf{\hat{c}(\text{Node } 2)} = \hat{c}(1) + A_1[1, 2] + \text{reductions} = 8 + 0 + 1 = \mathbf{9}$$

Tour from Node 2 ($P \to Q$):
Remaining path is uniquely $Q \to R \to P$.
In original matrix:
$$\text{Cost}(P \to Q \to R \to P) = C[P, Q] + C[Q, R] + C[R, P] = 4 + 4 + 1 = \mathbf{9}$$

---

##### Child 2: Path $P \to R$ (Node 3, Edge $(1, 3)$)
- Base cost: $\hat{c}(1) = 8$
- Edge cost from $A_1$: $A_1[1, 3] = 0$
- Transformations to $A_1$:
  1. Set Row 1 ($P$) to $\infty$.
  2. Set Col 3 ($R$) to $\infty$.
  3. Prevent premature cycle $R \to P$: Set $A[3, 1] = \infty$.

Matrix before reduction:
$$\begin{pmatrix} 
\infty & \infty & \infty \\ 
0 & \infty & \infty \\ 
\infty & 5 & \infty 
\end{pmatrix}$$

- **Row Reduction:**
  - Row 1: All $\infty$ (skip)
  - Row 2: Minimum is $0$.
  - Row 3: Minimum is $5$. Subtract 5: $[\infty, 0, \infty]$.
  - Row reduction sum $= 5$.
- **Col Reduction:**
  - Col 1: Minimum is $0$.
  - Col 2: Minimum is $0$.
  - Col 3: All $\infty$ (skip).
  - Col reduction sum $= 0$.

Total new reduction $= 5 + 0 = 5$.
$$\mathbf{\hat{c}(\text{Node } 3)} = \hat{c}(1) + A_1[1, 3] + \text{reductions} = 8 + 0 + 5 = \mathbf{13}$$

Tour from Node 3 ($P \to R$):
Remaining path is uniquely $R \to Q \to P$.
In original matrix:
$$\text{Cost}(P \to R \to Q \to P) = C[P, R] + C[R, Q] + C[Q, P] = 2 + 8 + 3 = \mathbf{13}$$

---

#### Step 3: State-Space Tree

```
                          [ Node 1 ]
                    (Start P, Lower Bound = 8)
                   /                          \
       Path P -> Q /                            \ Path P -> R
                  /                              \
            [ Node 2 ]                      [ Node 3 ]
       (Lower Bound = 9)               (Lower Bound = 13)
              |                                (PRUNED)
    Complete Tour: P->Q->R->P
        Total Cost = 9
      [ OPTIMAL TOUR ]
```

---

#### Step 4: Final Conclusion
- **Optimal TSP Tour:**
  $$P \longrightarrow Q \longrightarrow R \longrightarrow P$$
- **Cost Breakdown:**
  - $P \to Q = 4$
  - $Q \to R = 4$
  - $R \to P = 1$
- **Minimum Total Tour Cost:**
  $$\text{Cost} = 4 + 4 + 1 = \mathbf{9}$$

---

### Instance B: Classic 4-City Benchmark Problem [10 Marks]

#### Problem Statement
> Solve the Traveling Salesman Problem using LCBB matrix reduction for the 4-city cost matrix starting from City 1:
>
> $$C = \begin{pmatrix} 
> \infty & 10 & 15 & 20 \\ 
> 5 & \infty & 9 & 10 \\ 
> 6 & 13 & \infty & 12 \\ 
> 8 & 8 & 9 & \infty 
> \end{pmatrix}$$

---

#### Step 1: Root Node Reduction (Node 1)
1. **Row Minimums:**
   - Row 1: $\min(10, 15, 20) = 10 \implies [\infty, 0, 5, 10]$
   - Row 2: $\min(5, 9, 10) = 5 \implies [0, \infty, 4, 5]$
   - Row 3: $\min(6, 13, 12) = 6 \implies [0, 7, \infty, 6]$
   - Row 4: $\min(8, 8, 9) = 8 \implies [0, 0, 1, \infty]$
   - Total Row Reduction: $10 + 5 + 6 + 8 = 29$.
2. **Column Minimums:**
   - Col 1: $\min(\infty, 0, 0, 0) = 0$
   - Col 2: $\min(0, \infty, 7, 0) = 0$
   - Col 3: $\min(5, 4, \infty, 1) = 1 \implies [4, 3, \infty, 0]^T$
   - Col 4: $\min(10, 5, 6, \infty) = 5 \implies [5, 0, 1, \infty]^T$
   - Total Column Reduction: $0 + 0 + 1 + 5 = 6$.
3. **Root Bound:**
   $$\hat{c}(1) = 29 + 6 = \mathbf{35}$$

Reduced Root Matrix $A_1$:
$$A_1 = \begin{pmatrix} 
\infty & 0 & 4 & 5 \\ 
0 & \infty & 3 & 0 \\ 
0 & 7 & \infty & 1 \\ 
0 & 0 & 0 & \infty 
\end{pmatrix}$$

---

#### Step 2: Children of Root Node 1 (Branches from City 1)

1. **Path $1 \to 2$ (Node 2):**
   - $A_1[1, 2] = 0$.
   - Row 1 $= \infty$, Col 2 $= \infty$, $A_1[2, 1] = \infty$.
   - Reduction sum $= 0$.
   $$\mathbf{\hat{c}(2)} = 35 + 0 + 0 = \mathbf{35}$$

2. **Path $1 \to 3$ (Node 3):**
   - $A_1[1, 3] = 4$.
   - Row 1 $= \infty$, Col 3 $= \infty$, $A_1[3, 1] = \infty$.
   - Reduction sum $= 11$.
   $$\mathbf{\hat{c}(3)} = 35 + 4 + 11 = \mathbf{50}$$

3. **Path $1 \to 4$ (Node 4):**
   - $A_1[1, 4] = 5$.
   - Row 1 $= \infty$, Col 4 $= \infty$, $A_1[4, 1] = \infty$.
   - Reduction sum $= 0$.
   $$\mathbf{\hat{c}(4)} = 35 + 5 + 0 = \mathbf{40}$$

---

#### Step 3: Expand Most Promising Node (Node 2, $\hat{c} = 35$)
From City 2, branches are to City 3 or City 4:
1. **Path $1 \to 2 \to 3$ (Node 5):**
   - Edge cost $A_2[2, 3] = 3$. Reduction $= 0$.
   $$\hat{c}(5) = 35 + 3 + 0 = \mathbf{38}$$
2. **Path $1 \to 2 \to 4$ (Node 6):**
   - Edge cost $A_2[2, 4] = 0$. Reduction $= 0$.
   $$\mathbf{\hat{c}(6)} = 35 + 0 + 0 = \mathbf{35}$$

---

#### Step 4: Expand Node 6 ($1 \to 2 \to 4$, $\hat{c} = 35$)
Only unvisited city is City 3. Tour must be:
$$1 \longrightarrow 2 \longrightarrow 4 \longrightarrow 3 \longrightarrow 1$$
Total Cost in Original Matrix:
$$\text{Cost} = C[1, 2] + C[2, 4] + C[4, 3] + C[3, 1] = 10 + 10 + 9 + 6 = \mathbf{35}$$

All other active nodes (Node 3 with 50, Node 4 with 40, Node 5 with 38) have lower bounds $> 35$, and are immediately **pruned**.

- **Optimal Tour:** $1 \to 2 \to 4 \to 3 \to 1$
- **Minimum Cost:** $\mathbf{35}$



<div style="page-break-after: always;"></div>

---

# Topic 11: Amortized Analysis Derivations (Stack & Binary Counter)

This document provides rigorous, step-by-step mathematical proofs and derivations for the **Amortized Analysis** numerical problems appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Definition of Amortized Analysis
Amortized analysis guarantees the **average performance of each operation in the worst-case sequence of $n$ operations**. Unlike average-case analysis, it does not involve probabilities or input distributions; it provides an upper bound on the total runtime across an entire operation sequence.

### 1.2 The Three Core Methods
1. **Aggregate Method:**
   Show that a sequence of $n$ operations takes total worst-case time $T(n)$. The amortized cost per operation is then:
   $$\text{Amortized Cost } \hat{c} = \frac{T(n)}{n}$$
2. **Accounting (Banker's) Method:**
   Assign an artificial charge (amortized cost $\hat{c}_i$) to each operation. If $\hat{c}_i > c_i$ (actual cost), the excess credit is stored on specific data structure elements. If $\hat{c}_i < c_i$, the deficit is paid using stored credit.
   Valid iff total accumulated credit is non-negative at all times:
   $$\sum_{i=1}^n \hat{c}_i \ge \sum_{i=1}^n c_i \quad \forall n$$
3. **Potential (Physicist's) Method:**
   Define a potential function $\Phi(D)$ mapping the data structure state $D$ to a real number, such that $\Phi(D_0) = 0$ and $\Phi(D_i) \ge 0$ for all $i$.
   The amortized cost of the $i$-th operation is:
   $$\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$$
   Summing over $n$ operations:
   $$\sum_{i=1}^n \hat{c}_i = \sum_{i=1}^n c_i + \Phi(D_n) - \Phi(D_0) \ge \sum_{i=1}^n c_i$$

---

## 2. Derivation 1: Dynamic Stack Operations (PUSH, POP, MULTIPOP)

### Problem Statement (SPPU Frequency: 4)
> Consider a stack supporting operations:
> - `PUSH(S, x)`: Pushes element $x$ onto stack $S$. Cost $= 1$.
> - `POP(S)`: Pops the top element. Cost $= 1$.
> - `MULTIPOP(S, k)`: Pops top $\min(k, |S|)$ elements from stack. Actual Cost $= \min(k, |S|)$.
>
> Prove that the amortized cost per operation in an arbitrary sequence of $n$ operations on an initially empty stack is $O(1)$ using:
> 1. Aggregate Method
> 2. Accounting Method
> 3. Potential Method

---

### Step 1: Aggregate Analysis of Stack
- A single `MULTIPOP` operation can take $O(n)$ time in the worst case (if the stack contains $n$ elements).
- A naive bound for $n$ operations would suggest $n \times O(n) = O(n^2)$ time.
- **Key Insight:** An element can only be popped if it was previously pushed!
- In any sequence of $n$ operations starting from an empty stack:
  $$\text{Number of PUSH operations} \le n$$
- Each pushed element can be removed **at most once** by either `POP` or `MULTIPOP`.
  $$\text{Total number of elements popped across all POP/MULTIPOP} \le n$$
- Total cost across all $n$ operations:
  $$T(n) = \text{Cost}(\text{all PUSH}) + \text{Cost}(\text{all POP / MULTIPOP}) \le n + n = 2n$$
- Amortized cost per operation:
  $$\hat{c} = \frac{T(n)}{n} \le \frac{2n}{n} = \mathbf{2} = \mathbf{O(1)}$$

---

### Step 2: Accounting Method for Stack
Assign the following amortized costs:
- **`PUSH`:** Charge $\mathbf{\hat{c}_{\text{PUSH}} = 2}$ credits.
  - $1$ credit pays for the immediate execution of `PUSH`.
  - $1$ credit is attached to the pushed element as prepayment for its eventual removal.
- **`POP`:** Charge $\mathbf{\hat{c}_{\text{POP}} = 0}$ credits.
  - The actual cost of $1$ is paid using the $1$ credit previously deposited on that element.
- **`MULTIPOP(k)`:** Charge $\mathbf{\hat{c}_{\text{MULTIPOP}} = 0}$ credits.
  - The actual cost of $k'$ is paid using the $k'$ credits stored on the $k'$ popped elements.

Since the number of elements in the stack is always $\ge 0$, the stored credit is always $\ge 0$.
$$\sum_{i=1}^n \hat{c}_i \ge \sum_{i=1}^n c_i$$
Thus, each operation has an amortized cost of $\mathbf{O(1)}$.

---

### Step 3: Potential Method for Stack
Define the potential function $\Phi(D_i)$ as the number of elements in the stack $S$:
$$\Phi(D_i) = |S_i|$$
- Initially empty stack: $\Phi(D_0) = 0$.
- For any valid stack: $\Phi(D_i) = |S_i| \ge 0$, satisfying the non-negativity property.

1. **For `PUSH(S, x)`:**
   - Actual cost: $c_i = 1$
   - Stack size increases by 1: $\Phi(D_i) - \Phi(D_{i-1}) = (|S| + 1) - |S| = 1$
   - Amortized cost:
     $$\hat{c}_i = c_i + \Delta\Phi = 1 + 1 = \mathbf{2} = \mathbf{O(1)}$$

2. **For `MULTIPOP(S, k)`:**
   - Let $k' = \min(k, |S|)$. Actual cost: $c_i = k'$
   - Stack size decreases by $k'$: $\Phi(D_i) - \Phi(D_{i-1}) = (|S| - k') - |S| = -k'$
   - Amortized cost:
     $$\hat{c}_i = c_i + \Delta\Phi = k' + (-k') = \mathbf{0} = \mathbf{O(1)}$$

3. **For `POP(S)`:**
   - Actual cost: $c_i = 1$
   - $\Phi(D_i) - \Phi(D_{i-1}) = -1$
   - Amortized cost:
     $$\hat{c}_i = 1 + (-1) = \mathbf{0} = \mathbf{O(1)}$$

Every operation has amortized cost $O(1)$.

---

## 3. Derivation 2: $k$-Bit Binary Counter Increment

### Problem Statement (SPPU 2024/2025 [5M], Nov/Dec 2023 [9M], Dec 2022 [10M])
> Prove that the amortized cost per operation for incrementing a $k$-bit binary counter is $O(1)$ using:
> 1. Aggregate Analysis
> 2. Potential Method

---

### Step 1: Counter Mechanism & Binary Representation
A $k$-bit binary counter is stored as an array $A[0..k-1]$ of bits, where $A[0]$ is the least significant bit (LSB).
The counter starts at value $0$ (`00...00`).
The `INCREMENT` algorithm:
```c
INCREMENT(A):
    i = 0
    while i < k and A[i] == 1:
        A[i] = 0        // Carry propagation
        i = i + 1
    if i < k:
        A[i] = 1        // Set first 0-bit to 1
```

The cost of each `INCREMENT` is the number of bits flipped.
- In the worst case (e.g., incrementing `111...11` to `000...00`), all $k$ bits flip $\implies O(k)$ time.
- A naive bound for $n$ increments gives $O(n \cdot k)$.

---

### Step 2: Aggregate Analysis
Observe the frequency at which each bit position flips during $n$ consecutive increments from $0$:
- Bit $A[0]$ (LSB) flips on **every** increment:
  $$\text{Flips} = \left\lfloor \frac{n}{2^0} \right\rfloor = n$$
- Bit $A[1]$ flips on every **2nd** increment:
  $$\text{Flips} = \left\lfloor \frac{n}{2^1} \right\rfloor = \left\lfloor \frac{n}{2} \right\rfloor$$
- Bit $A[2]$ flips on every **4th** increment:
  $$\text{Flips} = \left\lfloor \frac{n}{2^2} \right\rfloor = \left\lfloor \frac{n}{4} \right\rfloor$$
- In general, bit $A[i]$ flips on every $2^i$-th increment:
  $$\text{Flips} = \left\lfloor \frac{n}{2^i} \right\rfloor$$

Total number of bit flips across all $n$ increments:
$$T(n) = \sum_{i=0}^{k-1} \left\lfloor \frac{n}{2^i} \right\rfloor < \sum_{i=0}^\infty \frac{n}{2^i} = n \sum_{i=0}^\infty \left(\frac{1}{2}\right)^i$$

Using the sum of an infinite geometric progression ($\sum_{i=0}^\infty r^i = \frac{1}{1-r}$ for $r = \frac{1}{2}$):
$$\sum_{i=0}^\infty \left(\frac{1}{2}\right)^i = \frac{1}{1 - 1/2} = 2$$

Therefore:
$$T(n) < 2n = O(n)$$

The amortized cost per increment is:
$$\hat{c} = \frac{T(n)}{n} < \frac{2n}{n} = \mathbf{2} = \mathbf{O(1)}$$

---

### Step 3: Potential Method Analysis
Define the potential function $\Phi(D_i)$ as the total number of `1` bits in the counter after $i$ increments:
$$\Phi(D_i) = b_i = \text{number of 1s in counter}$$
- Initial state: counter is 0 $\implies \Phi(D_0) = 0$.
- For all states: $b_i \ge 0 \implies \Phi(D_i) \ge 0$.

During the $i$-th increment:
- Suppose $t_i$ bits flip from `1` to `0` during carry propagation.
- If $t_i < k$, exactly one bit flips from `0` to `1`.
- Total bits flipped (actual cost):
  $$c_i = t_i + 1$$
- Number of 1s after the operation:
  $$b_i = b_{i-1} - t_i + 1$$
- The change in potential:
  $$\Delta\Phi = \Phi(D_i) - \Phi(D_{i-1}) = b_i - b_{i-1} = 1 - t_i$$

Now, calculate the amortized cost $\hat{c}_i$:
$$\hat{c}_i = c_i + \Delta\Phi = (t_i + 1) + (1 - t_i) = \mathbf{2}$$

Thus, the amortized cost of each increment operation is strictly bounded by:
$$\hat{c}_i = 2 = \mathbf{O(1)}$$

Total cost for $n$ increments is at most $2n = O(n)$.



<div style="page-break-after: always;"></div>

---

# Topic 12: String Matching Algorithms (Rabin-Karp & Naive)

This document provides complete, step-by-step mathematical calculations, rolling hash computations, and spurious hit analyses for the **Rabin-Karp and Naive String Matching** numerical problems appearing in SPPU DAA Endsem examination papers from 2022 to 2025.

---

## 1. Algorithmic Foundation & Theory

### 1.1 Problem Definition
Given a text array $T[0..n-1]$ of length $n$ and a pattern array $P[0..m-1]$ of length $m$ ($m \le n$) over an alphabet $\Sigma$ of size $d$.
We say that pattern $P$ occurs with **shift $s$** in text $T$ ($0 \le s \le n - m$) if:
$$T[s + j] = P[j] \quad \text{for all } 0 \le j \le m - 1$$

### 1.2 Rabin-Karp Rolling Hash Formulation
Rabin-Karp treats strings of decimal digits as numbers in base $d$ modulo a suitable prime $q$:
1. **Precomputing Exponent Multiplier:**
   $$h = d^{m-1} \bmod q$$
2. **Initial Pattern and Window Hash (Horner's Rule):**
   $$p = \left( \sum_{j=0}^{m-1} P[j] \cdot d^{m - 1 - j} \right) \bmod q$$
   $$t_0 = \left( \sum_{j=0}^{m-1} T[j] \cdot d^{m - 1 - j} \right) \bmod q$$
3. **Rolling Hash Transition (Constant Time $O(1)$):**
   To compute $t_{s+1}$ from $t_s$:
   - Remove high-order outgoing digit $T[s]$: subtract $T[s] \cdot h$
   - Shift left: multiply by $d$
   - Add incoming low-order digit $T[s+m]$
   $$t_{s+1} = \Big( d \cdot \big(t_s - T[s] \cdot h\big) + T[s + m] \Big) \bmod q$$
   *(If the result is negative in modular arithmetic, add $q$ until it is non-negative).*
4. **Spurious Hit vs. Valid Match:**
   - If $t_s \ne p$, then $T[s..s+m-1] \ne P$ (no match).
   - If $t_s = p$, test character-by-character:
     - If all $m$ characters match $\implies$ **Valid Match**.
     - If any character differs $\implies$ **Spurious Hit** (hash collision caused by modulo $q$).

### 1.3 Complexity Analysis
- **Preprocessing Time:** $\Theta(m)$ to compute $p$ and $t_0$.
- **Matching Time (Average / Best Case):** $O(n - m + 1) = O(n)$ when spurious hits are rare.
- **Worst-Case Time:** $O((n - m + 1) \cdot m)$ when hash collisions occur at every shift (e.g., $T = a^n, P = a^m, q = 2$).
- **Space Complexity:** $O(1)$ auxiliary space.

---

## 2. PYQ Problem Instances & Step-by-Step Solutions

### Instance A: SPPU 2024/2025 Examination [10 Marks]

#### Problem Statement
> Let the hash code for the text be $T = \text{"4133124"}$ and that for the pattern matching text be $P = \text{"124"}$. Use modulo arithmetic with prime number $q = 13$ and base $d = 10$. Apply the Rabin-Karp string matching algorithm for this case. Show the stepwise process of matching the pattern, evaluate all rolling hashes, and report spurious hits and valid matches.

---

#### Step 1: Input Setup & Preprocessing

- Text: $T = \text{"4133124"}, \quad n = 7$
- Pattern: $P = \text{"124"}, \quad m = 3$
- Radix: $d = 10$
- Prime modulus: $q = 13$
- Possible shifts: $s \in \{0, 1, 2, 3, 4\}$ ($n - m = 7 - 3 = 4$).

**1. Multiplier $h$:**
$$h = d^{m-1} \bmod q = 10^{3-1} \bmod 13 = 100 \bmod 13$$
$$100 = 13 \times 7 + 9 \implies \mathbf{h = 9}$$

**2. Pattern Hash $p$:**
$$p = 124 \bmod 13$$
$$124 = 13 \times 9 + 7 = 117 + 7 \implies \mathbf{p = 7}$$

**3. Initial Window Hash $t_0$ (Text substring $T[0..2] = \text{"413"}$):**
$$t_0 = 413 \bmod 13$$
$$413 = 13 \times 31 + 10 = 403 + 10 \implies \mathbf{t_0 = 10}$$

---

#### Step 2: Stepwise Shift Trace & Rolling Hash Calculations

##### Shift $s = 0$ (Substring $T[0..2] = \text{"413"}$):
- Hash: $t_0 = 10$
- Pattern hash: $p = 7$
- Compare: $t_0 \ne p$ ($10 \ne 7$).
- **Result:** No match.

---

##### Shift $s = 1$ (Substring $T[1..3] = \text{"133"}$):
- Outgoing character: $T[0] = 4$
- Incoming character: $T[3] = 3$
- Rolling formula:
  $$t_1 = \Big( 10 \cdot (t_0 - T[0] \cdot h) + T[3] \Big) \bmod 13$$
  $$T[0] \cdot h = 4 \times 9 = 36 \equiv 10 \pmod{13}$$
  $$t_0 - (T[0] \cdot h) = 10 - 10 = 0$$
  $$t_1 = (10 \times 0 + 3) \bmod 13 = \mathbf{3}$$
- Compare: $t_1 = 3 \ne p (7)$.
- **Result:** No match.

---

##### Shift $s = 2$ (Substring $T[2..4] = \text{"331"}$):
- Outgoing character: $T[1] = 1$
- Incoming character: $T[4] = 1$
- Rolling formula:
  $$t_2 = \Big( 10 \cdot (t_1 - T[1] \cdot h) + T[4] \Big) \bmod 13$$
  $$T[1] \cdot h = 1 \times 9 = 9$$
  $$t_1 - 9 = 3 - 9 = -6 \equiv -6 + 13 = 7 \pmod{13}$$
  $$t_2 = (10 \times 7 + 1) \bmod 13 = 71 \bmod 13$$
  $$71 = 13 \times 5 + 6 \implies \mathbf{t_2 = 6}$$
- Compare: $t_2 = 6 \ne p (7)$.
- **Result:** No match.

---

##### Shift $s = 3$ (Substring $T[3..5] = \text{"312"}$):
- Outgoing character: $T[2] = 3$
- Incoming character: $T[5] = 2$
- Rolling formula:
  $$t_3 = \Big( 10 \cdot (t_2 - T[2] \cdot h) + T[5] \Big) \bmod 13$$
  $$T[2] \cdot h = 3 \times 9 = 27 \equiv 1 \pmod{13}$$
  $$t_2 - 1 = 6 - 1 = 5$$
  $$t_3 = (10 \times 5 + 2) \bmod 13 = 52 \bmod 13$$
  $$52 = 13 \times 4 + 0 \implies \mathbf{t_3 = 0}$$
- Compare: $t_3 = 0 \ne p (7)$.
- **Result:** No match.

---

##### Shift $s = 4$ (Substring $T[4..6] = \text{"124"}$):
- Outgoing character: $T[3] = 3$
- Incoming character: $T[6] = 4$
- Rolling formula:
  $$t_4 = \Big( 10 \cdot (t_3 - T[3] \cdot h) + T[6] \Big) \bmod 13$$
  $$T[3] \cdot h = 3 \times 9 = 27 \equiv 1 \pmod{13}$$
  $$t_3 - 1 = 0 - 1 = -1 \equiv -1 + 13 = 12 \pmod{13}$$
  $$t_4 = (10 \times 12 + 4) \bmod 13 = 124 \bmod 13$$
  $$124 = 13 \times 9 + 7 \implies \mathbf{t_4 = 7}$$
- Compare: $t_4 = 7 == p = 7$! **Hash MATCH!**
- **Character Verification:**
  - $T[4] = '1' == P[0] ('1')$
  - $T[5] = '2' == P[1] ('2')$
  - $T[6] = '4' == P[2] ('4')$
- **Result:** **EXACT MATCH FOUND AT SHIFT $s = 4$!**

---

#### Step 3: Summary Table of Computations

| Shift ($s$) | Text Substring | Numeric Value | Hash Value ($t_s = \text{Val} \bmod 13$) | Hash Match with $p=7$? | Verification Result |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | `413` | 413 | 10 | No | No match |
| **1** | `133` | 133 | 3 | No | No match |
| **2** | `331` | 331 | 6 | No | No match |
| **3** | `312` | 312 | 0 | No | No match |
| **4** | `124` | 124 | **7** | **Yes** | **Exact Valid Match** |

- **Spurious Hits:** **0**
- **Valid Matches:** **1** at shift $s = 4$ (Index 4 to 6).

---

### Instance B: SPPU Nov/Dec 2023 [8 Marks] (Spurious Hit Analysis)

#### Problem Statement
> For string matching with working modulus $q = 11$ and radix $d = 10$, how many spurious hits does the Rabin-Karp matcher encounter in Text $T = \text{"31415926535"}$ with Pattern $P = \text{"26"}$?

---

#### Step 1: Preprocessing & Modulus Hash
- Text: $T = \text{"31415926535"}$, $n = 11$
- Pattern: $P = \text{"26"}$, $m = 2$
- Modulus: $q = 11$, Radix $d = 10$
- Multiplier: $h = 10^{2-1} \bmod 11 = 10 \equiv -1 \pmod{11}$
- Pattern Hash:
  $$p = 26 \bmod 11 = \mathbf{4}$$

---

#### Step 2: Evaluation of All Windows ($0 \le s \le 9$)

| Shift ($s$) | Window $T[s..s+1]$ | Number | Hash ($t_s = \text{Num} \bmod 11$) | Matches $p = 4$? | Character Comparison | Status |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | `31` | 31 | $31 \bmod 11 = 9$ | No | - | No Match |
| **1** | `14` | 14 | $14 \bmod 11 = 3$ | No | - | No Match |
| **2** | `41` | 41 | $41 \bmod 11 = 8$ | No | - | No Match |
| **3** | `15` | 15 | $15 \bmod 11 = \mathbf{4}$ | **Yes** | `'1' \ne '2'` | **SPURIOUS HIT 1** |
| **4** | `59` | 59 | $59 \bmod 11 = \mathbf{4}$ | **Yes** | `'5' \ne '2'` | **SPURIOUS HIT 2** |
| **5** | `92` | 92 | $92 \bmod 11 = \mathbf{4}$ | **Yes** | `'9' \ne '2'` | **SPURIOUS HIT 3** |
| **6** | `26` | 26 | $26 \bmod 11 = \mathbf{4}$ | **Yes** | `'2'=='2', '6'=='6'` | **VALID MATCH** |
| **7** | `65` | 65 | $65 \bmod 11 = 10$ | No | - | No Match |
| **8** | `53` | 53 | $53 \bmod 11 = 9$ | No | - | No Match |
| **9** | `35` | 35 | $35 \bmod 11 = 2$ | No | - | No Match |

---

#### Step 3: Final Answer
- **Number of Spurious Hits:** **3** (at shifts $s = 3$, $s = 4$, and $s = 5$).
- **Valid Match Location:** Exactly **1** match at shift $s = 6$ ($T[6..7] = \text{"26"}$).

---

### Instance C: Naive String Matching Comparison [8 Marks]

#### Problem Statement
> Trace the Naive String Matching algorithm for Text $T = \text{"KITCHEN"}$ and Pattern $P = \text{"HEN"}$.

1. $n = 7, m = 3$. Total shifts: $s \in [0, 4]$.
2. **Trace:**
   - $s = 0$: Compare $T[0] ('K') \ne P[0] ('H') \implies$ Mismatch at 1st char. (1 comparison)
   - $s = 1$: Compare $T[1] ('I') \ne P[0] ('H') \implies$ Mismatch at 1st char. (1 comparison)
   - $s = 2$: Compare $T[2] ('T') \ne P[0] ('H') \implies$ Mismatch at 1st char. (1 comparison)
   - $s = 3$: Compare $T[3] ('C') \ne P[0] ('H') \implies$ Mismatch at 1st char. (1 comparison)
   - $s = 4$:
     - $T[4] ('H') == P[0] ('H')$ (Match)
     - $T[5] ('E') == P[1] ('E')$ (Match)
     - $T[6] ('N') == P[2] ('N')$ (Match)
     - Complete Match! (3 comparisons)
3. **Total Character Comparisons:** $1 + 1 + 1 + 1 + 3 = 7$ comparisons.
4. **Result:** Match found at shift $s = 4$ (Index 4).



<div style="page-break-after: always;"></div>

---

# Topic 13: Distributed Breadth-First Search (Distributed BFS)

This document provides a complete, step-by-step mathematical and network message-passing trace for the **Distributed Breadth-First Search (BFS)** numerical problem appearing in the SPPU DAA Endsem Examination (specifically May/June 2024, 10 Marks).

---

## 1. Algorithmic Foundation & Theory

### 1.1 Distributed Computing Model
In a distributed asynchronous or synchronous network:
- Each node is an autonomous processor with its own local memory and unique identifier (UID).
- There is no global clock or shared memory; processors coordinate exclusively by **passing messages** along network edges.
- Each node knows only its immediate neighbors (local adjacency list).

### 1.2 Objective of Distributed BFS
Given a designated root node $r$, construct a **Breadth-First Search Spanning Tree** rooted at $r$ such that:
1. Every node $u$ knows its parent in the tree: $\text{parent}(u)$.
2. Every node $u$ knows its children: $\text{children}(u)$.
3. Every node $u$ knows its shortest hop distance from the root: $\text{level}(u) = \text{dist}(r, u)$.

### 1.3 Message Types & Protocol
The algorithm operates via wave-based message exchanges:
1. `EXPLORE(level)`: Sent by a node at `level - 1` to propose tree edges to its neighbors.
2. `ACCEPT / PARENT`: Sent by an unvisited node to accept the sender as its parent in the BFS tree.
3. `REJECT / ALREADY_VISITED`: Sent by a node that has already joined the BFS tree at this or an earlier level.
4. `COMPLETED / ECHO`: Propagated from leaf nodes upwards to parents to signal completion of level exploration.

### 1.4 Complexity Analysis
- **Message Complexity:** $O(|E|)$ messages in a synchronized wave model, where each edge transmits a constant number of control messages.
- **Time Complexity:** $O(D)$ synchronous rounds, where $D$ is the network diameter (maximum hop distance from root).

---

## 2. SPPU May/June 2024 PYQ Numerical [10 Marks]

### Problem Statement
> Given a distributed network of 7 nodes $V = \{A, B, C, D, E, F, G\}$ represented by the following adjacency matrix:
>
> | Node | $A$ | $B$ | $C$ | $D$ | $E$ | $F$ | $G$ |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **$A$** | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
> | **$B$** | 1 | 0 | 0 | 1 | 1 | 0 | 0 |
> | **$C$** | 1 | 0 | 0 | 0 | 0 | 1 | 1 |
> | **$D$** | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
> | **$E$** | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
> | **$F$** | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
> | **$G$** | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
>
> Show the stepwise process of how the Distributed Breadth-First Search algorithm works on this graph starting with root node $A$. State the messages exchanged in each wave, the resulting BFS levels, tree edges, and the final spanning tree.

---

### Step 1: Initial Local State Configuration

From the adjacency matrix, each node's local neighbor set is:
- $\text{Neighbors}(A) = \{B, C\}$
- $\text{Neighbors}(B) = \{A, D, E\}$
- $\text{Neighbors}(C) = \{A, F, G\}$
- $\text{Neighbors}(D) = \{B\}$
- $\text{Neighbors}(E) = \{B\}$
- $\text{Neighbors}(F) = \{C\}$
- $\text{Neighbors}(G) = \{C\}$

Initialize all nodes:
- $\forall u \ne A$: $\text{visited}(u) = \text{false}$, $\text{level}(u) = \infty$, $\text{parent}(u) = \text{NULL}$, $\text{children}(u) = \emptyset$.
- Root $A$: $\text{visited}(A) = \text{true}$, $\text{level}(A) = 0$, $\text{parent}(A) = \text{self}$.

---

### Step 2: Stepwise Wave-by-Wave Execution

#### Wave 1: Level 0 $\longrightarrow$ Level 1 Exploration

1. **Root Action:**
   - Root $A$ initiates the BFS at Round 0.
   - Node $A$ sends `EXPLORE(level = 1)` to all its neighbors: $B$ and $C$.
     - Message $M_1$: $A \xrightarrow{\text{EXPLORE}(1)} B$
     - Message $M_2$: $A \xrightarrow{\text{EXPLORE}(1)} C$

2. **Nodes $B$ and $C$ Processing:**
   - **Node $B$:** Receives `EXPLORE(1)` from $A$.
     - Since $\text{visited}(B) = \text{false}$:
       - $\text{visited}(B) \leftarrow \text{true}$
       - $\text{level}(B) \leftarrow 1$
       - $\text{parent}(B) \leftarrow A$
       - Reply: $B \xrightarrow{\text{ACCEPT}} A$
   - **Node $C$:** Receives `EXPLORE(1)` from $A$.
     - Since $\text{visited}(C) = \text{false}$:
       - $\text{visited}(C) \leftarrow \text{true}$
       - $\text{level}(C) \leftarrow 1$
       - $\text{parent}(C) \leftarrow A$
       - Reply: $C \xrightarrow{\text{ACCEPT}} A$

3. **Root $A$ Acknowledgment:**
   - $A$ receives `ACCEPT` from $B \implies \text{children}(A) \leftarrow \{B\}$. Edge $(A, B)$ is marked as a **Tree Edge**.
   - $A$ receives `ACCEPT` from $C \implies \text{children}(A) \leftarrow \{B, C\}$. Edge $(A, C)$ is marked as a **Tree Edge**.
   - Level 1 frontier discovery is complete.

---

#### Wave 2: Level 1 $\longrightarrow$ Level 2 Exploration

1. **Level 1 Nodes Action:**
   Nodes $B$ and $C$ (at Level 1) now send `EXPLORE(level = 2)` to all their unvisited neighbors (excluding parent $A$):
   - **Node $B$:** Sends `EXPLORE(2)` to $D$ and $E$.
     - Message $M_3$: $B \xrightarrow{\text{EXPLORE}(2)} D$
     - Message $M_4$: $B \xrightarrow{\text{EXPLORE}(2)} E$
   - **Node $C$:** Sends `EXPLORE(2)` to $F$ and $G$.
     - Message $M_5$: $C \xrightarrow{\text{EXPLORE}(2)} F$
     - Message $M_6$: $C \xrightarrow{\text{EXPLORE}(2)} G$

2. **Nodes $D, E, F, G$ Processing:**
   - **Node $D$:** Receives `EXPLORE(2)` from $B$. Unvisited $\implies \text{level}(D) = 2, \text{parent}(D) = B$.
     - Reply: $D \xrightarrow{\text{ACCEPT}} B$
   - **Node $E$:** Receives `EXPLORE(2)` from $B$. Unvisited $\implies \text{level}(E) = 2, \text{parent}(E) = B$.
     - Reply: $E \xrightarrow{\text{ACCEPT}} B$
   - **Node $F$:** Receives `EXPLORE(2)` from $C$. Unvisited $\implies \text{level}(F) = 2, \text{parent}(F) = C$.
     - Reply: $F \xrightarrow{\text{ACCEPT}} C$
   - **Node $G$:** Receives `EXPLORE(2)` from $C$. Unvisited $\implies \text{level}(G) = 2, \text{parent}(G) = C$.
     - Reply: $G \xrightarrow{\text{ACCEPT}} C$

3. **Level 1 Parents Acknowledgment:**
   - Node $B$ adds $D$ and $E$ to $\text{children}(B)$: Tree edges $(B, D)$ and $(B, E)$.
   - Node $C$ adds $F$ and $G$ to $\text{children}(C)$: Tree edges $(C, F)$ and $(C, G)$.
   - Level 2 frontier discovery is complete.

---

#### Wave 3: Termination & Echo Phase (Convergecast)

1. Nodes $D, E, F, G$ have no other unvisited neighbors ($\text{degree} = 1$).
2. They are identified as **Leaf Nodes** of the BFS tree.
3. Leaf nodes send `ECHO / COMPLETED` messages upwards:
   - $D \xrightarrow{\text{ECHO}} B$
   - $E \xrightarrow{\text{ECHO}} B$
   - $F \xrightarrow{\text{ECHO}} C$
   - $G \xrightarrow{\text{ECHO}} C$
4. Node $B$ and Node $C$ aggregate all child echoes and send their completion echoes to root $A$:
   - $B \xrightarrow{\text{ECHO}} A$
   - $C \xrightarrow{\text{ECHO}} A$
5. Root $A$ receives echoes from all its children. The distributed BFS algorithm successfully **terminates**.

---

### Step 3: Message Passing Summary Table

| Phase / Round | Sender Node | Receiver Node | Message Sent | Action Taken by Receiver |
| :---: | :---: | :---: | :---: | :--- |
| **Round 1** | $A$ | $B$ | `EXPLORE(1)` | $B$ sets $\text{parent}=A, \text{level}=1$ |
| **Round 1** | $A$ | $C$ | `EXPLORE(1)` | $C$ sets $\text{parent}=A, \text{level}=1$ |
| **Round 1** | $B$ | $A$ | `ACCEPT` | $A$ adds $(A, B)$ to tree edges |
| **Round 1** | $C$ | $A$ | `ACCEPT` | $A$ adds $(A, C)$ to tree edges |
| **Round 2** | $B$ | $D$ | `EXPLORE(2)` | $D$ sets $\text{parent}=B, \text{level}=2$ |
| **Round 2** | $B$ | $E$ | `EXPLORE(2)` | $E$ sets $\text{parent}=B, \text{level}=2$ |
| **Round 2** | $C$ | $F$ | `EXPLORE(2)` | $F$ sets $\text{parent}=C, \text{level}=2$ |
| **Round 2** | $C$ | $G$ | `EXPLORE(2)` | $G$ sets $\text{parent}=C, \text{level}=2$ |
| **Round 2** | $D, E$ | $B$ | `ACCEPT` | $B$ adds $(B, D), (B, E)$ to tree edges |
| **Round 2** | $F, G$ | $C$ | `ACCEPT` | $C$ adds $(C, F), (C, G)$ to tree edges |
| **Round 3** | $D, E$ | $B$ | `ECHO` | Leaves report wave completion to $B$ |
| **Round 3** | $F, G$ | $C$ | `ECHO` | Leaves report wave completion to $C$ |
| **Round 3** | $B, C$ | $A$ | `ECHO` | $B, C$ report full tree completion to Root $A$ |

---

### Step 4: Final Distributed BFS Spanning Tree

```
                     [ Node A ]  (Level 0 - Root)
                     /        \
                    /          \
                   v            v
             [ Node B ]      [ Node C ]  (Level 1)
             /        \      /        \
            v          v    v          v
          [ D ]      [ E ] [ F ]      [ G ]  (Level 2 - Leaves)
```

#### Final Node State Parameters:
| Node | Level ($\text{dist}$) | Parent | Children |
| :---: | :---: | :---: | :---: |
| **$A$** | 0 | Self | $\{B, C\}$ |
| **$B$** | 1 | $A$ | $\{D, E\}$ |
| **$C$** | 1 | $A$ | $\{F, G\}$ |
| **$D$** | 2 | $B$ | $\emptyset$ |
| **$E$** | 2 | $B$ | $\emptyset$ |
| **$F$** | 2 | $C$ | $\emptyset$ |
| **$G$** | 2 | $C$ | $\emptyset$ |

- **Total Tree Edges:** $6$ edges: $\{(A, B), (A, C), (B, D), (B, E), (C, F), (C, G)\}$
- **Cross / Back Edges:** None (graph is a tree network).
- **Total Rounds to Complete:** 3 rounds.
- **Total Messages Exchanged:** 18 messages.
