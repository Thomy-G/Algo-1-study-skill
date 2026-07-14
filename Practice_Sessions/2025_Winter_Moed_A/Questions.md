# Algorithms 1 (89-220) - 2025 Winter Moed A Exam Questions

Welcome to the practice session for **2025 Winter Moed A**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Constrained Minimum Spanning Trees (20 Points)

Let $G=(V,E)$ be a connected, undirected graph with an edge weight function $w:E\rightarrow\mathbb{R}$.

*   **a) [6 Points]** Define a **Minimum Spanning Tree (MST)** of $G$.
*   **b) [7 Points]** Suppose there is a vertex $s \in V$ whose degree is exactly 8. Describe an algorithm that finds a spanning tree of $G$ with the minimum weight among all spanning trees containing **at most 2 edges** incident to $s$ (for simplicity, assume such a spanning tree exists). Explain the correctness of your algorithm and analyze its runtime complexity.
*   **c) [7 Points]** Under the same assumptions, describe an algorithm that finds a spanning tree of $G$ with the minimum weight among all spanning trees containing **at least 2 edges** incident to $s$. Explain the correctness of your algorithm and analyze its runtime complexity.

---

## Question 2: Algo-Mania Coupon Collection Strategy (20 Points)

In a university campus, there is a machine selling "Algo-Mania" collectible cards. There are $n$ distinct types of cards. You want to collect at least one card of each type. The machine offers two buying options:
1. **Option 1 (1 NIS):** Buy a card that is chosen uniformly at random from all $n$ types.
2. **Option 2 (5 NIS):** Buy a card that is chosen uniformly at random from the types you do **not** currently own.

Consider the following algorithm, `Collect-Algo-Mania(n, k)`:
*   While there are strictly more than $k$ uncollected card types:
    - Purchase cards using Option 1 (1 NIS each).
*   While you do not have all $n$ card types:
    - Purchase cards using Option 2 (5 NIS each).

Answer the following questions:
*   **a) [15 Points]** Analyze precisely (i.e., not asymptotically) the expected total cost of this algorithm as a function of $n$ and $k$.
*   **b) [5 Points]** For which value of $k$ is the expected total cost minimized? Prove your answer.

---

## Question 3: Matrix Row and Column Sums Realization (20 Points)

The **Matrix Row and Column Sums Realization** problem is defined as follows:
*   **Input:** Natural numbers $r_1, r_2, \dots, r_n$ and $c_1, c_2, \dots, c_m$.
*   **Output:** Does there exist a binary matrix $M$ of size $n \times m$ (containing only $0$s and $1$s) such that for all $1 \le i \le n$, the sum of the $i$-th row is exactly $r_i$, and for all $1 \le j \le m$, the sum of the $j$-th column is exactly $c_j$?
*   **Your Task:** Describe an algorithm that solves this problem. Explain the correctness of your algorithm and analyze its runtime complexity.
    *(Hint: Think about Network Flow).*

---

## Question 4: Min-Plus Matrix Multiplication (20 Points)

Let $A, B$ be two square matrices of size $n \times n$ where entries are natural numbers or $\infty$.
The **min-plus product** of $A$ and $B$, denoted by $C = A \star B$, is defined as:
$$\forall 1 \le i, j \le n: \quad c_{i,j} = \min_{1 \le k \le n} \{a_{i,k} + b_{k,j}\}$$

The Min-Plus Product problem from a bounded range $[M]$ is defined as:
*   **Input:** Two $n \times n$ matrices $A, B$ whose entries are from $[M] \cup \{\infty\}$, where $[M] = \{1, 2, \dots, M\}$.
*   **Output:** The matrix $C = A \star B$.

Answer the following questions:
*   **a) [14 Points]** Describe an algorithm, as efficient as possible, to solve this problem for $M = 1$. Explain the correctness of your algorithm and analyze its runtime complexity.
    *(Note: The runtime should be dominated by $O(n^\omega)$ where $\omega$ is the matrix multiplication exponent).*
*   **b) [6 Points]** Describe an algorithm to solve the problem for any $M \ge 1$ with runtime complexity $O(M^2 \cdot n^\omega)$. Explain the correctness of your algorithm and analyze its complexity.

---

## Question 5: RNA Secondary Structure Folding (20 Points)

Let $S = s_1, s_2, \dots, s_n$ be an RNA sequence where each $s_i \in \{A, U, C, G\}$. An RNA pairing of $S$ is a set of index pairs that satisfies:
1. Each index appears in at most one pair.
2. For each pair $(i, j)$, $i < j$.
3. Each pair $(i, j)$ must satisfy $(s_i, s_j) \in \{AU, UA, CG, GC\}$.
4. If there is a pair $(i, j)$, then there cannot be another pair $(x, y)$ such that $i < x < j$ and $y \notin [i+1, j-1]$ (i.e., no crossing pairs, or "pseudo-knots").

The **Largest RNA Pairing** problem is defined as:
*   **Input:** An RNA sequence $S$.
*   **Output:** The size of the largest RNA pairing of $S$ (i.e., the maximum number of pairs).
*   **Your Task:** Describe a recursive formula that solves the Largest RNA Pairing problem. Explain the correctness of your recursive formula, and describe a dynamic programming algorithm based on it, including its time and space complexities.
