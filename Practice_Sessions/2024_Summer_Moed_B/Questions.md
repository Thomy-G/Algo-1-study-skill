# Algorithms 1 (89-220) - 2024 Summer Moed B Exam Questions

Welcome to the practice session for **2024 Summer Moed B**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Longest Contiguous Decreasing Subsequence (20 Points)

Let $S = a_1 a_2 \dots a_n$ be a sequence of $n$ integers. A subsequence is defined as **contiguous and strictly decreasing** if it is of the form $a_i, a_{i+1}, \dots, a_j$ where $a_k > a_{k+1}$ for all $i \le k < j$.

*   **a) [10 Points]** Describe a recursive formula to compute the length of the longest contiguous strictly decreasing subsequence in $S$. Explain the correctness of your formula.
*   **b) [10 Points]** Describe an efficient dynamic programming algorithm based on your recursive formula. Analyze its time and space complexities.

---

## Question 2: Minimum Graded Weight Spanning Tree (20 Points)

Let $G=(V,E)$ be a connected, undirected graph with vertex weights $w: V ightarrow \mathbb{R}$.
For any spanning tree $T$, the **graded weight** of $T$ is defined as:
$$W_d(T) = \sum_{v \in V} w(v) \cdot deg_T(v)$$
where $deg_T(v)$ is the degree of $v$ in $T$.

*   **Your Task:** Describe an algorithm, as efficient as possible, to find a spanning tree $T$ that minimizes the graded weight $W_d(T)$. Prove the correctness of your algorithm and analyze its complexity.

---

## Question 3: SSSP passing through $v_0$ (20 Points)

Let $G=(V,E)$ be a directed weighted graph with an edge weight function $w: E ightarrow \mathbb{R}$, and a designated vertex $v_0 \in V$.

*   **Your Task:** Describe an algorithm running in $O(|V||E|)$ time that computes, for every pair of vertices $u, v \in V$, the weight of the shortest path from $u$ to $v$ that passes through $v_0$. Prove the correctness of your algorithm.

---

## Question 4: Edges crossing Every Min Cut (20 Points)

Let $G=(V,E)$ be a flow network with source $s$, sink $t$, positive integer edge capacities, and a subset of edges $F \subseteq E$.

*   **Your Task:** Describe an algorithm that decides whether **all edges in $F$ cross every minimum $s-t$ cut** in $G$. Explain the correctness of your algorithm and analyze its complexity.

---

## Question 5: Reservoir Sampling Proof (20 Points)

In Reservoir Sampling, we want to sample $k$ elements uniformly at random from a stream of $n$ elements ($k < n$).
The algorithm is as follows:
1. Initialize an array $R[1 \dots k]$ with the first $k$ elements $x_1, \dots, x_k$.
2. For each subsequent element $x_i$ (for $i = k+1 \dots n$):
   - Choose a random index $j$ uniformly at random from $\{1, \dots, i\}$.
   - If $j \le k$, set $R[j] = x_i$.
   - Otherwise, discard $x_i$.
3. Return $R$.

Prove that for each $i \ge k+1$, the element $x_i$ is in $R$ at the end of the algorithm with probability exactly $k/n$:
*   **a) [8 Points]** Prove the statement for the last element $x_n$.
*   **b) [12 Points]** Prove the statement for any element $x_i$ where $k+1 \le i \le n$.
