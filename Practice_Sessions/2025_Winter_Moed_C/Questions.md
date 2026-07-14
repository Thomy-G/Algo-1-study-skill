# Algorithms 1 (89-220) - 2025 Winter Moed C Exam Questions

Welcome to the practice session for **2025 Winter Moed C**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Flow Networks & Min Cut Verifications (20 Points)

Let $G=(V,E)$ be a flow network with a source $s$, sink $t$, positive edge capacity function $c$, and a designated edge $e = (u,v) \in E$.

*   **a) [5 Points]** Define a **minimum $s-t$ cut** of $G$.
*   **b) [7 Points]** Describe an algorithm, as efficient as possible, that returns `True` if and only if **$e$ belongs to every minimum $s-t$ cut** of $G$. Explain the correctness of your algorithm and analyze its runtime complexity.
*   **c) [8 Points]** Describe an algorithm, as efficient as possible, that returns `True` if and only if **$e$ belongs to no minimum $s-t$ cut** of $G$. Explain the correctness of your algorithm and analyze its runtime complexity.

---

## Question 2: Shortest Paths with Even Number of Edges (20 Points)

Let $G=(V,E)$ be a directed weighted graph with an edge weight function $w: E ightarrow \mathbb{R}$. We are guaranteed that $G$ contains **no negative cycles**.

*   **Your Task:** Describe an algorithm that finds, for every vertex $v \in V$, the weight of the shortest path starting from $v$ to all other vertices that contains an **even number of edges**. Prove the correctness of your algorithm and analyze its runtime complexity.

---

## Question 3: Minimum Steps to Reach $n$ (20 Points)

Starting from the number 1, we want to reach a given target integer $n$ in the minimum number of steps. In each step, we can perform one of the following operations:
1. Add 1 to the current number ($x ightarrow x + 1$).
2. Multiply the current number by 2 ($x ightarrow 2x$).

*   **Your Task:** Describe an algorithm that takes $n$ as input and returns the minimum number of steps required to reach $n$. Analyze its runtime complexity and prove its correctness.

---

## Question 4: Randomized Connectivity Tester (20 Points)

Let $G=(V,E)$ be an undirected graph. We are guaranteed that either:
*   $G$ is connected, or
*   $G$ is disconnected, but **every connected component has size at most $|V|/4$**.

We are given a query function $f(u, v)$ that runs in $O(1)$ time and returns `True` if there is a path between $u$ and $v$ in $G$, and `False` otherwise.

*   **a) [10 Points]** Describe a randomized algorithm that decides whether $G$ is connected, succeeding with probability **at least 3/4**. Analyze its runtime complexity and prove its correctness.
*   **b) [10 Points]** Describe a randomized algorithm that decides whether $G$ is connected, succeeding **with high probability** (w.h.p.) in $|V|$. Analyze its runtime complexity and prove its correctness.

---

## Question 5: Edit Distance with Custom Gap Penalty (20 Points)

The **Global Alignment** score between two strings $S$ and $T$ over the alphabet $\Sigma = \{A, C, G, T\}$ is defined by aligning characters or using gaps `'-'`.
The scoring rules are:
*   A match between identical characters adds $+1$ to the score.
*   A mismatch between different characters subtracts $-1$ from the score.
*   A gap `'-'` aligned with any character subtracts $-0.5$ from the score.

Answer the following questions:
*   **a) [10 Points]** Describe a recursive formula to compute the maximum global alignment score between strings $S = s_1 \dots s_n$ and $T = t_1 \dots t_m$. Explain the correctness of your formula.
*   **b) [10 Points]** Describe a dynamic programming algorithm based on your recursive formula. Analyze its time and space complexities.
