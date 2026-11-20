# Topic 13: Distributed Breadth-First Search (Distributed BFS)

This document provides a complete, step-by-step mathematical and network message-passing trace for the **Distributed Breadth-First Search (BFS)** numerical problem appearing in the SPPU DAA Endsem Examination (specifically May/June 2024, 10 Marks).

---

## 1. Algorithmic Foundation & Theory

### 1.1 Distributed Computing Model
In a distributed asynchronous or synchronous network:
- Each node is an autonomous processor with its own local memory and unique identifier (UID).
- There is no global clock or shared memory; processors coordinate exclusively by **passing messages** along network edges.
- Each node knows only its immediate neighbors (local adjacency list).

### 1.2 Objective of Distributed BFS
Given a designated root node $r$, construct a **Breadth-First Search Spanning Tree** rooted at $r$ such that:
1. Every node $u$ knows its parent in the tree: $\text{parent}(u)$.
2. Every node $u$ knows its children: $\text{children}(u)$.
3. Every node $u$ knows its shortest hop distance from the root: $\text{level}(u) = \text{dist}(r, u)$.

### 1.3 Message Types & Protocol
The algorithm operates via wave-based message exchanges:
1. `EXPLORE(level)`: Sent by a node at `level - 1` to propose tree edges to its neighbors.
2. `ACCEPT / PARENT`: Sent by an unvisited node to accept the sender as its parent in the BFS tree.
3. `REJECT / ALREADY_VISITED`: Sent by a node that has already joined the BFS tree at this or an earlier level.
4. `COMPLETED / ECHO`: Propagated from leaf nodes upwards to parents to signal completion of level exploration.

### 1.4 Complexity Analysis
- **Message Complexity:** $O(|E|)$ messages in a synchronized wave model, where each edge transmits a constant number of control messages.
- **Time Complexity:** $O(D)$ synchronous rounds, where $D$ is the network diameter (maximum hop distance from root).

---

## 2. SPPU May/June 2024 PYQ Numerical [10 Marks]

### Problem Statement
> Given a distributed network of 7 nodes $V = \{A, B, C, D, E, F, G\}$ represented by the following adjacency matrix:
>
> | Node | $A$ | $B$ | $C$ | $D$ | $E$ | $F$ | $G$ |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **$A$** | 0 | 1 | 1 | 0 | 0 | 0 | 0 |
> | **$B$** | 1 | 0 | 0 | 1 | 1 | 0 | 0 |
> | **$C$** | 1 | 0 | 0 | 0 | 0 | 1 | 1 |
> | **$D$** | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
> | **$E$** | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
> | **$F$** | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
> | **$G$** | 0 | 0 | 1 | 0 | 0 | 0 | 0 |
>
> Show the stepwise process of how the Distributed Breadth-First Search algorithm works on this graph starting with root node $A$. State the messages exchanged in each wave, the resulting BFS levels, tree edges, and the final spanning tree.

---

### Step 1: Initial Local State Configuration

From the adjacency matrix, each node's local neighbor set is:
- $\text{Neighbors}(A) = \{B, C\}$
- $\text{Neighbors}(B) = \{A, D, E\}$
- $\text{Neighbors}(C) = \{A, F, G\}$
- $\text{Neighbors}(D) = \{B\}$
- $\text{Neighbors}(E) = \{B\}$
- $\text{Neighbors}(F) = \{C\}$
- $\text{Neighbors}(G) = \{C\}$

Initialize all nodes:
- $\forall u \ne A$: $\text{visited}(u) = \text{false}$, $\text{level}(u) = \infty$, $\text{parent}(u) = \text{NULL}$, $\text{children}(u) = \emptyset$.
- Root $A$: $\text{visited}(A) = \text{true}$, $\text{level}(A) = 0$, $\text{parent}(A) = \text{self}$.

---

### Step 2: Stepwise Wave-by-Wave Execution

#### Wave 1: Level 0 $\longrightarrow$ Level 1 Exploration

1. **Root Action:**
   - Root $A$ initiates the BFS at Round 0.
   - Node $A$ sends `EXPLORE(level = 1)` to all its neighbors: $B$ and $C$.
     - Message $M_1$: $A \xrightarrow{\text{EXPLORE}(1)} B$
     - Message $M_2$: $A \xrightarrow{\text{EXPLORE}(1)} C$

2. **Nodes $B$ and $C$ Processing:**
   - **Node $B$:** Receives `EXPLORE(1)` from $A$.
     - Since $\text{visited}(B) = \text{false}$:
       - $\text{visited}(B) \leftarrow \text{true}$
       - $\text{level}(B) \leftarrow 1$
       - $\text{parent}(B) \leftarrow A$
       - Reply: $B \xrightarrow{\text{ACCEPT}} A$
   - **Node $C$:** Receives `EXPLORE(1)` from $A$.
     - Since $\text{visited}(C) = \text{false}$:
       - $\text{visited}(C) \leftarrow \text{true}$
       - $\text{level}(C) \leftarrow 1$
       - $\text{parent}(C) \leftarrow A$
       - Reply: $C \xrightarrow{\text{ACCEPT}} A$

3. **Root $A$ Acknowledgment:**
   - $A$ receives `ACCEPT` from $B \implies \text{children}(A) \leftarrow \{B\}$. Edge $(A, B)$ is marked as a **Tree Edge**.
   - $A$ receives `ACCEPT` from $C \implies \text{children}(A) \leftarrow \{B, C\}$. Edge $(A, C)$ is marked as a **Tree Edge**.
   - Level 1 frontier discovery is complete.

---

#### Wave 2: Level 1 $\longrightarrow$ Level 2 Exploration

1. **Level 1 Nodes Action:**
   Nodes $B$ and $C$ (at Level 1) now send `EXPLORE(level = 2)` to all their unvisited neighbors (excluding parent $A$):
   - **Node $B$:** Sends `EXPLORE(2)` to $D$ and $E$.
     - Message $M_3$: $B \xrightarrow{\text{EXPLORE}(2)} D$
     - Message $M_4$: $B \xrightarrow{\text{EXPLORE}(2)} E$
   - **Node $C$:** Sends `EXPLORE(2)` to $F$ and $G$.
     - Message $M_5$: $C \xrightarrow{\text{EXPLORE}(2)} F$
     - Message $M_6$: $C \xrightarrow{\text{EXPLORE}(2)} G$

2. **Nodes $D, E, F, G$ Processing:**
   - **Node $D$:** Receives `EXPLORE(2)` from $B$. Unvisited $\implies \text{level}(D) = 2, \text{parent}(D) = B$.
     - Reply: $D \xrightarrow{\text{ACCEPT}} B$
   - **Node $E$:** Receives `EXPLORE(2)` from $B$. Unvisited $\implies \text{level}(E) = 2, \text{parent}(E) = B$.
     - Reply: $E \xrightarrow{\text{ACCEPT}} B$
   - **Node $F$:** Receives `EXPLORE(2)` from $C$. Unvisited $\implies \text{level}(F) = 2, \text{parent}(F) = C$.
     - Reply: $F \xrightarrow{\text{ACCEPT}} C$
   - **Node $G$:** Receives `EXPLORE(2)` from $C$. Unvisited $\implies \text{level}(G) = 2, \text{parent}(G) = C$.
     - Reply: $G \xrightarrow{\text{ACCEPT}} C$

3. **Level 1 Parents Acknowledgment:**
   - Node $B$ adds $D$ and $E$ to $\text{children}(B)$: Tree edges $(B, D)$ and $(B, E)$.
   - Node $C$ adds $F$ and $G$ to $\text{children}(C)$: Tree edges $(C, F)$ and $(C, G)$.
   - Level 2 frontier discovery is complete.

---

#### Wave 3: Termination & Echo Phase (Convergecast)

1. Nodes $D, E, F, G$ have no other unvisited neighbors ($\text{degree} = 1$).
2. They are identified as **Leaf Nodes** of the BFS tree.
3. Leaf nodes send `ECHO / COMPLETED` messages upwards:
   - $D \xrightarrow{\text{ECHO}} B$
   - $E \xrightarrow{\text{ECHO}} B$
   - $F \xrightarrow{\text{ECHO}} C$
   - $G \xrightarrow{\text{ECHO}} C$
4. Node $B$ and Node $C$ aggregate all child echoes and send their completion echoes to root $A$:
   - $B \xrightarrow{\text{ECHO}} A$
   - $C \xrightarrow{\text{ECHO}} A$
5. Root $A$ receives echoes from all its children. The distributed BFS algorithm successfully **terminates**.

---

### Step 3: Message Passing Summary Table

| Phase / Round | Sender Node | Receiver Node | Message Sent | Action Taken by Receiver |
| :---: | :---: | :---: | :---: | :--- |
| **Round 1** | $A$ | $B$ | `EXPLORE(1)` | $B$ sets $\text{parent}=A, \text{level}=1$ |
| **Round 1** | $A$ | $C$ | `EXPLORE(1)` | $C$ sets $\text{parent}=A, \text{level}=1$ |
| **Round 1** | $B$ | $A$ | `ACCEPT` | $A$ adds $(A, B)$ to tree edges |
| **Round 1** | $C$ | $A$ | `ACCEPT` | $A$ adds $(A, C)$ to tree edges |
| **Round 2** | $B$ | $D$ | `EXPLORE(2)` | $D$ sets $\text{parent}=B, \text{level}=2$ |
| **Round 2** | $B$ | $E$ | `EXPLORE(2)` | $E$ sets $\text{parent}=B, \text{level}=2$ |
| **Round 2** | $C$ | $F$ | `EXPLORE(2)` | $F$ sets $\text{parent}=C, \text{level}=2$ |
| **Round 2** | $C$ | $G$ | `EXPLORE(2)` | $G$ sets $\text{parent}=C, \text{level}=2$ |
| **Round 2** | $D, E$ | $B$ | `ACCEPT` | $B$ adds $(B, D), (B, E)$ to tree edges |
| **Round 2** | $F, G$ | $C$ | `ACCEPT` | $C$ adds $(C, F), (C, G)$ to tree edges |
| **Round 3** | $D, E$ | $B$ | `ECHO` | Leaves report wave completion to $B$ |
| **Round 3** | $F, G$ | $C$ | `ECHO` | Leaves report wave completion to $C$ |
| **Round 3** | $B, C$ | $A$ | `ECHO` | $B, C$ report full tree completion to Root $A$ |

---

### Step 4: Final Distributed BFS Spanning Tree

```
                     [ Node A ]  (Level 0 - Root)
                     /        \
                    /          \
                   v            v
             [ Node B ]      [ Node C ]  (Level 1)
             /        \      /        \
            v          v    v          v
          [ D ]      [ E ] [ F ]      [ G ]  (Level 2 - Leaves)
```

#### Final Node State Parameters:
| Node | Level ($\text{dist}$) | Parent | Children |
| :---: | :---: | :---: | :---: |
| **$A$** | 0 | Self | $\{B, C\}$ |
| **$B$** | 1 | $A$ | $\{D, E\}$ |
| **$C$** | 1 | $A$ | $\{F, G\}$ |
| **$D$** | 2 | $B$ | $\emptyset$ |
| **$E$** | 2 | $B$ | $\emptyset$ |
| **$F$** | 2 | $C$ | $\emptyset$ |
| **$G$** | 2 | $C$ | $\emptyset$ |

- **Total Tree Edges:** $6$ edges: $\{(A, B), (A, C), (B, D), (B, E), (C, F), (C, G)\}$
- **Cross / Back Edges:** None (graph is a tree network).
- **Total Rounds to Complete:** 3 rounds.
- **Total Messages Exchanged:** 18 messages.
