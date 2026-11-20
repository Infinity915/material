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
