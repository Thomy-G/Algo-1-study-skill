# Algorithms 1 (89-220) - 2025 Summer Moed B Exam Questions

Welcome to the practice session for **2025 Summer Moed B**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Vertex Capacities & Smallest Edge-Count Min Cut (25 Points)

*   **a) [13 Points]** Suppose we are given a flow network $G = (V,E)$ where, in addition to edge capacities, each vertex $v \in V \setminus \{s,t\}$ has a **vertex capacity** $l(v)$ limiting the total flow that can pass through $v$. Describe an algorithm to find the maximum flow in this network. Prove its correctness and analyze its complexity.
*   **b) [12 Points]** Given a standard flow network $G = (V,E)$ with integer capacities, describe an algorithm that finds a minimum $s-t$ cut that contains the **minimum number of edges**. Explain the correctness of your algorithm.

---

## Question 2: Dynamic MST Edge Weight Decrease (20 Points)

Let $G=(V,E)$ be a connected, undirected graph with distinct edge weights, and let $T$ be its unique Minimum Spanning Tree (MST).
Suppose we decrease the weight of a single edge $e_0 
otin T$ to a new weight $w'(e_0) < w(e_0)$.

*   **Your Task:** Describe an algorithm, as efficient as possible, to find the new MST of the graph after this change. Prove the correctness of your algorithm and analyze its complexity.

---

## Question 3: Bottleneck Paths in a DAG (15 Points)

Let $G=(V,E)$ be a Directed Acyclic Graph (DAG) with edge weights, and let $s, t \in V$.
For any path $P$ from $s$ to $t$, the **heavy weight** of $P$ is the weight of the heaviest edge on $P$.
The **bottleneck value** between $s$ and $t$ is the minimum, over all paths $P$ from $s$ to $t$, of the heavy weight of $P$.

*   **Your Task:** Describe an algorithm to compute the bottleneck value between $s$ and $t$ in $G$ running in $O(|V| + |E|)$ time. Explain its correctness.

---

## Question 4: Dynamic Programming Egg Dropping (20 Points)

You are given $m$ identical eggs and a building with $n$ floors. There is a critical floor $x$ ($0 \le x \le n$) such that any egg dropped from a floor higher than $x$ will break, and any egg dropped from floor $x$ or below will not break.
If an egg breaks, you cannot use it again. If it survives, you can reuse it.

*   **Your Task:** Write a recursive formula to find the minimum number of drops needed to determine $x$ with certainty in the worst case. Based on this, write a dynamic programming algorithm, and analyze its time and space complexities.

---

## Question 5: Expected Value in Secretary Problems (20 Points)

*   **a) [10 Points]** In the classic Secretary Problem with $N$ candidates arriving in a uniform random order, consider the following greedy strategy: hire the first candidate. For each subsequent candidate, if they are better than the currently hired candidate, fire the current one and hire the new one. Calculate the expected number of hirings/firings.
*   **b) [10 Points]** Suppose we have $K$ open positions and we hold $K$ secretaries at the same time. When a new candidate arrives, if she is better than at least one of the $K$ currently hired secretaries, we fire the worst of the $K$ and hire her. Calculate the expected number of replacements as a function of $N$ and $K$.
