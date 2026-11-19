# SPPU B.E. Computer Engineering (2019 Pattern)
## Design and Analysis of Algorithms — Unit 3: Amortized Analysis & Lower Bound Theory
### Complete Examination Solutions Guide (Engineered for Full 8/8 Marks)

---

## Q1. What is amortized analysis? Explain the aggregate method, accounting method, and potential function method used for amortized analysis. (Frequency: 7) [8 Marks]

### Introduction & Core Intuition

**Amortized Analysis** is an advanced **algorithm** analysis technique used to evaluate the average **performance** of each operation in a sequence of operations. Unlike standard average-**case** analysis, amortized analysis involves no probability or assumptions about input **data** distributions; it guarantees the average cost of an operation in the <i>worst-case sequence</i>.

### **Core Motivation and Difference from Average-Case**

In many sophisticated **data** structures (such as dynamic resizable arrays, splay **trees**, or Fibonacci heaps), an occasional operation may be extremely expensive, while the vast majority of operations are very cheap. Traditional worst-**case** analysis provides an overly pessimistic upper **bound** by assuming that every operation costs the maximum possible amount. **Amortized analysis** averages the total cost.

### **1. The Aggregate Method**

**The Aggregate Method** determines the total upper-**bound** cost T(n) for a **sequence** of n **operations** in the worst **case**. The **amortized** cost per operation is then computed directly as the simple mathematical average: Amortized_Cost = T(n) / n. In the aggregate method, every operation in the sequence is assigned the exact same amortized cost, regardless of.

### **2. The Accounting Method (Banker's Method)**

The **Accounting Method** assigns an **artificial** cost, **termed** the **Amortized Cost** c_hat_i, to each **individual** **operation** type. Operations that execute quickly and cheaply (such as inserting into an array with free capacity) are deliberately <i>overcharged</i>: c_hat_i > c_i. The excess amount (c_hat_i - c_i) is stored as credit (or bank savings) associated with specific objects in.

### **3. The Potential Function Method (Physicist's Method)**

**The Potential Function Method** formalizes **credit** as **potential** energy stored in the overall **state** of the **data** structure. A mathematical **function** Phi maps the data structure state D_i after operation i to a real number Phi(D_i). The amortized cost of operation i is defined as: c_hat_i = c_i + Phi(D_i) - Phi(D_i-1), representing the actual cost.

### Architectural Flow & System Diagram

![Figure 3.1: What is amortized analysis?  the aggregate method, accounting method, and potential function method used for amortized analysis](diagrams/u3_q1.png)

*Figure 3.1: Architectural Flow and Conceptual System Diagram for What is amortized analysis?  the aggregate method, accounting method, and potential function method used for amortized analysis*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Formulations of Amortized Analysis Methods:**

```text
Aggregate Amortized Cost: c_hat = T(n) / n   where T(n) = ∑_(i=1)^n c_i
Accounting Credit Invariant: Total Credit = ∑_(i=1)^k ( c_hat_i - c_i ) ≥ 0   for all k
Potential Function Formula: c_hat_i = c_i + Φ(D_i) - Φ(D_(i-1))
Telescoping Potential Sum: ∑_(i=1)^n c_hat_i = ∑_(i=1)^n c_i + Φ(D_n) - Φ(D_0) ≥ ∑_(i=1)^n c_i
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Dynamic Array Resizing Example** (Doubling **array** when full): • **Sequence** of n PUSH **operations** **starting** with array capacity 1. • **Table** copies occur at insertions 1, 2, 4, 8, 16, ..., 2^k. • Total actual cost: T(n) = n (regular insertions) + (1 + 2 + 4 + ... + n) (reallocations) < n + 2n = 3n. • Aggregate Method: Amortized Cost = T(n) / n < 3n / n = 3 = O(1).

### Key Comparative Evaluation

| Analysis Method | Measurement Mechanism | Credit Representation | Flexibility Across Operation Types |
| :--- | :--- | :--- | :--- |
| Aggregate Method | Direct division: T(n) / n | No explicit credit tracking | Rigid: all operations receive identical average cost |
| Accounting Method | Artificial charging c_hat_i | Stored as prepaid credit on specific elements | Flexible: different operations have distinct amortized rates |
| Potential Method | State potential difference: ΔΦ | Stored as global potential energy Φ(D) in state | Most rigorous: clean mathematical proofs via telescoping sums |

### Concluding Exam Takeaway

**Amortized analysis** guarantees that the **average** cost of an **operation** **across** any worst-**case** sequence is bounded, proving that **data** structures with occasional spikes (like dynamic arrays) operate in true O(1) amortized time.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 14: Amortized Analysis, Sections 14.1–14.4, Pages 525–534.

---

## Q2. Find the amortized cost of PUSH, POP, and MULTIPOP stack operations using the Aggregate or Potential function method. (Frequency: 4) [8 Marks]

### Introduction & Core Intuition

In **standard** **stack** **implementations**, the **operations** PUSH and POP take O(1) **worst-case time**. However, augmenting the stack with a MULTIPOP(S, k) operation—which pops the top min(k, |S|) elements—can take up to O(n) time in a single invocation. **Amortized analysis** proves that across any sequence of n operations, the amortized cost of every operation (including MULTIPOP) is strictly O(1).

### **Operation Definitions and Worst-Case Analysis**

**Consider** an **initially** **empty** **stack** S. We **support** **three** operations: (1) PUSH(S, x) pushes element x onto the stack, costing 1 unit of time; (2) POP(S) removes the top element, costing 1 unit of time; and (3) MULTIPOP(S, k) executes while not StackEmpty(S) and.

### **Aggregate Method Analysis**

To **apply** the **Aggregate** **method**, **observe** that each **element** can be **popped** at most once for each time it is pushed onto the stack. In any sequence of n operations starting from an empty stack, the maximum number of PUSH operations is at most.

### **Potential Function Method Analysis**

We **define** the potential **function** Phi as the **number** of **elements** **currently** **stored** in the stack: Phi(S) = |S|. Initially, the stack is empty, so Phi(S_0) = 0. Because the number of elements in a stack is.

### **Derivation of Amortized Costs via Potential Method**

For PUSH(S, x): **Actual** cost c_i = 1. **Stack** size **increases** by 1, so Phi(S_i) - Phi(S_i-1) = (|S| + 1) - |S| = 1. The **amortized** cost is: c_hat = c_i + Delta_Phi = 1 + 1 = 2 = O(1).<br/>For POP(S): **Actual**.

### Architectural Flow & System Diagram

![Figure 3.2: Find the amortized cost of PUSH, POP, and MULTIPOP stack operations using the Aggregate or Potential function method](diagrams/u3_q2.png)

*Figure 3.2: Architectural Flow and Conceptual System Diagram for Find the amortized cost of PUSH, POP, and MULTIPOP stack operations using the Aggregate or Potential function method*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Derivation of Amortized Stack Operations:**

```text
Stack Potential: Φ(S) = |S|   [Number of elements in stack]
PUSH Amortized Cost: c_hat_push = 1 + ( |S| + 1 - |S| ) = 1 + 1 = 2
POP Amortized Cost: c_hat_pop = 1 + ( |S| - 1 - |S| ) = 1 - 1 = 0
MULTIPOP Amortized Cost: c_hat_multipop = k' + ( |S| - k' - |S| ) = k' - k' = 0
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Sequence Trace** of 6 **Operations** **starting** from **empty** **stack**: • Op 1: PUSH(A) -> Cost = 1, **Stack** = [A] (|S|=1). Amortized = 2. • Op 2: PUSH(B) -> Cost = 1, Stack = [A, B] (|S|=2). Amortized = 2. • Op 3: PUSH(C) -> Cost = 1, Stack = [A, B, C] (|S|=3). Amortized = 2. • Op 4: MULTIPOP(2) -> Cost = 2, Stack = [A] (|S|=1). Amortized = 0. • Op 5:.

### Key Comparative Evaluation

| Stack Operation | Actual Cost (c_i) | Change in Potential (ΔΦ) | Amortized Cost (c_hat_i) | Asymptotic Class |
| :--- | :--- | :--- | :--- | :--- |
| PUSH(S, x) | 1 | +1 (stack grows by 1) | 1 + 1 = 2 | O(1) Amortized |
| POP(S) | 1 | -1 (stack shrinks by 1) | 1 + (-1) = 0 | O(1) Amortized |
| MULTIPOP(S, k) | k' = min(k, |S|) | -k' (stack shrinks by k') | k' + (-k') = 0 | O(1) Amortized |

### Concluding Exam Takeaway

**Using** **either** **Aggregate** or **Potential** methods, the occasional O(n) MULTIPOP operation is completely absorbed by the prepaid potential of preceding PUSH operations, establishing an O(1) amortized **bound**. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 14: Amortized Analysis, Section 14.2: Stack Operations, Pages 526–529.

---

## Q3. Give an amortized analysis of a k-bit binary counter using the aggregate method and prove that the amortized cost per operation is O(1). (Frequency: 2) [8 Marks]

### Introduction & Core Intuition

A k-bit **Binary Counter** stores non-**negative** **integers** as an **array** of k bits A[0..k-1], where A[0] is the lowest-order bit. Incrementing the counter by 1 in the worst **case** (such as incrementing 1111 to 10000) causes all k bits to flip, taking O(k) time. Amortized **aggregate analysis** proves that across a sequence of n successive increments starting from 0, the.

### **Increment Algorithmic Working Mechanism**

The **standard** **binary** **counter** increment **algorithm** begins at bit **index** i = 0. While i < k and A[i] == 1, it **flips** A[i] = 0 (handling the arithmetic carry) and increments i = i + 1. If i < k, it sets A[i] =.

### **Pessimistic Upper Bound vs Aggregate Reality**

A naive worst-**case** analysis would **bound** the cost of n **increments** as: **Total Time** = n * O(k) = O(n * k). However, this **bound** is **excessively** loose because not every increment flips all k bits. In fact, higher-order bits flip much less frequently than.

### **Rigorous Mathematical Bit-Flip Enumeration**

In a **sequence** of n **INCREMENT** **operations** **starting** from zero:<br/>• Bit A[0] **flips** on every **increment**: exactly n times (n / 1 = n / 2^0).<br/>• Bit A[1] flips every 2 increments: exactly floor(n / 2) = floor(n / 2^1) times.<br/>• Bit A[2] flips every.

### **Summation Over All Bits**

The **total** **number** of bit **flips** **across** all k **positions** for the **entire** sequence of n increments is: Total_Flips = sum( floor(n / 2^i) for i = 0 to k-1 ) < sum( n / 2^i for i = 0 to infinity ) =.

### Architectural Flow & System Diagram

![Figure 3.3: Give an amortized analysis of a k-bit binary counter using the aggregate method and prove that the amortized cost per operation is O(1)](diagrams/u3_q3.png)

*Figure 3.3: Architectural Flow and Conceptual System Diagram for Give an amortized analysis of a k-bit binary counter using the aggregate method and prove that the amortized cost per operation is O(1)*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Proof of Binary Counter Aggregate Cost:**

```text
Total Bit Flips T(n) = ∑_(i=0)^(k-1) ⌊ n / 2^i ⌋ < n · ∑_(i=0)^∞ ( 1 / 2 )^i
Infinite Geometric Sum: ∑_(i=0)^∞ ( 1 / 2 )^i = 1 / ( 1 - 1/2 ) = 2
Total Work Bound: T(n) < 2 · n
Amortized Cost per Operation: c_hat = T(n) / n < ( 2 · n ) / n = 2 = O(1)   [Q.E.D.]
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Bit Flip Table** for First 8 **Increments** (k = **4 bits**): • Inc 1: 0000 -> 0001 (1 flip) • Inc 2: 0001 -> 0010 (2 **flips**: bits 0, 1) • Inc 3: 0010 -> 0011 (1 flip) • Inc 4: 0011 -> 0100 (3 **flips**: bits 0, 1, 2) • Inc 5: 0100 -> 0101 (1 flip) • Inc 6: 0101 -> 0110 (2 **flips**: bits 0, 1) • Inc 7: 0110 -> 0111.

### Key Comparative Evaluation

| Bit Position (i) | Frequency of Flips | Flips in n = 16 Operations | Flips in General n Operations | Contribution to Total Work |
| :--- | :--- | :--- | :--- | :--- |
| Bit 0 (LSB) | Every increment | 16 | n | n |
| Bit 1 | Every 2nd increment | 8 | n / 2 | 0.50 n |
| Bit 2 | Every 4th increment | 4 | n / 4 | 0.25 n |
| Bit 3 | Every 8th increment | 2 | n / 8 | 0.125 n |

### Concluding Exam Takeaway

Through **geometric** **series** **summation**, the **total** **number** of bit **flips** in n successive increments is proven to be strictly bounded by 2n, establishing that the amortized cost per increment is O(1).

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 14: Amortized Analysis, Section 14.3: Binary Counter, Pages 529–531.

---

## Q4. Why can the potential function method not be used for analyzing a binary counter? (Frequency: 1) [8 Marks]

### Introduction & Core Intuition

The **premise** **posed** in the question—that the potential **function** method <i>cannot</i> be used for analyzing a binary counter—is technically a misconception in classical **algorithm** theory. **The Potential Function Method** CAN indeed be used effectively to analyze a binary counter, provided the counter starts at zero or initial non-zero potential is accounted for. The question highlights the specific failure **condition** that.

### **Standard Successful Potential Formulation for Binary Counter**

In **standard** **Cormen** (CLRS) and Horowitz-Sahni **algorithm** literature, the potential **function** method is **successfully** **applied** to a binary counter by defining the potential Phi(D_i) as the number of 1s in the counter after operation i: Phi(D_i) = b_i. Suppose increment i resets t bits from 1 to 0 and sets 1 bit from 0 to 1..

### **The Condition Under Which the Potential Method Breaks Down**

The **potential method** guarantees that the sum of **amortized** **costs** **bounds** the sum of **actual** costs: sum(c_i) <= sum(c_hat_i) - (Phi(D_n) - Phi(D_0)). For this bound to hold unconditionally, the foundational invariant is: Phi(D_n) >= Phi(D_0). If the counter starts at zero, Phi(D_0) = 0 and since b_i >= 0, the **condition** holds. However, if the.

### **Consequence of Negative Potential Difference (Phi(D_n) < Phi(D_0))**

If the **counter** **starts** with all 1s (D_0 = 111...1, so b_0 = k) and a **single** **increment** **flips** all bits to 0 and adds a **carry** (D_1 = 000...0), the change in potential is Phi(D_1) - Phi(D_0) = 1 - k, which is deeply negative. The sum of actual costs becomes: sum(c_i) = sum(c_hat_i) +.

### **Decrementation and Non-Monotonic Potential Failures**

**Furthermore**, if the **data** structure **supports** both **INCREMENT** and **DECREMENT** operations, a simple count of 1s fails completely. An adversarial sequence can oscillate between 10000 and 01111, incurring k bit flips on every single operation. Here, the amortized cost per operation degenerates to O(k), and the potential **function** Phi = b_i fails to absorb the oscillating costs.

### Architectural Flow & System Diagram

![Figure 3.4: Why can the potential function method not be used](diagrams/u3_q4.png)

*Figure 3.4: Architectural Flow and Conceptual System Diagram for Why can the potential function method not be used*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Invariant and Initial State Offset:**

```text
Telescoping Sum Identity: ∑_(i=1)^n c_i = ∑_(i=1)^n c_hat_i - ( Φ(D_n) - Φ(D_0) )
Valid Amortized Bound Condition: Φ(D_n) ≥ Φ(D_0)   for all n
Failure when D_0 has non-zero bits: If b_0 = k and b_n = 1, then:
    ∑ c_i = 2n + ( b_0 - b_n ) = 2n + k - 1 > ∑ c_hat_i
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Failure Scenario**: Counter **initialized** to 1111 (b_0 = 4) with n = 1 **increment**: • **State** 0: 1111 (b_0 = 4 1-bits). **State** 1: 10000 (b_1 = 1 1-bit). • Actual Cost c_1: **4 bits** flipped from 1 to 0 + 1 bit flipped to 1 = 5 bit flips. • Potential Formula: c_hat_1 = c_1 + Φ(D_1) - Φ(D_0) = 5 + (1 - 4) = 5 - 3 = 2. • Analysis: The.

### Key Comparative Evaluation

| Counter Setup Condition | Initial Potential Φ(D_0) | Final Potential Φ(D_n) | Does Amortized Bound Hold? | Status of Potential Method |
| :--- | :--- | :--- | :--- | :--- |
| Starts at Zero (D_0 = 0000) | Φ(D_0) = 0 | Φ(D_n) >= 0 | YES: ∑ c_i <= ∑ c_hat_i | 100% VALID (Amortized = O(1)) |
| Starts at Arbitrary State (b_0 > 0) | Φ(D_0) = b_0 | Can be < b_0 | NO (without offset b_0) | REQUIRES INITIAL BIAS CORRECTION |
| Supports Both Inc and Dec | Variable | Oscillating | NO: Degenerates to O(k) | FAILS ON ADVERSARIAL OSCILLATION |

### Concluding Exam Takeaway

The **potential method** is **valid** when the **counter** **starts** at zero, but fails to provide a pure upper **bound** when the counter begins at an arbitrary non-zero **state** unless the initial potential offset b_0 is added, or when alternating decrements defeat amortized credits.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 14: Amortized Analysis, Section 14.3: Binary Counter Analysis, Pages 530–533.

---

## Q5. What is a randomized algorithm? Give examples, enlist reasons to use them, and explain random variables and binomial random variables. (Frequency: 6) [8 Marks]

### Introduction & Core Intuition

A **Randomized Algorithm** is an **algorithm** that **incorporates** a **degree** of randomness into its logic, typically employing a pseudo-random number generator to influence choices during **execution** (such as picking a pivot or selecting a sample). Unlike **deterministic** algorithms that produce identical execution traces for identical inputs, a randomized algorithm's behavior and execution time vary stochastically across runs.

### **Two Fundamental Classes: Las Vegas vs Monte Carlo**

Randomized **algorithms** are **bifurcated** into two foundational **classes**: (1) **Las Vegas Algorithms**: These **algorithms** **ALWAYS** return the mathematically correct and exact output; only their running time is a random variable (e.g., Randomized QuickSort, which always produces a perfectly sorted array,.

### **Primary Reasons for Using Randomized Algorithms**

Randomized **algorithms** are **widely** **adopted** across computer science for five compelling reasons: (1) **Defeating Worst**-**Case** Inputs: **Deterministic** algorithms often possess malicious inputs (e.g., an already sorted array degrading deterministic QuickSort to O(n^2)); random pivot selection neutralizes adversary inputs. (2) Simplicity.

### **Mathematical Foundation: Random Variables**

A **Random Variable** X is a mathematical **function** that maps **outcomes** of a random **process** in a **sample** **space** S to real numbers: X: S -> R. An Indicator Random Variable I{A} takes value 1 if event A occurs and.

### **Binomial Random Variables in Algorithm Analysis**

A **Binomial Random Variable** X ~ Bin(n, p) **models** the **total** **number** of **successes** in n independent Bernoulli trials, each having success probability p and failure probability 1 - p. Its Probability Mass **Function** is: P(X = k) = C(n,.

### Architectural Flow & System Diagram

![Figure 3.5: What is a randomized algorithm? Give examples, enlist reasons to use them, and explain random variables and binomial random variables](diagrams/u3_q5.png)

*Figure 3.5: Architectural Flow and Conceptual System Diagram for What is a randomized algorithm? Give examples, enlist reasons to use them, and explain random variables and binomial random variables*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Formulations of Random Variables:**

```text
Expectation: E[X] = ∑_(x) x · P(X = x)
Linearity of Expectation: E[ ∑_(i=1)^n X_i ] = ∑_(i=1)^n E[X_i]   [Holds even for dependent variables!]
Binomial PMF: P(X = k) = ( n choose k ) · p^k · ( 1 - p )^(n-k)
Binomial Parameters: Mean E[X] = n · p,   Variance Var(X) = n · p · (1 - p)
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Coin Toss Analysis** for **Randomized** Skip List: • In a Skip List, **node** tower **height** is **determined** by tossing a fair coin (p = 0.5) until tails appears. • Let X be number of coin tosses. Expected height E[X] = 1 / p = 1 / 0.5 = 2. • Probability of height exceeding k is (1/2)^k. For n elements, maximum height is bounded by O(log n) with high probability. Provides full binary **tree** search capabilities without complex balancing rotations.

### Key Comparative Evaluation

| Randomized Paradigm | Output Correctness | Execution Runtime | Error Reduction Technique | Prominent Example |
| :--- | :--- | :--- | :--- | :--- |
| Las Vegas | ALWAYS 100% correct | Random Variable (Expected time polynomial) | Re-run until execution completes | Randomized QuickSort / QuickSelect |
| Monte Carlo (One-Sided) | May err in one direction (e.g., false prime) | Deterministic polynomial time | Repeat test k times (Error <= (1/4)^k) | Miller-Rabin Primality Test |
| Monte Carlo (Two-Sided) | May err in both directions | Deterministic polynomial time | Majority voting over repeated runs | Randomized Max-Cut Approximation |

### Concluding Exam Takeaway

Randomized **algorithms** harness **stochastic** **choices** to break symmetry, neutralize worst-**case** input configurations, and provide fast, simple solutions across sorting, cryptography, and distributed **systems**. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 13: Randomized Algorithms, Sections 13.1–13.4, Pages 485–496.

---

## Q6. Does the randomized algorithm for quick sort improve the average case time complexity? Discuss. (Frequency: 2) [8 Marks]

### Introduction & Core Intuition

In **answering** **whether** Randomized QuickSort improves the **average-case time** complexity, the precise theoretical answer is: NO, it does not improve the asymptotic average-case **time complexity**, because **deterministic** QuickSort already runs in Theta(n log n) average time. **What Randomized** QuickSort does is far more profound: it eliminates the dependency on input distribution, guaranteeing that the expected running time is O(n log n) for EVERY input instance.

### **Average-Case vs Expected Running Time Distinction**

Standard **Deterministic** QuickSort (e.g., **choosing** the first or last **element** as **pivot**) **achieves** Theta(n log n) **Average-Case Time** only under the strict assumption that all n! permutations of input elements are equally likely. If an adversary deliberately feeds a pre-sorted array, a reverse-sorted array, or an array with identical elements, deterministic QuickSort degenerates catastrophically to its.

### **How Randomized QuickSort Operates**

**Randomized** **QuickSort** **modifies** the **algorithm** by **selecting** the **pivot** element uniformly at random from the current subarray A[p..r]: pivot_index = Random(p, r). It swaps A[pivot_index] with A[r] and proceeds with standard Lomuto or Hoare partitioning. Because the pivot selection is randomized internally within the algorithm, no external input ordering—regardless of whether it is pre-sorted or maliciously.

### **Expected Running Time Derivation via Indicator Variables**

Let z_1, z_2, ..., z_n be the **elements** of **array** A **sorted** in **ascending** **order**. The **total** running time is dominated by the total number of element comparisons X. Two elements z_i and z_j are compared if and only if one of them is chosen as pivot before any intermediate element z_k (for i < k.

### **Worst-Case Scenario in Randomized QuickSort**

**Can Randomized** QuickSort **still** run in O(n^2) time? Yes, **theoretically**, if the **pseudo**-random **generator** **happens** to pick the absolute minimum or maximum element at every recursive level. However, the probability of this occurring on an array of size n is (2/n) * (2/(n-1)) * ... * (2/2) = 2^(n-1) / n!, which is astronomically small (for.

### Architectural Flow & System Diagram

![Figure 3.6: Does the randomized algorithm for quick sort improve the average case time complexity?](diagrams/u3_q6.png)

*Figure 3.6: Architectural Flow and Conceptual System Diagram for Does the randomized algorithm for quick sort improve the average case time complexity?*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Proof of Expected QuickSort Comparisons:**

```text
Probability of Comparing Elements z_i and z_j: P(z_i is compared to z_j) = 2 / ( j - i + 1 )
Total Expected Comparisons: E[X] = ∑_(i=1)^(n-1) ∑_(j=i+1)^n ( 2 / ( j - i + 1 ) )
Harmonic Series Approximation: E[X] ≤ 2 · n · H_n ≈ 2 · n · ln n = 1.386 · n · log₂ n
Expected Running Time: E[T(n)] = Θ(n log n)   [Holds for ALL input distributions!]
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Comparative Behavior** on Pre-**Sorted** **Array** [1, 2, 3, 4, 5, ..., 1000]: • **Deterministic** QuickSort (First **element** pivot): Picks 1 -> Splits into 0 and 999 elements. Picks 2 -> Splits into 0 and 998 elements. Recursion depth = 1000, Total Comparisons = 1000 * 999 / 2 ≈ 500,000 comparisons (O(n^2) worst **case**!). • Randomized QuickSort (Random pivot): Picks random elements near center -> Produces balanced ~500:500 splits. Recursion depth ≈ 2 ln(1000) ≈.

### Key Comparative Evaluation

| QuickSort Variant | Average-Case Time Complexity | Worst-Case Time Complexity | Vulnerability to Pre-Sorted Inputs | Performance Guarantee |
| :--- | :--- | :--- | :--- | :--- |
| Deterministic QuickSort | Θ(n log n) | Θ(n^2) | HIGH (Sorted inputs trigger worst-case O(n^2)) | Conditional on uniform input permutation |
| Randomized QuickSort | Θ(n log n) | Θ(n^2) [probability ≈ 0] | NONE (Neutralizes sorted and reversed inputs) | Guaranteed O(n log n) expected for ANY input |

### Concluding Exam Takeaway

**Randomized** **QuickSort** does not **lower** the average-case **bound** from Theta(n log n), but it eliminates input **vulnerability**, guaranteeing expected O(n log n) **execution** across all inputs with probability approaching 1.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 13: Randomized Algorithms, Section 13.2: Randomized QuickSort, Pages 488–492.

---

## Q7. What are approximation algorithms? Based on the approximation ratio, classify the approximation algorithms. (Frequency: 6) [8 Marks]

### Introduction & Core Intuition

An **Approximation Algorithm** is an efficient polynomial-time **algorithm** designed to solve NP-hard **optimization problems** by finding a provably near-optimal solution rather than searching indefinitely for the exact global optimum. Its quality is formally certified by a mathematical parameter called the **Approximation Ratio** (ρ). This foundational concept enables engineers to **model** complex system dynamics and make **deterministic** decisions under real-world constraints.

### **Definition of Approximation Ratio (ρ)**

Let C be the cost of the **solution** **produced** by the **approximation algorithm**, and let C* be the cost of the true **optimal** **solution** for an instance of size n. The **Approximation Ratio** ρ(n) is defined such that: max( C / C*, C*.

### **1. Constant-Factor Approximation Algorithms**

A Constant-Factor **Approximation Algorithm** achieves an **approximation ratio** ρ that is a **fixed** **constant** **independent** of the input size n. For example: (1) The 2-Approximation for **Vertex Cover** (picking both endpoints of maximal matching edges) guarantees C <= 2 * C*; (2) Christofides'.

### **2. Polynomial-Time Approximation Schemes (PTAS)**

A Polynomial-**Time Approximation Scheme** (PTAS) is an **algorithm** that accepts the problem instance along with a user-specified **error** tolerance **parameter** epsilon > 0. For any fixed epsilon, the PTAS produces a solution with an **approximation ratio** of (1 + epsilon) for minimization (or.

### **3. Fully Polynomial-Time Approximation Schemes (FPTAS)**

A **Fully Polynomial**-Time Approximation Scheme (FPTAS) is the most desirable **class** of approximation **algorithms**. It guarantees a (1 + epsilon) **approximation ratio**, and its running time is strictly polynomial in <i>both</i> the input size n and the reciprocal of the error **parameter** 1/epsilon:.

### Architectural Flow & System Diagram

![Figure 3.7: What are approximation algorithms? Based on the approximation ratio, classify the approximation algorithms](diagrams/u3_q7.png)

*Figure 3.7: Architectural Flow and Conceptual System Diagram for What are approximation algorithms? Based on the approximation ratio, classify the approximation algorithms*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Classification of Approximation Ratios:**

```text
Approximation Ratio: max( C / C*, C* / C ) ≤ ρ(n)
Constant-Factor: ρ(n) = O(1)   [Independent of input size n]
PTAS Runtime: T(n, ε) = O( n^( f(1/ε) ) )   [Polynomial in n, arbitrary in 1/ε]
FPTAS Runtime: T(n, ε) = O( (n / ε)^k )   [Polynomial in BOTH n and 1/ε]
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

FPTAS **Knapsack Example** for n = 100 **Items** with ε = 0.10 (10% **error** margin): • **Exact** DP requires O(n·W) = 100 x 10^9 operations (OOM failure). • FPTAS scaling factor: K = (ε · p_max) / n. • Scaled profits: p'_i = floor(p_i / K). • FPTAS **Runtime**: O(n³ / ε) = (100³ / 0.10) = 10,000,000 operations (Executes in **0.05 seconds**!). Guarantees that solution profit is at least 90% of the true global optimal profit.

### Key Comparative Evaluation

| Approximation Class | Approximation Ratio ρ | Runtime Dependency | Best-Known Problem Example |
| :--- | :--- | :--- | :--- |
| FPTAS | 1 + ε (Min) or 1 - ε (Max) | Polynomial in both n and 1/ε | 0/1 Knapsack Problem: O(n³ / ε) |
| PTAS | 1 + ε (Min) or 1 - ε (Max) | Polynomial in n; exponential in 1/ε | Euclidean TSP: O(n · (log n)^(O(1/ε))) |
| Constant Factor | Fixed constant ρ (e.g., 1.5, 2.0) | Polynomial in n, independent of ε | Vertex Cover (ρ = 2.0), Metric TSP (ρ = 1.5) |
| Logarithmic Factor | O(log n) | Polynomial in n | Set Cover (Greedy heuristic achieves H_n ≈ ln n) |

### Concluding Exam Takeaway

Approximation **algorithms** provide a **mathematically** **sound** **method** for overcoming NP-hardness, classified in a strict hierarchy ranging from flexible FPTAS schemes down to constant-factor **heuristics**. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 12: NP-Completeness & Approximation, Sections 12.5–12.7, Pages 465–474.

---

## Q8. What are the special needs of embedded algorithms? Which sorting algorithm is best for embedded systems (like medical devices processing real-time sensor data)? Explain Embedded system scheduling using a power-optimized algorithm. (Frequency: 3) [8 Marks]

### Introduction & Core Intuition

**An Embedded Algorithm** is **software** **designed** to execute within specialized, resource-constrained embedded **systems** (such as medical pacemakers, automotive brake controllers, or IoT sensor motes). Unlike general-purpose computing where **memory** and CPU power are abundant, embedded environments enforce stringent physical constraints regarding memory footprints, **deterministic** real-time deadlines, and battery power consumption.

### **Special Needs of Embedded Algorithms**

Embedded **algorithms** operate under five **unique** **architectural** constraints: (1) **Strict Memory Limitations**: Microcontrollers possess tiny RAM (often **16 KB** to 256 KB); algorithms must avoid dynamic **memory** allocation (malloc) to prevent.

### **Best Sorting Algorithm for Embedded Medical Devices**

For an **embedded** **medical** **device** (e.g., an ECG monitor processing a **stream** of real-time sensor samples), the mathematically optimal sorting **algorithm** is HeapSort. Let us evaluate the alternatives: QuickSort has worst-**case**.

### **When Insertion Sort is Preferred in Embedded Systems**

For small **buffer** windows (n <= 32) of **continuous** real-time sensor **data** that **arrives** **nearly** sorted, **Insertion Sort** is exceptionally effective. Insertion Sort operates.

### **Embedded System Power-Optimized Scheduling**

**Microprocessor** **power** **consumption** is **governed** by the CMOS **dynamic** **power** equation: P = C * V^2 * f, where C is capacitance, V is supply voltage, and f is clock frequency..

### Architectural Flow & System Diagram

![Figure 3.8: What are the special needs of embedded algorithms? Which sorting algorithm is best for embedded systems (like medical devices processing real-time sensor data)?  Embedded system scheduling using a power-optimized algorithm](diagrams/u3_q8.png)

*Figure 3.8: Architectural Flow and Conceptual System Diagram for What are the special needs of embedded algorithms? Which sorting algorithm is best for embedded systems (like medical devices processing real-time sensor data)?  Embedded system scheduling using a power-optimized algorithm*

### Mathematical Formulation & Algorithmic Mechanics

**Embedded CMOS Power and DVFS Frequency Scaling Formula:**

```text
Dynamic Power Dissipation: P = C · V² · f
Energy Consumption per Task: E = P · t = C · V² · f · ( Cycles / f ) = C · V² · Cycles
DVFS Frequency Selection: f_req = Cycles / ( D_deadline - t_current )
HeapSort WCET Bound: T_worst(n) ≤ 2 · n · log₂ n + O(n)   [Auxiliary Space = O(1)]
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

Real-Time ECG **Sensor Buffer Processing** (n = 64 samples): • **Memory** Budget: Total Microcontroller RAM = **32 KB**. Available Stack **Buffer** = 512 bytes. • Sorting Comparison: - MergeSort: Demands 64 x 4 = 256 bytes extra heap buffer (Fails safety margins). - QuickSort: Worst-**case** recursion depth = 64 stack frames (Threatens **stack overflow**). - HeapSort: In-place array transformation. Auxiliary RAM = 0 bytes! WCET = 64 x 6 x 2 = 768 cycles. •.

### Key Comparative Evaluation

| Sorting Algorithm | Worst-Case Time | Auxiliary Memory | Stability | Suitability for Embedded Medical Devices |
| :--- | :--- | :--- | :--- | :--- |
| HeapSort | Guaranteed O(n log n) | Strictly O(1) in-place | No | BEST (Guaranteed timing + zero extra RAM) |
| Insertion Sort | O(n^2) worst / O(n) best | Strictly O(1) in-place | Yes | EXCELLENT for small sensor windows (n <= 32) |
| MergeSort | Guaranteed O(n log n) | O(n) auxiliary buffer | Yes | POOR (Exhausts scarce microcontroller RAM) |
| QuickSort | O(n^2) worst-case | O(log n) stack memory | No | UNSAFE (Risk of stack overflow on sorted inputs) |

### Concluding Exam Takeaway

Embedded **algorithms** prioritize **deterministic** timing and zero **memory** fragmentation, **making** **HeapSort** the optimal sorting choice, while DVFS-integrated EDF scheduling maximizes battery longevity through quadratic voltage scaling. For optimal SPPU examination scoring, answers must present this complete structure: rigorous theoretical definitions with bold keywords, an accurate architectural flow diagram, clear mathematical formulation, and a structured worked simulation to secure the full 8/**8 marks**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 14: Special Topics & Embedded Algorithms, Sections 14.5–14.7, Pages 535–542.

---

## Q9. What are tractable and non-tractable (intractable) problems? Give examples and discuss if the traveling salesperson problem belongs to intractable problems. (Frequency: 4) [8 Marks]

### Introduction & Core Intuition

In computational complexity theory, problems are fundamentally classified into **Tractable Problems** and Intractable Problems based on whether they can be solved by **algorithms** operating within **polynomial time** bounds as input size scales. This foundational concept enables engineers to **model** complex **system** dynamics and make **deterministic** decisions under real-world constraints.

### **Definition of Tractable Problems (Class P)**

A **problem** is **defined** as **Tractable** if there exists a **deterministic** algorithm that solves it in **Polynomial Time**, denoted as O(n^k) for some constant k independent of input size n. Tractable problems constitute the **complexity** class P. These problems are considered practically solvable on modern computers even for large values of n. Examples include Sorting (O(n.

### **Definition of Intractable (Non-Tractable) Problems**

A **problem** is **defined** as Intractable if it cannot be solved in **polynomial time**; any **algorithm** designed to solve it to exact global optimality requires Super-Polynomial or **Exponential Time**, such as O(2^n), O(n!), or O(n^n). As the input size n grows beyond modest values (e.g., n > 40), the number of required **execution** steps exceeds the.

### **Analysis of the Traveling Salesperson Problem (TSP)**

**The Traveling Salesperson Problem** (TSP) asks for the minimum-cost **Hamiltonian cycle** visiting every city exactly once. TSP is a quintessential Intractable Problem. In its **optimization** form, TSP is **NP-Hard**; in its decision form ('Does a tour of cost <= k exist?'), it is **NP-Complete**. To date, no polynomial-time algorithm is known for TSP, and if P.

### **Why TSP is Provably Intractable**

For a complete **graph** of n cities, the number of distinct candidate tours is (n - 1)! / 2. A brute-force **search** on n = 25 cities requires examining approximately 3.1 x 10^23 tours, which would take thousands of years on modern supercomputers. **Dynamic programming** (the Held-Karp **algorithm**) improves the **runtime** to O(n^2 * 2^n), which.

### Architectural Flow & System Diagram

![Figure 3.9: What are tractable and non-tractable (intractable) problems? Give examples and discuss if the traveling salesperson problem belongs to intractable problems](diagrams/u3_q9.png)

*Figure 3.9: Architectural Flow and Conceptual System Diagram for What are tractable and non-tractable (intractable) problems? Give examples and discuss if the traveling salesperson problem belongs to intractable problems*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Complexity Bounds: Tractable vs Intractable:**

```text
Tractable Criterion (Class P): T(n) = O( n^k )   [k is a fixed constant]
Intractable Criterion: T(n) = Ω( 2^(n^c) )   or   T(n) = Ω( n! )
TSP Brute-Force Permutations: Total Tours = ( n - 1 )! / 2
Held-Karp Dynamic Programming: T_tsp(n) = O( n² · 2^n )   [Still strictly exponential!]
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Computational Scale Comparison** for n = 30 **Items** / **Cities**: • **Tractable** **Problem**: **Sorting** array of size n = 30 via MergeSort: Operations = 30 · log₂(30) ≈ 30 · 5 = 150 operations (Executes in < 0.0001 milliseconds!). • Intractable Problem: TSP on n = 30 cities via brute force: Tours = 29! / 2 ≈ 4.42 x 10^30 tours. At 1 billion tours evaluated per second, it requires 1.4 x 10^14 years (10,000.

### Key Comparative Evaluation

| Complexity Category | Mathematical Definition | Growth Rate of Runtime | Solvability on Large n | Representative Examples |
| :--- | :--- | :--- | :--- | :--- |
| Tractable (Class P) | Solvable in deterministic polynomial time O(n^k) | Slow polynomial growth (O(n), O(n^2), O(n^3)) | Solvable up to millions of elements | Sorting, MST, Shortest Paths, Matrix Mult |
| Intractable (NP-Hard / NP-Complete) | Requires super-polynomial time O(2^n) or O(n!) | Explosive exponential growth | Unsolvable exactly for n > 50 | TSP, 0/1 Knapsack, Clique, Graph Coloring |
| Provably Non-computable | Undecidable; no algorithm can exist | Infinite | Never solvable by any computer | Turing Halting Problem |

### Concluding Exam Takeaway

Tractable problems belong to **class** P and scale comfortably, whereas the **Traveling Salesperson Problem** is intractable (**NP-hard**) with an exponential O(n^2·2^n) **bound**, necessitating approximation **heuristics** in practical applications.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 12: NP-Completeness, Sections 12.1–12.3, Pages 455–462.

---

## Q10. Comment on the following statements regarding NP classes: 'The knapsack problem is NP-hard', 'Boolean Satisfiability Problem (SAT) is NP-complete', 'Minimum spanning tree is tractable problem', and 'The problem of Searching an ordered/unordered list belongs to the class of tractable problems'. (Frequency: 2) [8 Marks]

### Introduction & Core Intuition

This **question** **assesses** the core foundational classifications of computational **complexity** theory. All four statements are MATHEMATICALLY ACCURATE AND TRUE. Below is the rigorous examination justification and formal complexity mapping for each statement. This foundational concept enables engineers to **model** complex **system** dynamics and make **deterministic** decisions under real-world constraints.

### **Statement 1: 'The knapsack problem is NP-hard' — [TRUE]**

The **0/1 Knapsack** optimization **problem** ('find the **subset** **maximizing** **profit** subject to capacity W') is provably **NP-Hard**. Its decision version ('does a subset exist with profit >= P and weight.

### **Statement 2: 'Boolean Satisfiability Problem (SAT) is NP-complete' — [TRUE]**

**The Boolean Satisfiability Problem** (SAT) is the most **celebrated** **problem** in **theoretical** computer science. By the landmark **Cook-Levin Theorem** (1971), SAT was proven to be the very first **NP-Complete** problem..

### **Statement 3: 'Minimum spanning tree is tractable problem' — [TRUE]**

A **problem** is **tractable** if it can be **solved** in deterministic **polynomial time**. **The Minimum Spanning Tree** (MST) problem takes an undirected weighted **graph** and connects all vertices with minimum.

### **Statement 4: 'Searching an ordered/unordered list belongs to tractable problems' — [TRUE]**

**Searching** for a **key** in a list is among the most **basic** **polynomial** **problems** in computer science. For an unordered list of size n, **Linear Search** checks elements sequentially in.

### Architectural Flow & System Diagram

![Figure 3.10: Comment on the following statements regarding NP classes: 'The knapsack problem is NP-hard', 'Boolean Satisfiability Problem (SAT) is NP-complete', 'Minimum spanning tree is tractable problem', and 'The problem of Searching an ordered/unordered list belongs to the class of tractable problems'](diagrams/u3_q10.png)

*Figure 3.10: Architectural Flow and Conceptual System Diagram for Comment on the following statements regarding NP classes: 'The knapsack problem is NP-hard', 'Boolean Satisfiability Problem (SAT) is NP-complete', 'Minimum spanning tree is tractable problem', and 'The problem of Searching an ordered/unordered list belongs to the class of tractable problems'*

### Mathematical Formulation & Algorithmic Mechanics

**Mathematical Reductions and Complexity Formulations:**

```text
Statement 1: Subset-Sum ≤_p 0/1 Knapsack  ==>  0/1 Knapsack is NP-Hard
Statement 2 (Cook-Levin Theorem): For all L ∈ NP, L ≤_p SAT  ==>  SAT is NP-Complete
Statement 3: T_MST(V, E) = O(E log V) ∈ P  ==>  Tractable
Statement 4: T_search(n) = O(log n) ⊂ O(n) ∈ P  ==>  Tractable
```

The **recurrence relation** and algorithmic **parameters** defined above govern the execution bounds of the procedure. Under rigorous asymptotic analysis, the system exhibits Best-Case **Time Complexity** Ω(n), **Average-Case Time** Complexity Θ(n log n), **Worst-Case Time** Complexity O(n²), and Auxiliary **Space Complexity** O(n), ensuring **deterministic** resource allocation.

### Worked Example & Practical Simulation

**Verification** of **Truth Assignment** for SAT (**Belonging** to NP): • **Boolean** **Formula**: Φ = (x1 ∨ ¬x2) ∧ (¬x1 ∨ x2 ∨ x3) ∧ (¬x3). • **Certificate** (Candidate Assignment): x1 = True, x2 = True, x3 = False. • Polynomial Verification:   Clause 1: (True ∨ False) = True.   Clause 2: (False ∨ True ∨ False) = True.   Clause 3: (¬False) = True. All clauses satisfied in 3 simple evaluation steps (O(n) time). Confirms SAT is in NP.

### Key Comparative Evaluation

| SPPU Examination Statement | Verdict | Complexity Class | Deterministic Time Bound | Formal Justification |
| :--- | :--- | :--- | :--- | :--- |
| 0/1 Knapsack is NP-hard | TRUE | NP-Hard | O(n·W) pseudo-poly | Polynomial reduction from Subset-Sum |
| Boolean SAT is NP-complete | TRUE | NP-Complete | O(2^n) brute force | Cook-Levin Theorem: First proven NP-complete problem |
| Minimum Spanning Tree is tractable | TRUE | Class P (Tractable) | O(E log V) via Kruskal's | Solvable via greedy matroid optimization in polynomial time |
| List Searching is tractable | TRUE | Class P (Tractable) | O(log n) sorted / O(n) unsorted | Trivially solvable in linear or logarithmic polynomial time |

### Concluding Exam Takeaway

All four **statements** are **validated** as mathematically true: Searching and MST are tractable **Class** P problems, whereas Boolean SAT is **NP-complete** and the Knapsack **optimization** problem is **NP-hard**.

**Textbook Reference:** Design and Analysis of Algorithms by Parag H. Dave & Himanshu B. Dave (Pearson), Chapter 12: NP-Completeness, Sections 12.1–12.4, Pages 455–468.

---
