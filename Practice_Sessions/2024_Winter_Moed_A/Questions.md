# Algorithms 1 (89-220) - 2024 Winter Moed A Exam Questions

Welcome to the practice session for **2024 Winter Moed A**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: FFT, Roots of Unity, and Sparse Multiplication (20 Points)

*   **a) [5 Points]** State the **Weak Duality Theorem** in the context of linear programming.
*   *Note: In parts b and c, you may assume that $n$ is a power of 2.*
*   **b) [5 Points]** For any integer $n > 1$, what is the **sum of the $n$ complex roots of unity** of order $n$? Explain your answer and simplify the expression.
*   **c) [5 Points]** For any integer $n > 1$, what is the **product of the $n$ complex roots of unity** of order $n$? Explain your answer and simplify the expression.
*   **d) [5 Points]** A polynomial $p(x)$ is defined to be $k$-sparse if it has at most $k$ non-zero coefficients. Describe an algorithm for the following problem:
    - **Input:** Two polynomials $A(x)$ and $B(x)$ of degree at most $n$ represented by coefficients, where $B(x)$ is $O(1)$-sparse.
    - **Output:** The product polynomial $C(x) = A(x) \cdot B(x)$ in coefficient representation.
    Analyze the time and space complexity of your algorithm.

---

## Question 2: Kruskal's Edge Sorting (20 Points)

Let $G=(V,E)$ be a connected, undirected graph with an edge weight function $w: E ightarrow \mathbb{R}$.
Prove or disprove the following claim:
For every Minimum Spanning Tree $T = (V, E_T)$ of $G$, there exists a sorting permutation of $E$, denoted by $\pi_T$, which sorts the edges by weight in non-decreasing order, such that running Kruskal's algorithm on $G$ using the sorted order $\pi_T$ returns the tree $T$.

---

## Question 3: Last Edge in Shortest Path DAG (20 Points)

Let $G=(V,E)$ be a directed graph with a positive edge weight function $w: E ightarrow \mathbb{R}^+$.
An edge $(u,v) \in E$ is defined as a **last edge** if there exists some shortest path $P$ in $G$ such that $(u,v)$ is the last edge of $P$.

*   **Your Task:** Describe an algorithm, as efficient as possible, that decides whether a given edge $(u,v)$ is a last edge. Prove the correctness of your algorithm and analyze its complexity.

---

## Question 4: Balanced Global Min Cut (20 Points)

Let $G=(V,E)$ be a directed graph with a positive edge weight function $w: E ightarrow \mathbb{R}^+$.
For a subset $S \subset V$, we denote the cut weight by $w(S, V \setminus S) = \sum_{u \in S, v \in V \setminus S} w(u,v)$.
The global min cut value is $w^* = \min_{\emptyset 
e S \subset V} w(S, V \setminus S)$.

For an integer $k > 1$, we say $G$ is **$1/k$-balanced** if there exists a minimum cut $(S, V \setminus S)$ such that:
$$rac{1}{k} \cdot |V| \le |S| \le \left(1 - rac{1}{k}ight) \cdot |V|$$

*   **a) [10 Points]** Describe a randomized algorithm to find $w^*$ in a **$1/3$-balanced** graph that succeeds with constant probability. Prove its success probability and analyze its complexity.
*   **b) [10 Points]** Use the algorithm in part (a) as a black box to design an algorithm that finds $w^*$ with **high probability** (w.h.p.) in $|V|$. Analyze its complexity.

---

## Question 5: Dynamic Programming Edit Distance (20 Points)

Aligning two strings $S$ and $T$ over the alphabet $\Sigma = \{A, C, G, T\}$ uses gaps `'-'`.
The scoring rules are:
*   A match between identical characters adds $+1$.
*   A mismatch between different characters subtracts $-1$.
*   A gap `'-'` aligned with any character subtracts $-1$.

Answer the following questions:
*   **a) [10 Points]** Describe a recursive formula to compute the maximum alignment score between strings $S = s_1 \dots s_n$ and $T = t_1 \dots t_m$. Explain the correctness of your formula.
*   **b) [10 Points]** Describe a dynamic programming algorithm based on your recursive formula. Analyze its time and space complexities.
