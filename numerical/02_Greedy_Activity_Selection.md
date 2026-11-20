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
