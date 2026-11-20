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
