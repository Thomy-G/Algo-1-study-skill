# 📝 Ester Practice Exam 2 - Questions

---

## Question 1 [20 Points]
A company needs to schedule $n$ tasks to be performed by $m$ workers. 
- Each task $i$ has a demand $d_i$ representing the exact number of hours needed to complete it.
- Each worker $j$ has a capacity $c_j$ representing the maximum number of hours they can work.
- Some tasks can only be performed by a subset of workers. Specifically, we are given a set of pairs $S \subseteq \{1,\dots,n\} \times \{1,\dots,m\}$, where $(i, j) \in S$ means task $i$ can be assigned to worker $j$.
- Each worker $j$ can spend at most $h_{ij}$ hours working on task $i$ (where $h_{ij}$ is given).

*   **Your Task:** Describe how to reduce the problem of finding a feasible task assignment (if one exists) to a standard circulation problem with demands and capacities. Prove the correctness of your reduction.

---

## Question 2 [20 Points]
Let $T[0 \dots n-1]$ be a text string and $P[0 \dots m-1]$ be a pattern string over an alphabet $\Sigma$. 
In addition to normal characters, the pattern $P$ can contain a special wildcard character `*` (joker), which matches any character in $\Sigma$.

*   **Your Task:** Describe an algorithm that finds all occurrences of the pattern $P$ in the text $T$ (where wildcards are matched correctly). Your algorithm must run in $O(n \log m)$ time. Explain its correctness and analyze its complexity.

---

## Question 3 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph where the weight of every edge $e \in E$ is either $1$ or $2$.

*   **Your Task:** Prove that Prim's algorithm for finding a Minimum Spanning Tree of $G$ can be implemented to run in $O(|V| + |E|)$ time. Describe the data structures and modification needed to achieve this runtime.

---

## Question 4 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph with $n$ vertices. We run Karger's randomized algorithm to find a global minimum cut of $G$.

*   **Your Task:**
    1. Prove that the probability that a single run of Karger's algorithm successfully finds a minimum cut of $G$ is at least $\frac{2}{n(n-1)}$.
    2. Analyze the number of independent runs of Karger's algorithm required to ensure that a minimum cut is found with probability at least $1 - \frac{1}{e}$.

---

## Question 5 [20 Points]
Suppose we are given a Linear Program with $n$ constraints in $d$ dimensions:
$$\text{Maximize } c^T x \quad \text{subject to } a_i^T x \le b_i \quad \forall 1 \le i \le n$$
where $d$ is a very small constant (low-dimensional LP).

*   **Your Task:**
    1. Describe the randomized incremental algorithm (Seidel's LP) to solve this problem.
    2. Prove that the expected runtime of the algorithm is $O(d! \cdot n)$.
    3. Explain how the algorithm handles the base case when $d=1$.
