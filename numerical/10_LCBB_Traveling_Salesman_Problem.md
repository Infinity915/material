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
