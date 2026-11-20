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
