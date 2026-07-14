# Algorithms 1 (89-220) - 2026 Moed B Exam Questions

Welcome to the practice session for **2026 Moed B**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: DFS Edge Classifications & Properties (20 Points)

Let $G=(V,E)$ be a directed graph.

*   **a) [8 Points]** Define the four types of edges in a DFS traversal of $G$: tree edges, forward edges, back edges, and cross edges.
*   Consider two different DFS runs on $G$, denoted by $R$ and $R'$. The differences between these runs arise from a different permutation of vertices in the main loop of the DFS, and/or different ordering of neighbors in the adjacency lists for each vertex. Let $e \in E$ be an edge.
*   **b) [6 Points]** Prove or disprove: If $e$ is a cross edge in DFS run $R$, then it is guaranteed *not* to be a tree edge in DFS run $R'$.
*   **c) [6 Points]** Prove or disprove: If $e$ is a cross edge in DFS run $R$, then it is guaranteed *not* to be a forward edge in DFS run $R'$.

---

## Question 2: Used Edge in Shortest Paths (20 Points)

Let $G=(V,E)$ be a directed graph, $w:E\rightarrow\mathbb{R}^{+}$ a positive edge weight function, and $s, t \in V$ two vertices. 
An edge $e \in E$ is defined as a **used edge** if there exist **at least 2 different shortest paths** from $s$ to $t$ that contain the edge $e$.

*   **Your Task:** Describe an algorithm, as efficient as possible, that decides whether a given edge $e \in E$ is a used edge. Explain the correctness of your algorithm and analyze its runtime complexity.

---

## Question 3: Binary Array Skip Score Optimization (20 Points)

For a binary array $A=[a_{0},...,a_{n-1}]$, the score of a "skip $k$" (where $1\le k < n$) is defined as the sum of products of elements at distance $k$ from each other. Formally:
$$Score(A,k)=\sum_{i=0}^{n-k-1} a_{i} \cdot a_{i+k}$$
This represents how many times $a_i = 1$ and $a_{i+k} = 1$ simultaneously.
For example, for the array $A = [1, 0, 1, 1, 0, 1]$, the score for $k=3$ is:
$$Score(A,3) = a_0 a_3 + a_1 a_4 + a_2 a_5 = (1\cdot 1) + (0\cdot 0) + (1\cdot 1) = 2$$

*   **Your Task:** Describe an algorithm that takes as input a binary array $A$ and finds an index $k^{*} \in \{1,...,n-1\}$ that maximizes the score, i.e., for all $k \in \{1,...,n-1\}$:
    $$Score(A,k^{*}) \ge Score(A,k)$$
    Analyze the time and space complexity of your algorithm, and explain its correctness.
    *(Hint: Think about polynomial multiplication and the Fast Fourier Transform).*

---

## Question 4: Bucket Sort with $m$ Buckets (20 Points)

Formulate the **Bucket Sort** algorithm for an input of size $n$, where each element is drawn independently and uniformly at random from the range $[0, 1)$. However, instead of using the standard $n$ buckets, use $m$ buckets, where $m \in o(n)$ (meaning $m$ is asymptotically much smaller than $n$).
*   **Your Task:** Analyze the expected runtime of the algorithm as a function of both $n$ and $m$. Explain how changing the number of buckets to $m$ affects the expected runtime compared to standard Bucket Sort (where $m = n$).

---

## Question 5: Modified Coupon Collector (20 Points)

In the Coupon Collector's problem, let $n = 2^l$ be the number of distinct coupons that need to be collected.
*   As long as there are strictly more than $2^{l-1}$ coupons remaining to be collected, the probability of selecting each coupon is uniform (i.e., $1/n$ for each coupon).
*   For each $k \in \{0, ..., l-1\}$, the moment the number of remaining uncollected coupons becomes exactly $2^k$, the probability distribution changes as follows:
    - For any coupon that has **not** been collected yet, its probability of being drawn becomes $\frac{1}{2^{k+1}}$.
    - For any coupon that has **already** been collected, its probability of being drawn becomes $\frac{1}{2n - 2^{k+1}}$.
    - These probabilities remain constant until the number of remaining uncollected coupons becomes $2^{k-1}$ (or all coupons are collected, for $k=0$).

*   **a) [8 Points]** Prove that at any point during this process, the sum of probabilities of all coupons in the sample space is always 1 (i.e., the probability distribution is always valid).
*   **b) [12 Points]** For $1 \le i < n$, let $p_i$ be the probability of drawing a *new* coupon type, given that $i-1$ distinct coupon types have already been collected. Prove that $p_i \ge 1/4$, and prove a tight asymptotic upper bound on the expected number of draws needed to collect all $n$ coupon types.
