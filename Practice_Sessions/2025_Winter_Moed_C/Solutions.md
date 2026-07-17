# Algorithms 1 (89-220) - 2025 Winter Moed C Solutions Manual

This document contains the official, rigorous solutions for the 2025 Winter Moed C exam questions.

---

## Question 1: Flow Networks & Min Cut Verifications (20 Points)

### Part a) [5 Points]
A cut $(S, T)$ of a flow network $G = (V,E)$ with source $s$ and sink $t$ is a partition of $V$ into two sets $S$ and $T = V \setminus S$ such that $s \in S$ and $t \in T$.
The capacity of the cut, denoted by $c(S, T)$, is the sum of capacities of edges going from $S$ to $T$:
$$c(S, T) = \sum_{u \in S, v \in T} c(u,v)$$
A **minimum $s-t$ cut** is a cut whose capacity is minimized over all valid cuts.

---

### Part b) [7 Points]
**Algorithm Description:**
We can decide this using a single run of the max-flow algorithm by analyzing reachability in the final residual graph $G_{f^*}$:
1. Compute the maximum flow $f^*$ on $G$ using Dinic's algorithm.
2. Build the residual graph $G_{f^*}$.
3. Find $S^*$, the set of all vertices reachable from $s$ in $G_{f^*}$ (using BFS/DFS starting from $s$).
4. Find $T^*$, the set of all vertices that can reach $t$ in $G_{f^*}$ (using BFS/DFS starting from $t$ on the reversed residual graph $G_{f^*}^{rev}$).
5. For the designated edge $e = (u,v)$, check if:
   $$u \in S^* \quad \text{and} \quad v \in T^*$$
6. If both conditions are satisfied, return `True`; otherwise, return `False`.

**Proof of Correctness:**
- By the Max-Flow Min-Cut Theorem, any minimum $s-t$ cut $(S, T)$ must satisfy $S^* \subseteq S$ and $T^* \subseteq T$.
- **Direction 1 (If):** If $u \in S^*$ and $v \in T^*$, then for *any* minimum cut $(S, T)$, we must have $u \in S$ and $v \in T$. Since $u \in S$ and $v \in T$, the edge $e = (u,v)$ crosses the cut from $S$ to $T$. Thus, $e$ is in every minimum cut.
- **Direction 2 (Only if):** Suppose $u \notin S^*$. We can define the cut $(S, T) = (S^*, V \setminus S^*)$, which is a minimum cut. Since $u \notin S^*$, we have $u \in T$, so $e = (u,v)$ does not cross this cut from $S$ to $T$. Thus, $e$ is not in this minimum cut, meaning it does not belong to *every* minimum cut. A symmetric argument holds if $v \notin T^*$ using the minimum cut $(V \setminus T^*, T^*)$.

**Complexity:**
- Running Dinic's algorithm takes $O(|V|^2 |E|)$ time.
- The BFS/DFS runs on $G_{f^*}$ and $G_{f^*}^{rev}$ take $O(|V| + |E|)$ time.
- **Total Time Complexity:** $O(|V|^2 |E|)$ time (requires only a single max-flow execution).
- **Total Space Complexity:** $O(|V| + |E|)$.

---

### Part c) [8 Points]
**Algorithm Description:**
An edge $e = (u,v)$ belongs to no minimum $s-t$ cut if and only if it cannot cross any minimum cut from the $S$-side to the $T$-side. This holds if and only if at least one of the following conditions is satisfied:
1. $e$ is not saturated in the max flow $f^*$ (i.e., $f^*(e) < c(e)$).
2. $v$ is reachable from $s$ in the residual graph $G_{f^*}$.
3. $t$ is reachable from $u$ in the residual graph $G_{f^*}$.
4. $u$ is reachable from $v$ in the residual graph $G_{f^*}$.

**Algorithm Steps:**
1. Compute the maximum flow $f^*$ on $G$ using Dinic's algorithm.
2. Build the residual graph $G_{f^*}$.
3. If $f^*(e) < c(e)$, return `True`.
4. Run a BFS/DFS starting from $s$ in $G_{f^*}$ to find $S^*$ (the set of reachable vertices from $s$). If $v \in S^*$, return `True`.
5. Run a BFS/DFS starting from $u$ in $G_{f^*}$. If $t$ is reachable from $u$, return `True`.
6. Run a BFS/DFS starting from $v$ in $G_{f^*}$. If $u$ is reachable from $v$, return `True`.
7. Otherwise, return `False`.

**Proof of Correctness:**
By Picard's characterization of minimum cuts, an edge $e = (u,v)$ belongs to **at least one** minimum cut $(S, T)$ (with $u \in S$ and $v \in T$) if and only if:
- $e$ is saturated (if $e$ is not saturated, $u \rightarrow v$ exists in $G_{f^*}$, so it cannot cross any minimum cut since a min cut cannot contain edges with positive residual capacity).
- $v \notin S^*$ (if $v \in S^*$, then since $S^* \subseteq S$ for all min cuts, we must have $v \in S$, preventing $v$ from being on the $T$-side).
- $u \notin T^*$ (if $u \in T^*$, then since $T^* \subseteq T$ for all min cuts, we must have $u \in T$, preventing $u$ from being on the $S$-side).
- $u$ is not reachable from $v$ in $G_{f^*}$ (if there is a path $v \rightarrow u$ in $G_{f^*}$, then we cannot have $u \in S$ and $v \in T$ because a path in $G_{f^*}$ cannot cross from $T$ to $S$ since all backward edges of the min cut are saturated, i.e., have capacity 0 in the opposite direction).

Since $e$ is in no minimum cut if and only if the negation of the above four conditions holds, $e$ is in no min cut if and only if $e$ is not saturated, or $v \in S^*$, or $u \in T^*$, or $u$ is reachable from $v$ in $G_{f^*}$.

**Complexity:**
- Dinic's algorithm takes $O(|V|^2 |E|)$ time.
- The three BFS/DFS searches take $O(|V| + |E|)$ time.
- **Total Time Complexity:** $O(|V|^2 |E|)$ time (requires only a single max-flow execution).
- **Total Space Complexity:** $O(|V| + |E|)$.

---

## Question 2: Shortest Paths with Even Number of Edges (20 Points)

### Algorithm Description
We solve this by constructing a bipartite representation of the graph (a layer graph).
1. Construct a new graph $G' = (V', E')$ where:
   - For each vertex $v \in V$, we create two vertices in $V'$: $v_{even}$ and $v_{odd}$.
   - For each directed edge $(u,v) \in E$ with weight $w(u,v)$:
     - Add edge $(u_{even}, v_{odd})$ with weight $w(u,v)$.
     - Add edge $(u_{odd}, v_{even})$ with weight $w(u,v)$.
2. The number of vertices in $G'$ is $2|V|$ and the number of edges is $2|E|$. Since $G$ has no negative cycles, $G'$ also has no negative cycles.
3. To find the shortest path from each starting node $u$ with an even number of edges, we run the **Bellman-Ford** algorithm on the entire graph $G'$ (or run it once from a virtual source to find all pairs, or run SSSP from each node).
4. Specifically, for SSSP from a vertex $u$: the shortest path to $v$ with an even number of edges is the shortest path from $u_{even}$ to $v_{even}$ in $G'$.

**Correctness:**
Any path in $G'$ starting at $u_{even}$ alternates between even and odd layers. Thus, a path reaching $v_{even}$ must contain an even number of edges. Since the edge weights are preserved, the shortest path from $u_{even}$ to $v_{even}$ in $G'$ corresponds exactly to the shortest path from $u$ to $v$ in $G$ with an even number of edges.

**Complexity Analysis:**
- Running SSSP from all vertices using Bellman-Ford takes $|V| \times O(|V'||E'|) = O(|V|^2 |E|)$ time.
- If edge weights are non-negative, we can use Dijkstra's algorithm from each node which takes $O(|V||E| + |V|^2 \log |V|)$ time.

---

## Question 3: Minimum Steps to Reach $n$ (20 Points)

### Algorithm Description
We solve this problem greedily by working backwards from $n$ to 1:
1. Initialize `steps = 0`.
2. While $n > 1$:
   - If $n$ is even, divide $n$ by 2: $n = n / 2$.
   - If $n$ is odd, subtract 1: $n = n - 1$.
   - Increment `steps` by 1.
3. Return `steps`.

### Proof of Correctness
Working backwards, the operations are:
1. Divide by 2 (if even).
2. Subtract 1.
- Suppose we are at an odd number $n$. The only way to reach $n$ in the forward direction is by adding 1 to $n-1$, because we cannot reach an odd number by multiplying by 2. Thus, the step $n \rightarrow n-1$ is forced.
- Suppose we are at an even number $n$. We can either divide by 2 ($n \rightarrow n/2$) or subtract 1 ($n \rightarrow n-1$).
  - If we subtract 1, we get an odd number $n-1$, which forces us to subtract 1 again to get $n-2$. This takes 2 steps to reach $n-2$.
  - If we divide by 2, we reach $n/2$ in 1 step. From $n/2$, we can reach $n-2$ in at most 1 step if we add 1 and then multiply by 2 (which is equivalent). In all cases, dividing by 2 is always optimal or better because it reduces the number much faster.

### Complexity Analysis
- In each step, if $n$ is odd, it becomes even in 1 step. If $n$ is even, it is halved.
- Thus, the number is halved at least every 2 steps.
- The number of steps is at most $2 \log_2 n \in O(\log n)$.
- **Total Time Complexity:** $O(\log n)$ time.
- **Total Space Complexity:** $O(1)$ space.

---

## Question 4: Randomized Connectivity Tester (20 Points)

### Part a) [10 Points]
**Algorithm Description:**
1. Choose a vertex $u \in V$ uniformly at random.
2. Choose 8 vertices $v_1, \dots, v_8$ independently and uniformly at random from $V$.
3. For each $v_i$:
   - Query $f(u, v_i)$.
   - If $f(u, v_i) == \text{False}$, return `False` (disconnected).
4. If all 8 queries return `True`, return `True` (connected).

**Proof of Correctness:**
- If $G$ is connected, the algorithm always returns `True` (100% correct).
- If $G$ is disconnected, by the problem statement, the largest connected component has size at most $|V|/4$.
  - The probability that a randomly chosen vertex $v_i$ belongs to the same component as $u$ is at most $1/4$.
  - The probability that all 8 vertices $v_1, \dots, v_8$ belong to the same component as $u$ is:
    $$P(\text{False Positive}) \le \left(\frac{1}{4}\right)^8 = \frac{1}{65536} < \frac{1}{4}$$
  - Thus, the success probability of detecting that the graph is disconnected is at least $1 - 1/65536 > 3/4$.

**Complexity:**
- We perform exactly 8 queries, each taking $O(1)$ time.
- **Total Time Complexity:** $O(1)$ time.
- **Total Space Complexity:** $O(1)$ space.

---

### Part b) [10 Points]
**Algorithm Description:**
To succeed with high probability, we increase the number of random samples:
1. Choose a vertex $u \in V$ uniformly at random.
2. Choose $C \log |V|$ vertices $v_1, v_2, \dots$ uniformly at random from $V$ (for some constant $C \ge 2$).
3. For each $v_i$, query $f(u, v_i)$. If any query returns `False`, return `False`.
4. If all queries return `True`, return `True`.

**Proof of Correctness:**
- If $G$ is connected, it always returns `True`.
- If $G$ is disconnected, the probability of failing to find a disconnected vertex is:
  $$P(\text{Failure}) \le \left(\frac{1}{4}\right)^{C \log |V|} = |V|^{-2C} \le |V|^{-c}$$
  This is less than $1/|V|^c$ for any desired constant $c$. Thus, the algorithm succeeds with high probability.

**Complexity:**
- **Total Time Complexity:** $O(\log |V|)$ time.
- **Total Space Complexity:** $O(1)$ space.

---

## Question 5: Edit Distance with Custom Gap Penalty (20 Points)

### Recursive Formula
Let $DP[i, j]$ be the maximum global alignment score between prefixes $S[1 \dots i]$ and $T[1 \dots j]$.
- **Base Cases:**
  - $DP[0, 0] = 0$.
  - $DP[i, 0] = -0.5 \times i$ (aligning $S[1 \dots i]$ with $i$ gaps).
  - $DP[0, j] = -0.5 \times j$ (aligning $T[1 \dots j]$ with $j$ gaps).
- **Recursive Step:**
  For $i > 0$ and $j > 0$:
  $$DP[i, j] = \max \begin{cases} 
    DP[i-1, j-1] + 1 & \text{if } s_i == t_j \text{ (Match)} \\
    DP[i-1, j-1] - 1 & \text{if } s_i \ne t_j \text{ (Mismatch)} \\
    DP[i-1, j] - 0.5 & \text{Gap in } T \\
    DP[i, j-1] - 0.5 & \text{Gap in } S 
  \end{cases}$$

### DP Complexity
- **Time Complexity:** $O(n m)$ cells, each taking $O(1)$ time to compute. Thus, the total time complexity is $O(n m)$.
- **Space Complexity:** $O(n m)$ space to store the DP table.
