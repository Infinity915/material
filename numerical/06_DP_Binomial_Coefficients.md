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
