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
