# 📝 Practice Session - Linear Programming

Welcome to the practice session on **Linear Programming (LP) & Randomized Geometry**. Below are 3 questions covering LP formulations, dualities, Seidel's LP algorithm, and Welzl's Smallest Enclosing Disc algorithm.

---

## Question 1 [30 Points]

*   **a) [10 Points]** Formulate the Maximum Flow problem on a directed flow network $G = (V,E)$ with source $s$, sink $t$, and edge capacities $c(u,v) \ge 0$ as a Linear Program (LP) in standard maximization form.
*   **b) [10 Points]** Formulate the Single-Source Shortest Paths (SSSP) problem from a source $s \in V$ on a directed graph $G = (V,E)$ with edge weights $w(u,v) \in \mathbb{R}$ (assuming no negative cycles) as a Linear Program (LP) in standard maximization form.
*   **c) [10 Points]** Write the **dual** of the SSSP LP from part (b), and explain the physical or network interpretation of this dual LP.

---

## Question 2 [35 Points]

*   **a) [10 Points]** Describe the incremental step of Seidel's randomized LP algorithm. Specifically, explain what happens when the $i$-th randomly chosen constraint $h_i$ is violated by the current optimal solution $x_{i-1}$, and how the problem is reduced to $d-1$ dimensions.
*   **b) [15 Points]** Prove that the expected runtime of Seidel's algorithm is $O(d! \cdot n)$ using backwards analysis. Clearly formulate the recurrence relation and show the induction steps.
*   **c) [10 Points]** Explain how the algorithm handles the base cases: (1) when $d=1$, and (2) when $n=0$. Specify their runtime complexities.

---

## Question 3 [35 Points]

*   **a) [10 Points]** Describe Welzl's randomized recursive algorithm to find the smallest enclosing disc of a set of 2D points $P$, specifying the roles of the input sets $P$ (points) and $R$ (boundary support set).
*   **b) [15 Points]** Prove that the expected runtime of Welzl's algorithm is $O(n)$ using backwards analysis. Formulate and solve the recurrence relation $T(n, i)$ for the expected time.
*   **c) [10 Points]** Explain the concept of a **support set** (boundary points) of the smallest enclosing disc. Prove why the maximum size of the support set $R$ is 3, and explain how the disc is calculated in the base cases when $|R|=1, 2, 3$.
