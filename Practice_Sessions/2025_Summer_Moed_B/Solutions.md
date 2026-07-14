# Algorithms 1 (89-220) - 2025 Summer Moed B Solutions Manual

This document contains the official, rigorous solutions for the 2025 Summer Moed B exam questions.

---

## Question 1: Vertex Capacities & Smallest Edge-Count Min Cut (25 Points)

### Part a) [13 Points]
**Reduction to Standard Max Flow:**
To enforce vertex capacities, we split each vertex $v \in V \setminus \{s,t\}$ into two vertices: $v_{in}$ and $v_{out}$.
1. Construct a new flow network $G' = (V', E')$:
   - For each $v \in V \setminus \{s,t\}$, add $v_{in}$ and $v_{out}$ to $V'$, and add a directed edge $(v_{in}, v_{out})$ with capacity $l(v)$.
   - Add $s$ and $t$ directly to $V'$.
   - For each directed edge $(u, v) \in E$ in $G$:
     - If $u = s$, add $(s, v_{in})$ with capacity $c(s,v)$.
     - If $v = t$, add $(u_{out}, t)$ with capacity $c(u,t)$.
     - If both are normal nodes, add $(u_{out}, v_{in})$ with capacity $c(u,v)$.
2. Run Dinic's algorithm on $G'$. The max flow value in $G'$ is the max flow under vertex capacities.

**Correctness:**
Any flow passing through $v$ in $G$ must enter $v_{in}$, go through edge $(v_{in}, v_{out})$, and leave via $v_{out}$. Since the capacity of $(v_{in}, v_{out})$ is capped at $l(v)$, this correctly enforces that at most $l(v)$ flow passes through $v$.

**Complexity:**
- $|V'| \le 2|V|$, $|E'| \le |E| + |V|$.
- Dinic's algorithm runs in $O(|V'|^2 |E'|) = O(|V|^2 |E|)$ time.

---

### Part b) [12 Points]
**Algorithm Description:**
We modify the edge capacities to penalize cuts with more edges.
1. Let $L$ be a very large number, e.g. $L = |E| + 1$.
2. Define a new capacity function $c'(e) = L \cdot c(e) + 1$ for each $e \in E$.
3. Find a minimum $s-t$ cut $(S, T)$ in $G$ using the capacity function $c'$.
4. Return $(S, T)$.

**Proof of Correctness:**
The capacity of any cut $(S, T)$ under $c'$ is:
$$c'(S, T) = \sum_{e \in 	ext{cut}} (L \cdot c(e) + 1) = L \cdot c(S, T) + k$$
where $k$ is the number of edges crossing the cut.
Since $k \le |E| < L$, the term $L \cdot c(S, T)$ dominates. A cut minimizes $c'(S, T)$ if and only if it first minimizes $c(S, T)$ (meaning it is a min cut), and then minimizes $k$ (the number of edges).

---

## Question 2: Dynamic MST Edge Weight Decrease (20 Points)

### Algorithm Description
1. Add $e_0 = (u,v)$ to the MST $T$. This creates a unique cycle $C$ in $T \cup \{e_0\}$.
2. Find the heaviest edge $e_{max}$ on the path between $u$ and $v$ in $T$ using BFS/DFS.
3. If $w'(e_0) < w(e_{max})$:
   - The new MST is $T' = T \cup \{e_0\} \setminus \{e_{max}\}$.
4. Otherwise, the MST remains unchanged: $T' = T$.
5. Return $T'$.

### Proof of Correctness
By the Cycle Property, for any cycle, the heaviest edge cannot belong to any MST. When we add $e_0$ to $T$, we form a cycle. The heaviest edge on this cycle is $e_{max}$ (since $w'(e_0) < w(e_{max})$). Thus, removing $e_{max}$ yields the optimal spanning tree.

### Complexity Analysis
- Finding the path between $u$ and $v$ in the tree $T$ takes $O(|V|)$ time using BFS or DFS.
- Modifying the tree takes $O(1)$ time.
- **Total Time Complexity:** $O(|V|)$ time.

---

## Question 3: Bottleneck Paths in a DAG (15 Points)

### Algorithm Description
We solve this using Dynamic Programming on the topological sort of the DAG.
1. Perform a topological sort of $G$ in $O(|V| + |E|)$ time.
2. Initialize an array $DP[u] = \infty$ for all $u \in V$. Set $DP[s] = 0$.
3. For each vertex $u$ in topological order starting from $s$:
   - For each outgoing edge $(u, v)$ with weight $w(u,v)$:
     - $DP[v] = \min(DP[v], \max(DP[u], w(u,v)))$.
4. Return $DP[t]$.

### Proof of Correctness
The subproblem $DP[v]$ is the minimum bottleneck value from $s$ to $v$. Since the graph is a DAG, we can compute the values in topological order, ensuring that when we process $u$, $DP[u]$ has already been computed optimally.

### Complexity Analysis
- **Time Complexity:** $O(|V| + |E|)$ to sort and relax all edges.
- **Space Complexity:** $O(|V|)$ to store the DP table.

---

## Question 4: Dynamic Programming Egg Dropping (20 Points)

### Recursive Formula
Let $DP[i, j]$ be the minimum number of drops needed to find the critical floor using $i$ eggs and $j$ floors.
- **Base Cases:**
  - $DP[1, j] = j$ (with 1 egg, we must test floor-by-floor from 1 to $j$).
  - $DP[i, 0] = 0$ (0 floors require 0 drops).
  - $DP[i, 1] = 1$ (1 floor requires 1 drop).
- **Recursive Step:**
  If we drop an egg from floor $k$ ($1 \le k \le j$):
  - If it breaks: we have $i-1$ eggs and must search the $k-1$ floors below.
  - If it survives: we have $i$ eggs and must search the $j-k$ floors above.
  Since we want the worst-case, we take the maximum, and we choose $k$ to minimize this:
  $$DP[i, j] = 1 + \min_{1 \le k \le j} \max(DP[i-1, k-1], DP[i, j-k])$$

### Complexity
- **Time Complexity:** $O(m \cdot n^2)$ using nested loops.
- **Space Complexity:** $O(m \cdot n)$ to store the table.

---

## Question 5: Expected Value in Secretary Problems (20 Points)

### Part a) [10 Points]
Let $X_i$ be the indicator random variable that the $i$-th candidate is hired.
- The $i$-th candidate is hired if and only if she is the best among the first $i$ candidates.
- Since candidates arrive in a uniform random permutation, the probability that the $i$-th candidate is the best among the first $i$ is exactly $1/i$.
- Thus, $E[X_i] = 1/i$.
- The total expected number of hirings/replacements is:
  $$E[X] = \sum_{i=1}^N E[X_i] = \sum_{i=1}^N rac{1}{i} = H_N pprox \ln N + \gamma$$

---

### Part b) [10 Points]
For $K$ positions, a candidate is hired if and only if she is among the top $K$ of the first $i$ candidates.
- For $i \le K$, every candidate is hired (so $E[X_i] = 1$ for $i \le K$).
- For $i > K$, the probability that the $i$-th candidate is among the top $K$ of the first $i$ is exactly $K/i$.
- The total expected number of replacements is:
  $$E[X] = \sum_{i=1}^K 1 + \sum_{i=K+1}^N rac{K}{i} = K + K \sum_{i=K+1}^N rac{1}{i} = K \left( 1 + H_N - H_K ight) pprox K \left(1 + \lnrac{N}{K}ight)$$
