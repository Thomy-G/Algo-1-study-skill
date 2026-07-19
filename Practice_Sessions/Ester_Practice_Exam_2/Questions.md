# 📝 Ester Practice Exam 2 - Questions

---

## Question 1 [20 Points]
A company needs to schedule $n$ tasks to be performed by $m$ workers. 
- Each task $i$ has a demand $d_i$ representing the exact number of hours needed to complete it.
- Each worker $j$ has a capacity $c_j$ representing the maximum number of hours they can work.
- Some tasks can only be performed by a subset of workers. Specifically, we are given a set of pairs $S \subseteq \{1,\dots,n\} \times \{1,\dots,m\}$, where $(i, j) \in S$ means task $i$ can be assigned to worker $j$.
- Each worker $j$ can spend at most $h_{ij}$ hours working on task $i$ (where $h_{ij}$ is given).

*   **a) [6 Points]** Describe the flow network graph structure (vertices and directed edges) and the capacities used to model the assignments, task demands, and worker limits.
*   **b) [7 Points]** Explain how to handle the task demand lower bounds $d_i$ by converting the lower-bound circulation problem to a standard maximum flow problem.
*   **c) [7 Points]** Prove the correctness of the reduction (i.e. show that a feasible task assignment exists if and only if the maximum flow in the transformed network is saturating).

---

## Question 2 [20 Points]
Let $T[0 \dots n-1]$ be a text string and $P[0 \dots m-1]$ be a pattern string over an alphabet $\Sigma$. 
In addition to normal characters, the pattern $P$ can contain a special wildcard character `*` (joker), which matches any character in $\Sigma$.

*   **a) [6 Points]** Formulate the matching condition at index $i$ as a mathematical sum $S(i)$ that equals $0$ if and only if there is a match (handling both normal characters and the wildcard `*` represented as 0).
*   **b) [8 Points]** Show how to expand $S(i)$ and compute the terms efficiently using polynomial multiplications via FFT. Describe the reversed pattern formulation.
*   **c) [6 Points]** Explain how to block the text $T$ to achieve the overall $O(n \log m)$ runtime complexity.

---

## Question 3 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph where the weight of every edge $e \in E$ is either $1$ or $2$.

*   **a) [10 Points]** Describe the modified priority queue data structure and explain how the `EXTRACT-MIN` and `DECREASE-KEY` operations are implemented in $O(1)$ time.
*   **b) [10 Points]** Analyze the total time and space complexity of the modified Prim's algorithm, proving it runs in $O(|V| + |E|)$ time.

---

## Question 4 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph with $n$ vertices. We run Karger's randomized algorithm to find a global minimum cut of $G$.

*   **a) [12 Points]** Prove that the probability that a single run of Karger's algorithm successfully finds a minimum cut of $G$ is at least $\frac{2}{n(n-1)}$.
*   **b) [8 Points]** Analyze the number of independent runs of Karger's algorithm required to ensure that a minimum cut is found with probability at least $1 - \frac{1}{e}$.

---

## Question 5 [20 Points]
Suppose we are given a Linear Program with $n$ constraints in $d$ dimensions:
$$\text{Maximize } c^T x \quad \text{subject to } a_i^T x \le b_i \quad \forall 1 \le i \le n$$
where $d$ is a very small constant (low-dimensional LP).

*   **a) [8 Points]** Describe the randomized incremental algorithm (Seidel's LP) to solve this problem.
*   **b) [7 Points]** Prove that the expected runtime of the algorithm is $O(d! \cdot n)$ using backwards analysis.
*   **c) [5 Points]** Explain how the algorithm handles the base case when $d=1$.
