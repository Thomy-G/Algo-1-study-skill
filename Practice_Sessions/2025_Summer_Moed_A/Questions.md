# Algorithms 1 (89-220) - 2025 Summer Moed A Exam Questions

Welcome to the practice session for **2025 Summer Moed A**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: Matrix Invariants & Graph Reductions (20 Points)

*   **a) [6 Points]** Describe a boolean matrix $M$ of size $n \times n$ containing the **minimum possible number of 1s**, such that its boolean square $M^2 = M \times M$ is entirely filled with 1s (i.e., for all $i, j$: $M^2[i,j] = 1$). Prove your answer.
*   **b) [7 Points]** Describe a boolean matrix $M$ of size $n \times n$ containing the **minimum possible number of 1s**, such that its $n$-th power $M^n$ is upper-triangular of 1s (i.e., for all $i, j$: if $j \ge i$ then $M^n[i,j] = 1$, else $M^n[i,j] = 0$). Prove your answer.
    *(Hint for a & b: Translate the matrix powers into paths in a directed graph).*
*   **c) [7 Points]** Give an example of a directed graph $G = (V,E)$ containing exactly **one negative-weight edge**, and two vertices $u, v \in V$, such that running Dijkstra's SSSP algorithm from $u$ fails to find the correct shortest path distance to $v$.

---

## Question 2: Diameter of an Undirected Tree (20 Points)

The **diameter** of an undirected tree $T=(V,E)$ is the maximum distance between any pair of vertices in $T$.
*   **a) [10 Points]** Given an undirected, unweighted tree $T$ rooted at $r$, describe a recursive formula to compute the diameter of $T$. Explain the correctness of your formula.
*   **b) [10 Points]** Describe a dynamic programming algorithm based on your recursive formula. Analyze its time and space complexities.

---

## Question 3: Bottleneck Spanning Trees (20 Points)

A **Bottleneck Spanning Tree (BST)** of an undirected graph $G=(V,E)$ is a spanning tree $T$ that minimizes the weight of its heaviest edge.
*   **a) [10 Points]** Prove that every Minimum Spanning Tree (MST) of $G$ is also a Bottleneck Spanning Tree (BST) of $G$.
*   **b) [10 Points]** Given a graph $G$ and an integer $k$, describe an algorithm that decides whether the bottleneck value of $G$ is at most $k$ in $O(|V| + |E|)$ time. Analyze its complexity.

---

## Question 4: Flow Networks and Cut Properties (20 Points)

*   **a) [10 Points]** Let $G=(V,E)$ be a flow network where the capacity of every edge is exactly 1. Prove or disprove: If the length of the shortest path from $s$ to $t$ is $k$, then the maximum flow value in $G$ is at most $|E|/k$.
*   **b) [10 Points]** Let $(S, T)$ and $(S', T')$ be two minimum $s-t$ cuts in a flow network $G$. Prove that $(S \cup S', T \cap T')$ is also a minimum $s-t$ cut in $G$.

---

## Question 5: Tail Quicksort Complexity (20 Points)

Consider the following variant of Quicksort that uses only one recursive call, called `Tail_Quicksort(A, p, r)`:
```
Tail_Quicksort(A, p, r)
1. while p < r:
2.     q = Partition(A, p, r)
3.     Tail_Quicksort(A, p, q-1)
4.     p = q + 1
```
*   **a) [5 Points]** Prove that `Tail_Quicksort` correctly sorts any input array $A[1 \dots n]$.
*   **b) [15 Points]** Prove formally that the expected number of comparisons made by `Tail_Quicksort` is $O(n \log n)$.
