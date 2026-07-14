---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-18
---
# Module 6 - Dynamic Programming Foundations & Subproblem Expansion Systems

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 6: Dynamic Programming Foundations & Subproblem Expansion Systems

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering Dynamic Programming (DP). It details subproblem construction choices, recurrence modeling, and the state-space expansion systems frequently tested in Bar-Ilan University exams. Mastery of these concepts is essential for successfully solving complex optimization sequences and constrained state structures.

### 1. Foundational Vocabulary & The SRTBOT Framework

In past exams, your ability to achieve full credit depends on breaking down a recursive problem using a strict, formalized paradigm. The course standard relies on the **SRTBOT** framework to structurally define and evaluate every dynamic programming solution.

#### 1.1 The SRTBOT Operational Paradigm

Every descriptive DP problem in your exam must be unrolled into these six explicit sections:

- **S – Subproblems (תת-בעיות):** Define the precise mathematical meaning of your state variables in plain words. Specify exactly what input slices (prefixes, suffixes, substrings) or additional parameters your function tracks.
    
- **R – Relations (נוסחת נסיגה):** Write out the recurrence equation. Express the solution to a larger subproblem as a combination of choices over strictly smaller subproblems (e.g., using $\min$, $\max$, $\sum$).
    
- **T – Topological Order (סדר טופולוגי):** Define the clear direction of subproblem evaluation (e.g., "increasing order of string length", "decreasing index $i$ from $n$ down to $0$"). This proves that your subproblems form a Directed Acyclic Graph (DAG) and contain zero circular dependencies.
    
- **B – Base Cases (מקרי בסיס):** State the explicit terminal values for the simplest trivial subproblems where no further recursion can occur (e.g., when string length equals 0 or budget bounds collapse).
    
- **O – Original Problem (הבעיה המקורית):** Identify exactly where the final answer to the complete global instance is stored within your subproblem state matrix (e.g., $F[0, n-1]$ or $\max_{k} F[n, k]$).
    
- **T – Time & Space Complexity (ניתוח סיבוכיות):** Provide a strict counting argument: $\text{Total Time} = (\text{Number of Unique Subproblems}) \times (\text{Work done per Subproblem})$. State both time and space bounds in Big-O notation.
    

#### 1.2 Core Subproblem Layout Patterns

The geometry of your data structure dictates the baseline design of your subproblems:

|**Input Structure**|**Standard Subproblem Choice**|**Size of Subproblem Space**|**Standard Branching Dimension**|
|---|---|---|---|
|**Single Sequence** (String/Array)|**Prefixes:** $x[0 \dots i]$ or **Suffixes:** $x[i \dots n-1]$|$O(n)$|Deciding what action to perform on the boundary element ($x[i]$).|
|**Single Sequence** (Non-contiguous)|**Substrings:** $x[i \dots j]$|$O(n^2)$|Deciding where to split the sequence ($k$) or evaluating endpoints $i$ and $j$.|
|**Dual Sequences** (Two Strings)|**Prefix Pairs:** $x[0 \dots i]$ and $y[0 \dots j]$|$O(n \cdot m)$|Evaluating matching alignments between boundary elements $x[i]$ and $y[j]$.|
|**Integers / Knapsack Budgets**|**Value Scaling:** Remaining capacity $w \in [0, W]$|$O(W)$|Deciding whether to absorb or reject an item of a given weight.|

### 2. Core Foundational Applications

#### 2.1 Longest Common Subsequence (LCS)

- **Objective:** Find the maximum length of a non-contiguous character sequence shared between two strings $X$ (length $n$) and $Y$ (length $m$).
    
- **Subproblem Definition:** Let $L[i, j]$ be the length of the LCS of prefixes $X[1 \dots i]$ and $Y[1 \dots j]$.
    
- **Recurrence Relation Rule:**
    
    $$L[i, j] = \begin{cases} L[i-1, j-1] + 1 & \text{if } X[i] == Y[j] \\ \max\big(L[i-1, j], \ L[i, j-1]\big) & \text{if } X[i] \neq Y[j] \end{cases}$$
    
- **Topological Direction:** Increasing order of indices $i$ and $j$.
    
- **Complexity:** Time: $\mathbf{O(n \cdot m)}$, Space: $O(n \cdot m)$ (can be optimized to $O(\min(n,m))$ space if only tracking the previous row).
    

#### 2.2 Longest Increasing Subsequence (LIS)

- **Objective:** Find the maximum length of a strictly increasing subsequence within a single array $A$ of size $n$.
    
- **Subproblem Definition:** Let $I[i]$ be the length of the longest increasing subsequence of $A[1 \dots n]$ that **must explicitly start with element $A[i]$**.
    
- **Recurrence Relation Rule:**
    
    $$I[i] = 1 + \max_{j > i \mid A[j] > A[i]} \big\{ I[j] \cup \{0\} \big\}$$
    
- **Topological Direction:** Decreasing index $i$ from $n$ down to $1$.
    
- **Original Problem Mapping:** Since the global LIS can start at any arbitrary index, the final solution is mapped via: $\text{Result} = \max_{1 \le i \le n} I[i]$.
    
- **Complexity:** Time: $\mathbf{O(n^2)}$, Space: $O(n)$.

#### 2.3 Matrix Chain Multiplication (כפל שרשרת מטריצות)

*   **Objective:** Given a sequence of $n$ matrices $\langle A_1, A_2, \dots, A_n \rangle$ where matrix $A_i$ has dimension $p_{i-1} \times p_i$, find the most efficient way to multiply these matrices (minimizing the number of scalar multiplications).
*   **Subproblem Definition:** Let $m[i, j]$ be the minimum number of scalar multiplications needed to compute the product matrix $A_{i \dots j}$ (for $1 \le i \le j \le n$).
*   **Recurrence Relation:**
    $$m[i, j] = \begin{cases} 0 & \text{if } i == j \\ \min\limits_{i \le k < j} \{ m[i, k] + m[k+1, j] + p_{i-1}p_k p_j \} & \text{if } i < j \end{cases}$$
*   **Topological Direction:** Increasing order of substring length $L = j - i$ from 1 to $n-1$.

```plaintext
MATRIX-CHAIN-ORDER(p):
    n = len(p) - 1
    allocate m matrix of size (n+1) x (n+1)
    for i = 1 to n:
        m[i, i] = 0
    for L = 2 to n: // L is the chain length
        for i = 1 to n - L + 1:
            j = i + L - 1
            m[i, j] = inf
            for k = i to j - 1:
                q = m[i, k] + m[k+1, j] + p[i-1]*p[k]*p[j]
                if q < m[i, j]:
                    m[i, j] = q
    return m[1, n]
```
*   **Complexity:** Time: $\Theta(n^3)$, Space: $\Theta(n^2)$.

### 3. State-Space Constraint Expansion Systems

> [!CAUTION]
> 
> **Exam Warning (Subproblem Constraint Expansion):** A common mistake in exam design is trying to solve highly constrained sequences using only baseline 1D or 2D subproblem allocations. If a problem states that your choices are bound by secondary conditions (e.g., "cannot pick three items in a row", "must use exactly $k$ special modifications"), you **must expand your state space** by adding explicit helper dimensions to maintain past decision memory.

#### 3.1 The Expansion Rule Framework

If a normal subproblem state tracks an index $i$, but your transition rules require knowing your exact current operational configuration, expand the state definition:

$$F[i] \implies F[i, \text{State}]$$

where $\text{State}$ is a discrete variable explicitly maintaining your historical context vector.

### 4. Exam-Style Applications & Problem Solving Techniques

#### 4.1 Paradigm A: RNA Secondary Structure Matching (2025 Moed A / 2024 Moed B)

- **Concept:** When sequence-matching rules mandate that characters must pair across a single sequence without structural crossings (creating pseudo-palindromic nested pairs), a linear prefix approach fails. You must utilize a **Substring $[i, j]$ expansion matrix** that sweeps outward based on structural length deltas.
    

#### 4.2 Worked Example: RNA Stem-Loop Interval Matching

Given an RNA single-strand sequence array $S = s_1s_2\dots s_n$ over alphabet $\Sigma = \{A, U, C, G\}$. A valid pairing can occur if and only if bases are complementary: $(A, U)$ or $(C, G)$. Furthermore, to prevent physical steric strain, bases $s_i$ and $s_j$ can only pair if they are separated by an absolute threshold distance of at least 4 positions (i.e., $j - i > 4$). Pairs cannot cross each other. Design an algorithm running in $O(n^3)$ time that calculates the **maximum total number of non-crossing base pairs** that can be formed within $S$.

##### Step-by-Step SRTBOT Problem Resolution:

1. **S – Subproblems:**
    
    Let $M[i, j]$ be the maximum number of valid base pairs that can be formed within the continuous substring segment $S[i \dots j]$, where $1 \le i \le j \le n$.
    
2. **R – Relations:**
    
    For a given substring interval $S[i \dots j]$, we consider the boundary element $s_j$. It can either remain unpaired, or it can pair with some complementary base $s_k$ situated within the substring range:
    
    $$M[i, j] = \max \begin{cases} M[i, j-1] & \text{(base } s_j \text{ remains completely unpaired)} \\ \max\limits_{k \in [i, \ j-5] \mid \text{ValidPair}(s_k, s_j)} \big\{ 1 + M[i, k-1] + M[k+1, j-1] \big\} & \text{(base } s_j \text{ pairs with base } s_k\text{)} \end{cases}$$
    
    where $\text{ValidPair}(s_k, s_j) == \text{True} \iff (s_k, s_j) \in \{(A,U), (U,A), (C,G), (G,C)\}$.
    
3. **T – Topological Order:**
    
    Subproblems must be computed in strictly increasing order of **substring length delta** $L = j - i$, ranging from $L = 0$ up to $n-1$. This ensures that when calculating $M[i, j]$, all interior sub-segments ($M[i, k-1]$ and $M[k+1, j-1]$) have shorter lengths and are already completely computed.
    
4. **B – Base Cases:**
    
    For any substring interval where the length is too short to permit a pairing under the separation constraint ($j - i \le 4$), no pairs can be legally formed:
    
    $$M[i, j] = 0 \quad \forall \ (j - i) \le 4$$
    
5. **O – Original Problem:**
    
    The answer to the complete global instance encompassing the full single-strand sequence is stored at:
    
    $$\text{Result} = M[1, n]$$
    
6. **T – Time & Space Complexity:**
    
    - **Subproblem Count:** There are $O(n^2)$ unique coordinate pairs $(i, j)$ where $1 \le i \le j \le n$.
        
    - **Work per Subproblem:** The outer choice checks an unpaired option ($O(1)$) and loops across index $k$ up to $j-5$, performing at most $O(n)$ evaluations per cell.
        
    - Total Time Complexity: $O(n^2) \times O(n) = \mathbf{O(n^3)}$.
        
    - Total Space Complexity: $\mathbf{O(n^2)}$ to maintain the two-dimensional memoization lookup table.
        

#### 4.3 Paradigm B: Multi-Player Resource Scaling & Token Choices (2025 Moed B / 2024 Moed A)

> [!IMPORTANT]
> 
> **Exam Target:** In competitive game-theory allocations where two players select elements sequentially from the boundaries of an input array, simple maximization fails because your state transitions are dependent on the choices of an adversarial opponent. You must design a **max-min subproblem system** that models the opponent's strategy to find your guaranteed optimal outcome.

#### 4.4 Worked Example: The Alternating Boundary Selection Game (2025 Retake Variant)

You are given an array of $n$ coins $V = [v_1, v_2, \dots, v_n]$, where $n$ is an even integer and each coin has a known positive scalar value $v_i$. Two players, Player 1 (You) and Player 2 (Opponent), take turns picking a single coin. A player can only pick a coin from one of the currently remaining endpoints of the array (either the leftmost coin or the rightmost coin). Player 2 plays perfectly aggressively, aiming to minimize the total cash you collect. Design an algorithm running in $O(n^2)$ time that computes the maximum total value Player 1 is guaranteed to win.

##### Step-by-Step SRTBOT Problem Resolution:

1. **S – Subproblems:**
    
    Let $W[i, j]$ be the maximum total coin value Player 1 can securely win from the remaining coin segment subarray $V[i \dots j]$ on their turn.
    
2. **R – Relations:**
    
    When it is Player 1's turn to choose from the segment $V[i \dots j]$, they can either select the leftmost coin $v_i$ or the rightmost coin $v_j$.
    
    - **If Player 1 chooses $v_i$:** The remaining array becomes $V[i+1 \dots j]$. Player 2 will then strategically select an endpoint to leave Player 1 with the absolute minimum possible value on the subsequent turn. Thus, Player 2 forces Player 1 into the worst remaining state: $\min(W[i+2, j], W[i+1, j-1])$.
        
    - **If Player 1 chooses $v_j$:** The remaining array becomes $V[i \dots j-1]$. Player 2 will similarly minimize Player 1's future outcome, forcing them into state: $\min(W[i+1, j-1], W[i, j-2])$.
        
    
    Combining these choices using an aggressive maximization strategy yields:
    
    $$W[i, j] = \max \begin{cases} v_i + \min\big(W[i+2, j], \ W[i+1, j-1]\big) & \text{(Player 1 selects left coin } v_i\text{)} \\ v_j + \min\big(W[i+1, j-1], \ W[i, j-2]\big) & \text{(Player 1 selects right coin } v_j\text{)} \end{cases}$$
    
3. **T – Topological Order:**
    
    Subproblems must be evaluated in strictly increasing order of **coin interval width** $D = j - i$, ranging from $D = 1$ up to $n-1$ in steps of 2 (since the size updates by 2 elements per round of turns).
    
4. **B – Base Cases:**
    
    - When only a single coin remains ($j - i == 0$), Player 1 simply takes it: $W[i, i] = v_i$.
        
    - When exactly two adjacent coins remain ($j - i == 1$), Player 1 greedily selects the maximum of the two: $W[i, i+1] = \max(v_i, v_{i+1})$.
        
5. **O – Original Problem:**
    
    The absolute optimal guaranteed value for the full game starting from the complete unpicked coin sequence array is stored at:
    
    $$\text{Result} = W[1, n]$$
    
6. **T – Time & Space Complexity:**
    
    - **Subproblem Count:** The grid maintains an $n \times n$ coordinate layout $\implies O(n^2)$ unique interval cells.
        
    - **Work per Subproblem:** Computing the nested $\max$-$\min$ expression requires only $O(1)$ constant scalar comparisons.
        
    - Total Time Complexity: $O(n^2) \times O(1) = \mathbf{O(n^2)}$.
        
    - Total Space Complexity: $\mathbf{O(n^2)}$ to maintain the memoization table.
        

### 5. Summary Matrix of Classic Algorithmic Reductions

This matrix summarizes the runtime profiles and design characteristics of classic Dynamic Programming applications.

| **Algorithm Paradigm / Problem** | **Core Subproblem Structure**                       | **Branching Choice Factor**          | **Time Complexity**                          | **Space Complexity**            |
| -------------------------------- | --------------------------------------------------- | ------------------------------------ | -------------------------------------------- | ------------------------------- |
| **Fibonacci Numbers**            | Integer sequence down to 0                          | 2 choices ($F_{n-1} + F_{n-2}$)      | $O(n)$                                       | $O(1)$ (with rolling variables) |
| **LCS (Dual Strings)**           | Prefix string coordinates $(i, j)$                  | 2-3 alignment checks                 | $O(n \cdot m)$                               | $O(n \cdot m)$                  |
| **LIS (Single Sequence)**        | Element-anchored starting tags $I[i]$               | $O(n)$ inner array sweep             | $O(n^2)$                                     | $O(n)$                          |
| **RNA Stem Pairing**             | Substring boundaries $[i, j]$                       | $O(n)$ internal split points         | $O(n^3)$                                     | $O(n^2)$                        |
| **Alternating Coin Game**        | Substring interval boundaries $[i, j]$              | 2 choices with opponent minimization | $O(n^2)$                                     | $O(n^2)$                        |
| **Subset Sum / Knapsack**        | Product space of items and weight capacity $(i, w)$ | 2 choices (absorb vs. reject)        | $\mathbf{O(n \cdot W)}$ _(Pseudopolynomial)_ | $O(n \cdot W)$                  |

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]