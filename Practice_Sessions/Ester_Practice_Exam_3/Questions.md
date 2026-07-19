# 📝 Ester Practice Exam 3 - Questions

---

## Question 1 [20 Points]
Let $G = (V, E)$ be a directed graph, and let $s, t \in V$ be two vertices. A set of paths from $s$ to $t$ is **vertex-disjoint** if no two paths in the set share any vertex in $V \setminus \{s, t\}$.

*   **a) [10 Points]** Describe the vertex splitting reduction to transform the vertex-disjointness constraints into edge-capacity constraints in a standard flow network $G'$.
*   **b) [5 Points]** Explain how to extract the actual vertex-disjoint paths from the computed maximum flow field and analyze the extraction runtime.
*   **c) [5 Points]** Analyze the overall time complexity of finding the paths using Dinic's algorithm on the transformed network.

---

## Question 2 [20 Points]
Given a set of $n$ points $(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$ where all $x_i$ are distinct, we want to find the unique polynomial $P(x)$ of degree at most $n-1$ that passes through all these points.

*   **a) [7 Points]** Describe how to construct the product tree of polynomials in $O(n \log^2 n)$ time.
*   **b) [7 Points]** Explain how to evaluate the derivative $M'(x)$ at all points $x_i$ using fast multipoint evaluation in $O(n \log^2 n)$ time.
*   **c) [6 Points]** Describe the divide-and-conquer summation step to reconstruct the coefficients of $P(x)$ and prove the recurrence relation $T(n) = 2T(n/2) + O(n \log n) = O(n \log^2 n)$.

---

## Question 3 [20 Points]
A spanning tree $T$ of a connected, undirected graph $G = (V, E)$ is a **Bottleneck Spanning Tree** (BST) if the weight of its heaviest edge is minimized over all possible spanning trees of $G$.
Suppose we are given a value $W$. We want to test if there exists a spanning tree of $G$ whose heaviest edge has weight at most $W$.

*   **a) [10 Points]** Describe an algorithm that tests this condition in $O(|V| + |E|)$ time.
*   **b) [10 Points]** Prove that the test is correct (i.e. it returns `TRUE` if and only if such a spanning tree exists).

---

## Question 4 [20 Points]
Suppose we flip a fair coin $n$ times independently. Let $X$ be the random variable representing the total number of heads. We want to bound the probability that $X \ge \frac{3n}{4}$.

*   **a) [7 Points]** Apply **Chebyshev's Inequality** to find an upper bound on $\Pr[X \ge \frac{3n}{4}]$.
*   **b) [7 Points]** Apply **Chernoff Bounds** to find an upper bound on $\Pr[X \ge \frac{3n}{4}]$.
*   **c) [6 Points]** Compare the two bounds asymptotically as $n \to \infty$. Explain which one is tighter and the mathematical reason for this difference.

---

## Question 5 [20 Points]
Let $P$ be a set of $n$ points in the 2D plane. We want to find the smallest enclosing disc that contains all points in $P$.

*   **a) [8 Points]** Describe Welzl's randomized recursive algorithm to find the smallest enclosing disc.
*   **b) [7 Points]** Prove that the expected runtime of the algorithm is $O(n)$ using backwards analysis.
*   **c) [5 Points]** Explain the role of the support set and why its maximum size is 3 in 2D space.
