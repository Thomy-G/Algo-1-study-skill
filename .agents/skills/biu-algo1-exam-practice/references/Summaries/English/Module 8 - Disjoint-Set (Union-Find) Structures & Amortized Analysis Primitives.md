---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-18
---
# Module 8 - Disjoint-Set (Union-Find) Structures & Amortized Analysis Primitives

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 8: Disjoint-Set (Union-Find) Structures & Amortized Analysis Primitives

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering Disjoint-Set data structures (Union-Find) and Amortized Analysis. It details tracking tree ranks, the structural impact of path compression, and the exact problem-solving paradigms frequently tested in Bar-Ilan University exams. Mastery of these concepts is essential for successfully building adaptive components and tracking non-trivial operational cost balances.

### 1. Foundational Vocabulary & Disjoint-Set Primitives

In past exams, you are frequently required to analyze partitions or track structural tree height adjustments. A Disjoint-Set data structure maintains a collection $\mathcal{S} = \{S_1, S_2, \dots, S_k\}$ of disjoint dynamic sets. Each set is identified by a unique member designated as its **representative**.

#### 1.1 The Dynamic Set Interface Operations

The three foundational operational primitives defining this standard library structure are:

- `MAKE-SET(x)`: Creates a new dynamic set containing exactly one element $x$. Since sets are disjoint, $x$ must not belong to any currently tracked set.
    
- `UNION(x, y)`: Merges the dynamic set containing $x$ ($S_x$) and the set containing $y$ ($S_y$) into a single joint set $S_x \cup S_y$.
    
- `FIND-SET(x)`: Returns a pointer to the unique representative element managing the dynamic set currently containing element $x$.
    

#### 1.2 Tree-Based Implementations & Optimization Primitives

We represent sets as directed trees, where each node contains a pointer to its parent: `parent[x]`. The root of the tree serves as the representative and satisfies `parent[root] == root`.

To prevent trees from degenerating into linear chains of length $O(n)$, we enforce two essential structural heuristics:
1. **Union by Rank (איחוד לפי דרגה):** Each node maintains an integer `rank[x]`, which represents a strict upper bound on the tree's height. When running a `UNION(x, y)` operation, always attach the root of the tree with the **smaller rank** to point directly to the root of the tree with the **larger rank**. If the ranks are identical, select one arbitrarily to be the parent and increment its rank by 1.
2. **Path Compression (דחיסת מסלולים):** Used during `FIND-SET(x)`. As the algorithm traverses the path from $x$ up to the root representative, it updates the parent pointer of _every_ intermediate node along the path to point **directly to the root**. This restructures the tree to be flat without invalidating rank metrics.

```plaintext
MAKE-SET(x):
    parent[x] = x
    rank[x] = 0

FIND-SET(x):
    if x != parent[x]:
        parent[x] = FIND-SET(parent[x]) // Path Compression recursion
    return parent[x]

UNION(x, y):
    LINK(FIND-SET(x), FIND-SET(y))

LINK(x, y):
    if rank[x] > rank[y]:
        parent[y] = x
    else:
        parent[x] = y
        if rank[x] == rank[y]:
            rank[y] = rank[y] + 1
```

### 2. Math Invariants & Performance Metrics

#### 2.1 Structural Rank Properties

When managing a forest via Union by Rank, the following invariants are strictly preserved for all elements $x$:

- If `parent[x] != x`, then $\text{rank}[x] < \text{rank}[\text{parent}[x]]$. Ranks strictly increase as you ascend toward a root.
    
- A root node of rank $r$ must manage a tree containing **at least $2^r$ unique elements**.
    
- The maximum rank of any node in a forest of size $n$ is bounded by $\lfloor \log_2 n \rfloor$.
    

#### 2.2 Amortized Asymptotic Performance Bounds

Amortized analysis tracks the average cost of an operation over a worst-case sequence of actions. For a disjoint-set structure initialized with $n$ entries via `MAKE-SET` operations followed by a sequence of $m$ total mixed operations ($m \ge n$):

|**Optimization Heuristics Enforced**|**Worst-Case Single Operation**|**Total Sequence Cost Bound**|**Amortized Cost per Operation**|
|---|---|---|---|
|Baseline Linked-Tree Representation|$\Theta(n)$|$O(m \cdot n)$|$O(n)$|
|**Union by Rank Only**|$\Theta(\log n)$|$O(m \log n)$|$O(\log n)$|
|**Path Compression Only**|$\Theta(n)$|$\Theta(m \log_{1 + m/n} n)$|$O(\log_{1 + m/n} n)$|
|**Both Optimizations Enforced**|$\Theta(\log n)$|$\mathbf{O(m \cdot \alpha(n))}$|$\mathbf{O(\alpha(n))}$|

The tracking metric $\alpha(n)$ represents the **Inverse Ackermann function**, which grows so slowly that for all practical values of universe sizes ($n < 2^{2^{16}}$), it satisfies $\alpha(n) \le 4$, making the operations **essentially constant time**.

#### 2.3 Methods of Amortized Analysis (שיטות לניתוח לשיעורין)

Amortized analysis guarantees the average cost of an operation in the worst case using three standard techniques:

##### 1. Aggregate Analysis (שיטת המקבץ)
Show that for a sequence of $n$ operations, the total worst-case time complexity is $T(n)$. The amortized cost per operation is then simply:
$$\hat{c} = \frac{T(n)}{n}$$
*No state variables or potentials are used. It is a direct global summation bound.*

##### 2. The Accounting Method (שיטת החיוב / פנקסנות)
We assign an **amortized cost** $\hat{c}_i$ to each operation:
*   If the amortized cost $\hat{c}_i$ exceeds the actual cost $c_i$, we deposit the difference ($\hat{c}_i - c_i$) as **credit** onto specific elements in the data structure.
*   If the amortized cost $\hat{c}_i$ is less than the actual cost $c_i$, we withdraw credit to pay for the excess work.
*   **Constraint:** The total accumulated credit must remain non-negative at all times: $\sum_{i=1}^n \hat{c}_i \ge \sum_{i=1}^n c_i$.

##### 3. The Potential Method (שיטת הפוטנציאל)
We define a potential function $\Phi(D)$ that maps the state of the data structure $D$ to a real number. The amortized cost of the $i$-th operation is:
$$\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$$
*   **The Global Bound:** The total amortized cost for $n$ operations is:
    $$\sum_{i=1}^n \hat{c}_i = \sum_{i=1}^n c_i + \Phi(D_n) - \Phi(D_0)$$
*   **Constraint:** If we prove $\Phi(D_i) \ge \Phi(D_0)$ for all $i$ (commonly $\Phi(D_0) = 0$ and $\Phi(D_i) \ge 0$), then the total amortized cost is an absolute upper bound on the actual cost: $\sum c_i \le \sum \hat{c}_i$.

### 3. Exam-Style Applications & Problem Solving Techniques

#### 3.1 Paradigm A: Real-Time Verification of Unique Spanning Structures (2025 Moed B)

> [!IMPORTANT]
> 
> **Exam Target:** If an exam problem requires tracking connectivity components incrementally as edges are introduced dynamically—or asks you to detect the exact index where a graph first becomes fully connected or contains a cycle—do not rerun BFS/DFS. Use a **Union-Find data structure** to process the sequence in near-linear time.

#### 3.2 Worked Example: Dynamic Backbone Discovery Matrix

Suppose you are given an unweighted, undirected graph $G = (V, E)$ containing $n$ vertices and $m$ edges. The edges are revealed to you one at a time in a specific chronological sequence: $e_1, e_2, \dots, e_m$. Design an algorithm running in $O(m \cdot \alpha(n))$ time that determines the **exact edge index $k$** at which the graph transitions from being disconnected to being **fully connected**, and outputs the structural edges forming that connectivity backbone.

##### Step-by-Step Problem Resolution:

1. **Initialize the Component Tracking Forest:**
    
    Initialize a Union-Find structure. For each vertex $u \in V$, execute `MAKE-SET(u)`. Track the total number of independent components using an integer variable: `ComponentCount` $\leftarrow n$.
    
2. **Process the Chronological Edge Stream:**
    
    Loop sequentially through the edge list from $i = 1$ to $m$, where $e_i = (u, v)$:
    
    - Find the representative root of the set containing $u$: `root_u` $\leftarrow$ `FIND-SET(u)`.
        
    - Find the representative root of the set containing $v$: `root_v` $\leftarrow$ `FIND-SET(v)`.
        
    - **Evaluate Set Equivalences:**
        
        - If `root_u == root_v`, the vertices $u$ and $v$ already belong to the same connected component. This means edge $e_i$ forms a cycle and does not alter connectivity. Skip it.
            
        - If `root_u != root_v`, this edge bridges two previously isolated components. Merge them: execute `UNION(root_u, root_v)`, add $e_i$ to your tracking backbone list, and decrement your component counter: `ComponentCount` $\leftarrow$ `ComponentCount` $- 1$.
            
3. **Evaluate the Global Termination Condition:**
    
    Immediately following a successful component merge, check the counter status:
    
    - If `ComponentCount == 1`, this edge $e_i$ marks the exact transition point where the graph becomes fully connected. Stop execution, output the index $k = i$, and return the accumulated backbone edge list.
        
4. **Handle Persistent Disconnection Invariants:**
    
    If the execution loop completes the entire edge list up to index $m$ and `ComponentCount > 1` remains true, the graph never becomes fully connected. Output $-1$.
    
5. **Complexity Analysis Verification:**
    
    - Initializing $n$ elements takes $O(n)$ time.
        
    - For each of the $m$ incoming edges, we perform at most a constant number of `FIND-SET` and `UNION` operations.
        
    - Enforcing both Union by Rank and Path Compression bounds the sequence cost to $O(m \cdot \alpha(n))$.
        
    - Total Runtime: $\mathbf{O(m \cdot \alpha(n))}$, which satisfies the optimal amortized performance requirement.
        

#### 3.3 Paradigm B: Potential Functions for Custom Amortized Counters (2024 Moed A / 2025 Moed C)

> [!NOTE]
> 
> **Exam Target:** When asked to prove that an unusual or complex data structure operation has a low amortized cost (such as a stack with a bulk-delete operation or a counter with asymmetric bit flips), use the **Potential Method (שיטת הפוטנציאל)**. Define a custom potential function $\Phi$ that maps the data structure's configuration to a real number, and prove that the amortized cost $\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$ is small.

#### 3.4 Worked Example: The Clear-and-Increment Asymmetric Bit Counter

Design a custom binary counter structure of size $k$ bits that supports a standard `INCREMENT` operation (adding 1 to the counter) and a tracking `RESET` operation (setting all bits to 0 simultaneously). Standard bit flips cost $1$ unit of work, but a `RESET` operation that modifies $b$ active high bits costs exactly $b$ units of work. Prove that a sequence of $n$ mixed operations starting from an all-zero counter costs at most **$O(n)$ total worst-case work**, yielding an amortized cost of **$O(1)$ per operation**.

##### Step-by-Step Problem Resolution:

1. **Define the Data Structure States and Potential Function:**
    
    Let $D_i$ represent the state of the binary counter after the $i$-th operation. Define a potential function $\Phi(D_i)$ that tracks the total number of bits currently set to $1$ in the counter:
    
    $$\Phi(D_i) = \text{number of 1-bits in the counter state } D_i$$
    
    - **Non-Negativity Verification:** Since a counter cannot contain a negative number of active bits, $\Phi(D_i) \ge 0$ holds true for all states.
        
    - **Initial State Invariant:** The counter starts at all zeros, meaning $\Phi(D_0) = 0$. Since the potential is initially 0 and can never become negative, the total amortized cost provides an absolute upper bound on the true total actual cost.
        
2. **Analyze the Amortized Cost of an INCREMENT Operation:**
    
    Suppose the $i$-th operation is an `INCREMENT`. To add 1, the counter flips a sequence of $t$ trailing 1-bits to 0-bits, and then flips the next 0-bit to a 1-bit.
    
    - **Actual Cost ($c_i$):** We perform $t + 1$ total bit flips $\implies c_i = t + 1$.
        
    - **Potential Delta ($\Delta \Phi$):** The operation turns $t$ bits from 1 to 0, and 1 bit from 0 to 1:
        
        $$\Phi(D_i) - \Phi(D_{i-1}) = 1 - t$$
        
    - **Amortized Cost Calculation ($\hat{c}_i$):**
        
        $$\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1}) = (t + 1) + (1 - t) = 2 = \mathbf{O(1)}$$
        
3. **Analyze the Amortized Cost of a RESET Operation:**
    
    Suppose the $i$-th operation is a `RESET`. Let $b$ be the total number of 1-bits currently active in the counter state $D_{i-1}$.
    
    - **Actual Cost ($c_i$):** Setting all active bits to 0 requires exactly $b$ operations $\implies c_i = b$.
        
    - **Potential Delta ($\Delta \Phi$):** Since the counter becomes completely empty, the final potential drops to 0:
        
        $$\Phi(D_i) - \Phi(D_{i-1}) = 0 - b = -b$$
        
    - **Amortized Cost Calculation ($\hat{c}_i$):**
        
        $$\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1}) = b + (-b) = 0 = \mathbf{O(1)}$$
        
4. **Aggregate Global Convergence Bounds:**
    
    Since the amortized cost of an `INCREMENT` is at most 2 and the amortized cost of a `RESET` is 0, every operation in the sequence is bounded by a constant value. For any sequence of $n$ mixed operations:
    
    $$\sum_{i=1}^n c_i \le \sum_{i=1}^n \hat{c}_i \le \sum_{i=1}^n 2 = 2n = \mathbf{O(n)}$$
    
    This proves that the asymmetric `RESET` operation cannot cause an algorithmic bottleneck, guaranteeing an amortized cost of **$O(1)$ per operation**.
    

### 5. Summary Matrix of Disjoint-Set & Amortized Performance

This matrix summarizes the runtime profiles and design characteristics of Disjoint-Set implementations and Amortized Analysis models.

|**Structure / Operational Primitive**|**Optimization Heuristics Applied**|**Worst-Case Single Run Cost**|**Amortized Single Run Cost**|**Dominant Space Complexity**|
|---|---|---|---|---|
|**Disjoint-Set Forest**|None (Naive Tree Links)|$\Theta(n)$|$\Theta(n)$|$\Theta(n)$|
|**Disjoint-Set Forest**|Union by Rank Only|$\Theta(\log n)$|$\Theta(\log n)$|$\Theta(n)$|
|**Disjoint-Set Forest**|Path Compression Only|$\Theta(n)$|$\Theta(\log_{1 + m/n} n)$|$\Theta(n)$|
|**Disjoint-Set Forest**|Union by Rank + Path Compression|$\Theta(\log n)$|$\mathbf{O(\alpha(n))}$ _(Ackermann)_|$\Theta(n)$|
|**Dynamic Array Expansion**|Double capacity upon full threshold|$\Theta(n)$|$\mathbf{O(1)}$|$\Theta(n)$|
|**Clear-and-Increment Counter**|Potential Method tracking active bits|$\Theta(k)$|$\mathbf{O(1)}$|$\Theta(k)$|

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]