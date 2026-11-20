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
