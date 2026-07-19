# 📝 Ester Practice Exam 3 - Questions

---

## Question 1 [20 Points]
Let $G = (V, E)$ be a directed graph, and let $s, t \in V$ be two vertices. A set of paths from $s$ to $t$ is **vertex-disjoint** if no two paths in the set share any vertex in $V \setminus \{s, t\}$.

*   **Your Task:** Describe an algorithm to find the maximum number of vertex-disjoint paths from $s$ to $t$ in $G$. Prove the correctness of your algorithm and analyze its complexity.

---

## Question 2 [20 Points]
Given a set of $n$ points $(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$ where all $x_i$ are distinct, we want to find the unique polynomial $P(x)$ of degree at most $n-1$ that passes through all these points.

*   **Your Task:** Describe a divide-and-conquer algorithm to compute the coefficients of $P(x)$ in $O(n \log^2 n)$ time (you may assume fast polynomial multiplication in $O(k \log k)$ time using FFT). Explain its correctness and analyze its complexity.

---

## Question 3 [20 Points]
A spanning tree $T$ of a connected, undirected graph $G = (V, E)$ is a **Bottleneck Spanning Tree** (BST) if the weight of its heaviest edge is minimized over all possible spanning trees of $G$.
Suppose we are given a value $W$. We want to test if there exists a spanning tree of $G$ whose heaviest edge has weight at most $W$.

*   **Your Task:**
    1. Describe an algorithm that tests this condition in $O(|V| + |E|)$ time.
    2. Prove that the test is correct (i.e., there exists a spanning tree with bottleneck weight at most $W$ if and only if your test returns `TRUE`).

---

## Question 4 [20 Points]
Suppose we flip a fair coin $n$ times independently. Let $X$ be the random variable representing the total number of heads. We want to bound the probability that $X \ge \frac{3n}{4}$.

*   **Your Task:**
    1. Apply **Chebyshev's Inequality** to find an upper bound on $\Pr[X \ge \frac{3n}{4}]$.
    2. Apply **Chernoff Bounds** to find an upper bound on $\Pr[X \ge \frac{3n}{4}]$.
    3. Compare the two bounds asymptotically as $n \to \infty$. Which one is tighter and why?

---

## Question 5 [20 Points]
Let $P$ be a set of $n$ points in the 2D plane. We want to find the smallest enclosing disc that contains all points in $P$.

*   **Your Task:**
    1. Describe Welzl's randomized recursive algorithm to find the smallest enclosing disc.
    2. Prove that the expected runtime of the algorithm is $O(n)$ using backwards analysis.
    3. Explain the role of the support set (the boundary points of the disc) in the algorithm and why its maximum size is 3.
