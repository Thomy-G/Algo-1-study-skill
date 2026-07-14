# Algorithms 1 (89-220) - 2025 Winter Moed B Solutions Manual

This document contains the official, rigorous solutions for the 2025 Winter Moed B exam questions.

---

## Question 1: Flow Networks & Ford-Fulkerson (20 Points)

### Part a) [6 Points]
- **Augmenting Path:** Given a flow network $G = (V,E)$ and a flow $f$, an augmenting path $P$ is a simple directed path from $s$ to $t$ in the residual graph $G_f = (V, E_f)$.
- **Bottleneck Capacity:** The bottleneck capacity of an augmenting path $P$, denoted by $c_f(P)$, is the minimum residual capacity of any edge in $P$:
  $$c_f(P) = \min_{(u,v) \in P} c_f(u,v)$$

---

### Part b) [7 Points]
**Algorithm Description:**
The constraint is that the flow uses at most 3 edges incident to $u$. Let $E(u)$ be the edges incident to $u$. Since $	ext{deg}(u) = 11$, there are 11 such edges.
1. Let $\mathcal{S}$ be the set of all subsets of $E(u)$ of size at most 3. The number of such subsets is:
   $$\sum_{i=0}^3 inom{11}{i} = 1 + 11 + 55 + 165 = 232$$
2. For each subset $E_u \in \mathcal{S}$:
   - Construct a modified flow network $G' = (V, E')$:
     - Remove $u$ and all its incident edges.
     - For each edge $(x, u)$ or $(u, y)$ in $E_u$: add direct capacity-constrained edges or route flow through virtual nodes to simulate the chosen active edges, capping their total flow.
     - More simply: For each subset $E_u$ of at most 3 edges:
       - Keep only the edges in $E_u$ and remove all other edges incident to $u$ in $G$.
       - Find the maximum flow in this modified network using Dinic's algorithm.
3. Return the maximum flow found across all 232 configurations.

**Correctness:**
The optimal constrained flow must use some subset of edges incident to $u$ of size at most 3. By running the max-flow algorithm on all possible 232 subgraphs where only these edges are kept and others are removed, we are guaranteed to find the absolute maximum flow that satisfies the constraint.

**Complexity:**
- There are exactly 232 configurations.
- For each, we run Dinic's algorithm which takes $O(|V|^2 |E|)$ time.
- **Total Time Complexity:** $232 	imes O(|V|^2 |E|) = O(|V|^2 |E|)$ time.
- **Total Space Complexity:** $O(|V| + |E|)$.

---

### Part c) [7 Points]
If the augmenting path is allowed to contain cycles, we can route flow along a cycle in the residual graph.
- Since augmenting along a cycle does not increase the net flow from $s$ to $t$ (as the net flow out of $s$ and into $t$ remains unchanged), the value of the flow does not increase.
- However, if the cycle contains backward edges, we might decrease flow along some edges and increase it along others in a loop.
- If the capacities are irrational, this can lead to infinite loops without converging. Even with integer capacities, the algorithm might run infinitely without making progress because it keeps augmenting along the same cycle, violating the termination guarantee of Ford-Fulkerson.

---

## Question 2: Randomized Permutation Generation (20 Points)

### Part a) [10 Points]
**Expected Runtime:**
- When placing the $x$-th element, there are $x-1$ occupied slots in the array, and $n - x + 1$ empty slots.
- The probability of choosing an empty slot in each try is $p_x = rac{n - x + 1}{n}$.
- The number of tries $T_x$ to find an empty slot is a geometric random variable with parameter $p_x$, so $E[T_x] = rac{n}{n - x + 1}$.
- The total expected number of tries is:
  $$E[T] = \sum_{x=1}^n E[T_x] = \sum_{x=1}^n rac{n}{n - x + 1} = n \sum_{j=1}^n rac{1}{j} = n H_n \in \Theta(n \log n)$$
  Each try takes $O(1)$ time. Thus, the expected runtime is $\Theta(n \log n)$.

**High Probability Bound (w.h.p.):**
- This problem is equivalent to the Coupon Collector's problem.
- Let $T$ be the total number of tries. It is a standard result that for any constant $c > 0$:
  $$P(T > n \ln n + c n) \le e^{-c}$$
  Choosing $c = lpha \ln n$, we get:
  $$P(T > (lpha + 1) n \ln n) \le n^{-lpha}$$
  Thus, with high probability, the number of tries is $O(n \log n)$.

---

### Part b) [10 Points]
**Proof of Uniformity:**
Let $P = [p_1, \dots, p_n]$ be any specific permutation of $\{1, \dots, n\}$.
- The algorithm assigns the numbers $1, 2, \dots, n$ to slots sequentially.
- When placing $x$, we choose one of the remaining $n - x + 1$ empty slots uniformly at random.
- The probability that $x$ is placed in the exact slot specified by the permutation is exactly $rac{1}{n - x + 1}$.
- Since the choices at each step are independent, the probability of generating the exact permutation $P$ is:
  $$\prod_{x=1}^n rac{1}{n - x + 1} = rac{1}{n} \cdot rac{1}{n-1} \dots rac{1}{1} = rac{1}{n!}$$
- Since this probability is identical for all $n!$ permutations, the output is uniformly distributed. $lacksquare$

---

## Question 3: Minimal Feedback Arc Set (20 Points)

### Algorithm Description
We find a minimal feedback arc set by starting with all edges and greedily trying to add them back if they do not create cycles.
1. Let $A = E$.
2. For each edge $e = (u,v) \in E$:
   - Check if the graph $G' = (V, E \setminus (A \setminus \{e\}))$ contains a directed cycle.
   - We can check for directed cycles in $G'$ by running a DFS and checking for back edges (or trying to find a topological sort) in $O(|V| + |E|)$ time.
   - If $G'$ does **not** contain any cycles, then $e$ is not essential to break cycles. We update $A = A \setminus \{e\}$.
   - If $G'$ contains a cycle, we keep $e$ in $A$.
3. Return $A$.

### Proof of Correctness
- **Is a Feedback Arc Set:** Throughout the algorithm, we only remove an edge $e$ from $A$ if the remaining graph $(V, E \setminus A)$ remains acyclic. Thus, the final set $A$ is guaranteed to be a feedback arc set.
- **Is Minimal:** Suppose there exists some proper subset $B \subset A$ that is also a feedback arc set. Then there is some edge $e \in A \setminus B$. Since $B \subset A \setminus \{e\}$, the graph $(V, E \setminus B)$ being acyclic implies that $(V, E \setminus (A \setminus \{e\}))$ must also be acyclic. But in our algorithm, we only kept $e$ in $A$ because removing it would create a cycle. This is a contradiction. Thus, no proper subset of $A$ can be a feedback arc set, so $A$ is minimal.

### Complexity Analysis
- We loop over all $|E|$ edges.
- In each iteration, we run a cycle detection algorithm (DFS) on a graph of size $|V|$ and $|E|$ which takes $O(|V| + |E|)$ time.
- **Total Time Complexity:** $O(|E|(|V| + |E|))$ time.
- **Total Space Complexity:** $O(|V| + |E|)$ to store the graph.

---

## Question 4: Forest and Tree Orientation (20 Points)

### Part a) [10 Points]
**Proof:**
1. Let $G = (V, E)$ be any orientation of the forest. The sum of in-degrees is equal to the sum of out-degrees, which is equal to the number of edges:
   $$\sum_{u \in V} in\_deg(u) = \sum_{u \in V} out\_deg(u) = |E|$$
2. Thus:
   $$\sum_{u \in V} (in\_deg(u) - out\_deg(u)) = 0$$
3. Let $D(u) = in\_deg(u) - out\_deg(u)$. The sum of $D(u)$ is 0.
4. If $|E| \ge 1$, the forest contains at least one connected component with at least one edge. Let $T$ be a tree in the forest with $|V_T|$ vertices and $|E_T| = |V_T| - 1$ edges.
5. In any orientation, the sum of degrees of vertices in $T$ satisfies $\sum_{u \in V_T} (in\_deg_T(u) + out\_deg_T(u)) = 2|E_T| = 2|V_T| - 2$.
6. This means the average degree is $2 - 2/|V_T| < 2$. Since the degrees are integers, there must be at least one leaf vertex $v$ in $T$ with degree exactly 1.
7. In the oriented graph, this leaf $v$ must have either $in\_deg(v) = 1, out\_deg(v) = 0$ or $in\_deg(v) = 0, out\_deg(v) = 1$.
8. In either case, $|in\_deg(v) - out\_deg(v)| = 1 \ge 1$.
Thus, there always exists at least one vertex with a difference of at least 1. $lacksquare$

---

### Part b) [10 Points]
**Algorithm Description:**
We orient the tree bottom-up from the leaves.
1. Choose an arbitrary vertex $r \in V$ as the root of the tree $T$ and orient all edges to/from children bottom-up.
2. Specifically, run a post-order traversal (DFS) starting at $r$.
3. For each vertex $u$ processed:
   - For each child $v$ of $u$:
     - If the difference $in\_deg(v) - out\_deg(v)$ is $+1$, we must orient the edge between $u$ and $v$ as $v ightarrow u$ to keep $v$'s difference at 0.
     - If the difference is $-1$, we orient it as $u ightarrow v$.
     - If the difference is $0$, we can choose either direction arbitrarily (e.g. $u ightarrow v$).
4. Return the oriented tree.

**Correctness:**
By induction, every child $v$ has its final in-degree and out-degree set before its parent edge is oriented. Since the parent edge is directed to balance $v$'s degrees, $v$'s final difference is always at most 1. The root $r$ has no parent edge, but since the sum of differences over all nodes is 0, and all other nodes have difference $\le 1$, the root's difference must also be at most 1.

**Complexity:**
- **Time Complexity:** $O(|V|)$ as we visit each node and edge once during DFS.
- **Space Complexity:** $O(|V|)$ for the recursion stack.

---

## Question 5: Longest Complementary-Reverse Subsequence (20 Points)

### Recursive Formula
Let $DP[i, j]$ be the length of the longest complementary-reverse symmetric subsequence of the substring $s_i \dots s_j$.
- **Base Cases:**
  - $DP[i, j] = 0$ for all $i \ge j$.
- **Recursive Step:**
  For $i < j$:
  - If $s_i$ and $s_j$ are complements of each other (i.e. $(s_i, s_j) \in \{AU, UA, CG, GC\}$):
    $$DP[i, j] = 2 + DP[i+1, j-1]$$
  - Otherwise, we must drop either $s_i$ or $s_j$:
    $$DP[i, j] = \max(DP[i+1, j], DP[i, j-1])$$

### Correctness Explanation
A complementary-reverse symmetric subsequence must have its first and last characters be complements of each other. If $s_i$ and $s_j$ are complements, they can match, and we add 2 to the optimal solution of the inner string $s_{i+1} \dots s_{j-1}$. Otherwise, they cannot both be matched as the outermost pair, so we take the maximum of the subproblems where we exclude either $s_i$ or $s_j$.

### DP Complexity
- **Time Complexity:** $O(n^2)$ cells, each computed in $O(1)$ time. Thus, the total time complexity is $O(n^2)$.
- **Space Complexity:** $O(n^2)$ to store the DP table.
