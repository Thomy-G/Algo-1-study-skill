# Algorithms 1 (89-220) - 2025 Winter Moed B Exam Questions

Welcome to the practice session for **2025 Winter Moed B**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Flow Networks & Ford-Fulkerson (20 Points)

Let $G=(V,E)$ be a flow network with positive edge capacities, and let $s, t \in V$.

*   **a) [6 Points]** Define an **augmenting path** in the residual graph $G_f$ and its **bottleneck capacity**.
*   **b) [7 Points]** Suppose there is a vertex $u \in V$ whose degree is exactly 11. Describe an algorithm to find the maximum flow in $G$ under the constraint that the flow can use **at most 3 edges** incident to $u$. Explain the correctness of your algorithm and analyze its runtime complexity.
*   **c) [7 Points]** Suppose we modify the Ford-Fulkerson algorithm such that it can find an augmenting path containing cycles (i.e., vertices and edges can be repeated). Explain why this modification can lead to an incorrect result or infinite loop.

---

## Question 2: Randomized Permutation Generation (20 Points)

Consider the following randomized algorithm to generate a permutation of $\{1, 2, \dots, n\}$ in an array $P[1 \dots n]$:
1. Initialize all elements in $P$ to 0.
2. For each $x = 1$ to $n$:
   a. Generate a random index $i$ uniformly at random from $\{1, 2, \dots, n\}$.
   b. While $P[i] 
eq 0$:
      - Generate a new random index $i$ uniformly at random from $\{1, 2, \dots, n\}$.
   c. Set $P[i] = x$.
3. Return $P$.

*   **a) [10 Points]** Analyze the **expected runtime** of this algorithm and its runtime **with high probability (w.h.p.)**.
*   **b) [10 Points]** Prove that the probability of obtaining any specific permutation of $\{1, \dots, n\}$ is exactly $1/n!$ (i.e., the algorithm generates a uniform random permutation).

---

## Question 3: Minimal Feedback Arc Set (20 Points)

Given a directed graph $G = (V, E)$, a subset of edges $A \subseteq E$ is called a **feedback arc set** if $E' = E \setminus A$ contains no directed cycles (i.e., $(V, E \setminus A)$ is a DAG).
We say $A$ is **minimal** if for any proper subset $B \subset A$, the graph $(V, E \setminus B)$ contains at least one directed cycle (i.e., we cannot add any edge of $A$ back to the graph without creating a cycle).

*   **Your Task:** Describe an algorithm, as efficient as possible, that finds a minimal feedback arc set $A$ for a given directed graph $G$. Analyze its runtime complexity and prove its correctness.

---

## Question 4: Forest and Tree Orientation (20 Points)

An **orientation** of an undirected forest $F=(V,E)$ is a directed graph obtained by assigning a direction to each edge in $E$.
We want to find an orientation such that for every vertex $u \in V$, the difference between its in-degree and out-degree is at most 1, i.e.:
$$\max_{u \in V} |in\_deg(u) - out\_deg(u)| \le 1$$

*   **a) [10 Points]** Prove that if $|E| \ge 1$, then for any orientation, there exists at least one vertex $u$ such that $|in\_deg(u) - out\_deg(u)| \ge 1$.
*   **b) [10 Points]** Describe an algorithm, as efficient as possible, that takes an undirected tree $T=(V,E)$ and finds an orientation satisfying the condition $\max_u |in\_deg(u) - out\_deg(u)| \le 1$. Explain the correctness of your algorithm.

---

## Question 5: Longest Complementary-Reverse Subsequence (20 Points)

Let $\Sigma = \{A, T, C, G\}$ be the alphabet of DNA nucleotides, where $A$ complements $T$ (and vice versa) and $C$ complements $G$ (and vice versa).
The complementary-reverse of a string $S$, denoted by $RC(S)$, is obtained by reversing $S$ and replacing each character with its complement.
For example, if $S = 	ext{"GTAGTC"}$, then $RC(S) = 	ext{"GACTAC"}$.

We define a string $W$ to be *complementary-reverse symmetric* if $W = S' \cdot RC(S')$ for some string $S'$.
Given a string $S$ of length $n$, we want to find the length of the longest complementary-reverse symmetric subsequence of $S$.
For example, for $S = 	ext{"GTAGTC"}$, the longest such subsequence is $	ext{"GATC"}$ (since $S'=	ext{"GA"}$ and $RC(S')=	ext{"TC"}$, and $	ext{"GATC"}$ is a subsequence of $S$). The output should be 4.

*   **a) [10 Points]** Describe a recursive formula to solve the problem. Explain the correctness of your formula.
*   **b) [10 Points]** Describe a dynamic programming algorithm based on your recursive formula. Analyze its time and space complexities.
