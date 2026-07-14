# Algorithms 1 (89-220) - 2026 Moed A Exam Questions

Welcome to the practice session for **2026 Moed A**. Below are the 5 questions from the exam, translated into English.

---

## Question 1: A* Search & Potential Functions (20 Points)

Let $G=(V,E)$ be a directed graph with a non-negative edge weight function $w:E\rightarrow\mathbb{R}^{+}$ and let $p:V\rightarrow\mathbb{R}$ be a potential function for the $A^*$ algorithm to find a shortest path from a source $s\in V$ to a target $t\in V$.

*   **a) [6 Points]** Define a **feasible potential function** in the context of the $A^*$ algorithm.
*   **b) [7 Points]** Suppose we have an arbitrarily chosen subset of vertices $S\subseteq V$. Assume that for all $u\in V$ and $v_{i}\in S$, the shortest-path distance $\delta(u,v_{i})$ from $u$ to $v_{i}$ in $G$ is precomputed and known. Prove or disprove whether the following potential function is feasible:
    $$p_{1}(u)=\max_{v_{i}\in S}\{\delta(u,v_{i})-\delta(t,v_{i})\}$$
*   **c) [7 Points]** Under the same assumptions as in part (b), prove or disprove whether the following potential function is feasible:
    $$p_{2}(u)=\min_{v_{i}\in S}\{\delta(u,v_{i})-\delta(t,v_{i})\}$$

---

## Question 2: Degree Sequence Realization (20 Points)

The **Degree Sequence Realization** problem is defined as follows:
*   **Input:** Two arrays $I=[i_{1},...,i_{n}]$ and $O=[o_{1},...,o_{n}]$ of size $n$ containing natural numbers.
*   **Output:** Does there exist a simple directed graph $G=(V,E)$ (without self-loops or multiple edges) such that $|V|=n$ and for all $1\le j\le n$:
    $$\text{in\_deg}(v_j) = i_j \quad \text{and} \quad \text{out\_deg}(v_j) = o_j$$
*   **Your Task:** Describe an algorithm, as efficient as possible, that decides the Degree Sequence Realization problem. If such a graph $G$ exists, your algorithm should construct and return it (represented as an adjacency list). Prove the correctness of your algorithm and analyze its runtime complexity.

---

## Question 3: Triangles Containing Each Edge (20 Points)

Let $G=(V,E)$ be an undirected graph. A triangle is a set of three vertices $\{u, v, w\}$ such that all three edges $(u,v)$, $(v,w)$, and $(w,u)$ exist in $E$.

*   **a) [12 Points]** Assuming $|E|\in\Theta(|V|^{2})$, describe an algorithm, as efficient as possible, that returns for each edge $e\in E$ the number of triangles in $G$ in which $e$ participates. Prove the correctness of your algorithm and analyze its runtime complexity.
    *(Note: Maximum points will only be awarded for an algorithm with runtime complexity $O(|V|^{\omega})$, where $\omega$ is the matrix multiplication exponent).*
*   **b) [8 Points]** Now assume that the graph is relatively sparse, specifically $|E|\in O(|V|^{1.5})$. Describe an algorithm, as efficient as possible, that returns for each edge $e\in E$ the number of triangles in $G$ in which $e$ participates. Prove the correctness of your algorithm and analyze its runtime complexity.

---

## Question 4: Minimum Spanning Trees in Complete Bipartite Graphs (20 Points)

Let $G=(L\cup R,E)$ be a complete bipartite graph (meaning $E$ contains all possible edges between $L$ and $R$). Let $|L|=p$ and $|R|=q$. We are given distinct real numbers $a_{1},a_{2},...,a_{p}$ and $b_{1},b_{2},...,b_{q}$. We define the edge weight function $w:E\rightarrow\mathbb{R}$ as follows:
$$\forall l_{i}\in L, r_{j}\in R: \quad w(l_{i},r_{j})=a_{i}+b_{j}$$

*   **a) [7 Points]** Let $a_{min}=\min_{i}\{a_{i}\}$ (corresponding to node $l_{min} \in L$) and $b_{min}=\min_{j}\{b_{j}\}$ (corresponding to node $r_{min} \in R$). Prove that $(l_{min},r_{min})\in E_{T}$ for any Minimum Spanning Tree $T=(L\cup R,E_{T})$ of $G$.
*   **b) [8 Points]** Prove that for every edge $(l_{i},r_{j})\in E$ such that $l_{i}\ne l_{min}$ and $r_{j}\ne r_{min}$, there exists a cycle $C$ in $G$ such that $(l_{i},r_{j})$ is the strictly heaviest edge on $C$.
*   **c) [5 Points]** Describe an algorithm, as efficient as possible, to find an MST in $G$. Prove the correctness of your algorithm and analyze its complexity.

---

## Question 5: Distributed Vertex Coloring in the LOCAL Model (20 Points)

*Recall: In the LOCAL model of distributed computing, communication proceeds in synchronous rounds. In each round, every vertex in an undirected graph $G=(V,E)$ can send a message to and receive a message from each of its neighbors. We denote the maximum degree of the graph by $\Delta$.*

*   **a) [12 Points]** Suppose that in a single round, each vertex $u\in V$ independently and uniformly chooses an integer $b_{u}$ from the range:
    $$\left[1,\left\lceil\frac{\Delta}{10\log(|V|)}\right\rceil\right]$$
    Prove that with high probability (w.h.p.), for all $u\in V$:
    $$|\{w\in N(u) \mid b_{w}=b_{u}\}|\in O(\log|V|)$$
*   **b) [8 Points]** Assume there exists an algorithm $A$ in the LOCAL model that, for any undirected graph $G=(V,E)$, colors $G$ with a legal coloring using $5\Delta$ colors in $O(\log\Delta)$ rounds, succeeding with high probability.
    Describe an algorithm, as efficient as possible, in the LOCAL model that colors $G=(V,E)$ with a legal coloring using $O(\Delta)$ colors and succeeds with high probability. Analyze the round complexity, prove the success probability, and explain the correctness of your algorithm.
