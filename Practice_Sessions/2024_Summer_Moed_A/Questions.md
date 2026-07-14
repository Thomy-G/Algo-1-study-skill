# Algorithms 1 (89-220) - 2024 Summer Moed A Exam Questions

Welcome to the practice session for **2024 Summer Moed A**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Minimum Vertex Cover on a Tree (20 Points)

A **Vertex Cover** of an undirected graph $G=(V,E)$ is a subset of vertices $S \subseteq V$ such that for every edge $(u,v) \in E$, at least one of $u$ or $v$ belongs to $S$.

*   **a) [10 Points]** Given a tree $T$ with $n$ vertices rooted at $r$, describe a recursive formula to compute the size of the minimum vertex cover of $T$. Explain the correctness of your formula.
*   **b) [10 Points]** Describe a dynamic programming algorithm based on your recursive formula. Explain its correctness and analyze its time and space complexities.

---

## Question 2: Reverse-Delete MST Algorithm (20 Points)

Consider the following **Reverse-Delete** algorithm to find a Minimum Spanning Tree of a connected, undirected graph $G = (V,E)$ with edge weights:
1. Iterate over all edges in $E$ in non-increasing order of their weights.
2. For each edge $e$:
   - If removing $e$ from the graph does **not** disconnect the graph, remove $e$ from $E$.
   - Otherwise, keep $e$ in $E$.
3. Return the remaining graph $G = (V,E)$.

*   **a) [5 Points]** Prove that the algorithm indeed returns a spanning tree of $G$.
*   **b) [15 Points]** Prove that the returned spanning tree is a Minimum Spanning Tree of $G$.

---

## Question 3: Dynamic SSSP Updates (20 Points)

Let $G=(V,E)$ be a directed graph with an edge weight function $w: E ightarrow \mathbb{R}$.
Let $v \in V$ be a designated vertex, and let $G' = G[V \setminus \{v\}]$ be the graph obtained by removing $v$ and its incident edges.
Suppose we are already given all-pairs shortest path (APSP) distances in $G'$ (i.e. $\delta_{G'}(a,b)$ is known for all $a,b \in V \setminus \{v\}$).

*   **a) [10 Points]** Describe an algorithm running in $O(|V|^2)$ time that computes the shortest path distance $\delta_G(u,v)$ from every vertex $u \in V$ to $v$ in $G$. Prove its correctness.
*   **b) [10 Points]** Suppose we are now given, for every vertex $u \in V$, the shortest path distances $\delta_G(u,v)$ and $\delta_G(v,u)$ in $G$. Describe an algorithm running in $O(|V|^2)$ time that computes the shortest path distance $\delta_G(s,t)$ for all pairs of vertices $s,t \in V$. Explain its correctness.

---

## Question 4: Two Edges crossing Every Min Cut (20 Points)

Let $G=(V,E)$ be a flow network with source $s$ and sink $t$, where edge capacities are positive even integers. We are given two specific edges $e_1, e_2 \in E$.

*   **Your Task:** Describe an algorithm that decides whether **both edges $e_1$ and $e_2$ cross every minimum $s-t$ cut** in $G$.
    Analyze its complexity and prove its correctness.
    *(Note: Maximum points will only be awarded for an algorithm that runs the max-flow algorithm at most twice).*

---

## Question 5: Randomized Matrix Equality Verification (20 Points)

*   **a) [15 Points]** Given four binary $n 	imes n$ matrices $A, B, C, D$, describe an efficient randomized algorithm that decides whether $A \cdot B = C \cdot D \pmod 2$ succeeding with probability at least $0.5$. Prove its correctness and analyze its complexity.
*   **b) [5 Points]** Use the algorithm from part (a) to design an algorithm that succeeds with high probability (w.h.p.). What is its runtime complexity?
