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
An edge $e = (u,v)$ is in every min cut if and only if increasing its capacity by any $\epsilon > 0$ increases the maximum flow value.
1. Run a max-flow algorithm (e.g. Dinic's) to find the max flow value $F$ on $G$.
2. Increase the capacity of $e$ by 1: $c'(e) = c(e) + 1$.
3. Run the max-flow algorithm on the modified network to find the new max flow value $F'$.
4. If $F' == F + 1$, return `True`; otherwise, return `False`.

**Proof of Correctness:**
By the Max-Flow Min-Cut Theorem, the value of the max flow equals the capacity of the min cut. If $e$ is in *every* min cut, then every min cut must contain $e$. Thus, increasing $c(e)$ by 1 increases the capacity of *every* min cut by 1. Consequently, the minimum cut capacity (and thus the max flow) must increase by exactly 1.
If there is even one min cut $(S, T)$ that does not contain $e$, then the capacity of $(S, T)$ does not change when we increase $c(e)$. Thus, the max flow value will remain $F$, so $F' == F$.

**Complexity:**
- We run the max-flow algorithm twice.
- Dinic's algorithm takes $O(|V|^2 |E|)$ time.
- **Total Time Complexity:** $O(|V|^2 |E|)$ time.
- **Total Space Complexity:** $O(|V| + |E|)$.

---

### Part c) [8 Points]
**Algorithm Description:**
An edge $e = (u,v)$ is in no min cut if and only if decreasing its capacity by a tiny amount $\epsilon > 0$ does not change the maximum flow value.
1. Run a max-flow algorithm to find the max flow value $F$ on $G$.
2. Check if $e$ is saturated (i.e. $f^*(e) == c(e)$). If $e$ is not saturated, then $e$ cannot belong to any min cut, so return `True`.
3. If $e$ is saturated, we temporarily decrease its capacity: $c'(e) = c(e) - \epsilon$ (where $\epsilon = 0.5$ if capacities are integers).
4. Run the max flow on this modified network to get $F'$.
5. If $F' == F$, return `True`; otherwise, return `False`.

**Proof of Correctness:**
If $e$ is in some min cut $(S, T)$, then $e$ must cross the cut from $S$ to $T$. By definition, any edge crossing a min cut must be saturated, and decreasing its capacity by $\epsilon$ must decrease the cut's capacity (and thus the max flow) by $\epsilon$. If decreasing $c(e)$ does not change the max flow, then no min cut contains $e$.

**Complexity:**
- **Total Time Complexity:** $O(|V|^2 |E|)$ time.
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
- Running SSSP from all vertices using Bellman-Ford takes $|V| 	imes O(|V'||E'|) = O(|V|^2 |E|)$ time.
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
- Suppose we are at an odd number $n$. The only way to reach $n$ in the forward direction is by adding 1 to $n-1$, because we cannot reach an odd number by multiplying by 2. Thus, the step $n ightarrow n-1$ is forced.
- Suppose we are at an even number $n$. We can either divide by 2 ($n ightarrow n/2$) or subtract 1 ($n ightarrow n-1$).
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
   - If $f(u, v_i) == 	ext{False}$, return `False` (disconnected).
4. If all 8 queries return `True`, return `True` (connected).

**Proof of Correctness:**
- If $G$ is connected, the algorithm always returns `True` (100% correct).
- If $G$ is disconnected, by the problem statement, the largest connected component has size at most $|V|/4$.
  - The probability that a randomly chosen vertex $v_i$ belongs to the same component as $u$ is at most $1/4$.
  - The probability that all 8 vertices $v_1, \dots, v_8$ belong to the same component as $u$ is:
    $$P(	ext{False Positive}) \le \left(rac{1}{4}ight)^8 = rac{1}{65536} < rac{1}{4}$$
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
  $$P(	ext{Failure}) \le \left(rac{1}{4}ight)^{C \log |V|} = |V|^{-2C} \le |V|^{-c}$$
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
  - $DP[i, 0] = -0.5 	imes i$ (aligning $S[1 \dots i]$ with $i$ gaps).
  - $DP[0, j] = -0.5 	imes j$ (aligning $T[1 \dots j]$ with $j$ gaps).
- **Recursive Step:**
  For $i > 0$ and $j > 0$:
  $$DP[i, j] = \max egin{cases} 
    DP[i-1, j-1] + 1 & 	ext{if } s_i == t_j 	ext{ (Match)} \
    DP[i-1, j-1] - 1 & 	ext{if } s_i 
e t_j 	ext{ (Mismatch)} \
    DP[i-1, j] - 0.5 & 	ext{Gap in } T \
    DP[i, j-1] - 0.5 & 	ext{Gap in } S 
  \end{cases}$$

### DP Complexity
- **Time Complexity:** $O(n m)$ cells, each taking $O(1)$ time to compute. Thus, the total time complexity is $O(n m)$.
- **Space Complexity:** $O(n m)$ space to store the DP table.
