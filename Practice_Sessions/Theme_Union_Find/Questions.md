# Algorithms 1 (89-220) - Theme: Union-Find & Amortized Complexity

Welcome to the practice session on **Union-Find & Amortized Complexity**. These topics are fundamental to dynamic connectivity (like in Kruskal's algorithm) but are rarely featured as standalone exam questions.

---

## Question 1: Union-Find Heuristics (20 Points)

The **Disjoint-Set Forest** data structure utilizes two primary heuristics to optimize the runtime of operations: **Union-by-Rank** (or Union-by-Size) and **Path Compression**.

*   **a) [10 Points]** Explain how the **Union-by-Rank** heuristic works. Why do we define "rank" instead of "height" when path compression is used?
*   **b) [10 Points]** Explain how the **Path Compression** heuristic works during a `Find` operation. Why is it that using *either* heuristic alone yields a worst-case operation complexity of $O(\log n)$, but using *both* reduces the amortized complexity to $O(\alpha(n))$?

---

## Question 2: Forest Tracing & Path Compression (30 Points)

Suppose we have 8 elements, initially in their own single-element sets: $\{1\}, \{2\}, \dots, \{8\}$, each represented as a tree node of rank 0.
We execute the following sequence of operations using the **Union-by-Rank** heuristic (where ties in rank are broken by making the second tree's root the parent of the first tree's root, i.e., in `Union(x, y)` if `rank(x) == rank(y)`, `parent[x] = y` and `rank(y)++`):

1.  `Union(1, 2)`
2.  `Union(3, 4)`
3.  `Union(5, 6)`
4.  `Union(7, 8)`
5.  `Union(1, 3)`
6.  `Union(5, 7)`
7.  `Union(1, 5)`

*   **a) [15 Points]** Draw or represent the tree forest structure after the 7 `Union` operations are completed. Specify the rank of each root node.
*   **b) [15 Points]** Now we run `Find(1)` with **Path Compression**. Draw the resulting tree forest structure after this `Find(1)` operation completes, and explain which node pointers were updated.

---

## Question 3: Mathematical Properties of Rank (30 Points)

In a disjoint-set forest that uses the **Union-by-Rank** heuristic (without assuming path compression), prove the following properties by induction:

*   **a) [10 Points]** Prove that for any node $x$ of rank $r$, the subtree rooted at $x$ contains at least $2^r$ nodes.
*   **b) [10 Points]** Prove that the number of nodes in the forest of rank exactly $r$ is at most $n/2^r$.
*   **c) [10 Points]** Prove that the maximum rank of any node in a forest of size $n$ is at most $\lfloor \log_2 n \rfloor$.

---

## Question 4: Amortized Analysis via the Potential Method (20 Points)

The amortized complexity of Union-Find can be analyzed using the **Potential Method**.
Define a potential function $\Phi$ for the disjoint-set forest. Explain how we assign potentials to nodes based on their rank and the rank of their parent, and briefly outline how this potential function is used to prove that a sequence of operations takes $O(m \log^* n)$ or $O(m \alpha(n))$ time.
