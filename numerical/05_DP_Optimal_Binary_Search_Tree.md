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
