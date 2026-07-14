# Algorithms 1 (89-220) - 2026 Moed A Solutions Manual

This document contains the official, rigorous solutions for the 2026 Moed A exam questions.

---

## Question 1: A* Search & Potential Functions (20 Points)

### Part a) [6 Points]
**Definition of a Feasible Potential Function:**
A potential function $p: V \rightarrow \mathbb{R}$ is feasible (or consistent/monotone) with respect to an edge weight function $w: E \rightarrow \mathbb{R}^+$ if for every directed edge $(u,v) \in E$, the potential-adjusted weight (or reduced cost) $\hat{w}(u,v)$ is non-negative:
$$\hat{w}(u,v) = w(u,v) - p(u) + p(v) \ge 0 \quad \forall (u,v) \in E$$
Which is equivalent to:
$$p(u) - p(v) \le w(u,v) \quad \forall (u,v) \in E$$
Furthermore, in the context of $A^*$ search terminating at target $t$, we typically require $p(t) = 0$.

---

### Part b) [7 Points]
**Proposition:** The potential function $p_{1}(u)=\max_{v_{i}\in S}\{\delta(u,v_{i})-\delta(t,v_{i})\}$ is feasible.
**Proof:**
1. Let $(u,v) \in E$ be an arbitrary directed edge. By definition of the shortest path distance function $\delta$, the triangle inequality holds in $G$:
   $$\delta(u, v_i) \le \delta(u, v) + \delta(v, v_i) \le w(u,v) + \delta(v, v_i) \quad \forall v_i \in S$$
2. Subtract $\delta(t, v_i)$ from both sides:
   $$\delta(u, v_i) - \delta(t, v_i) \le w(u,v) + \delta(v, v_i) - \delta(t, v_i) \quad \forall v_i \in S$$
3. Since this inequality holds for every $v_i \in S$, it must also hold for the maximum value over all $v_i \in S$:
   $$\max_{v_i \in S} \{\delta(u, v_i) - \delta(t, v_i)\} \le \max_{v_i \in S} \{w(u,v) + \delta(v, v_i) - \delta(t, v_i)\}$$
4. Since $w(u,v)$ is a constant scalar independent of the variable $v_i$ being maximized, we can factor it out:
   $$\max_{v_i \in S} \{\delta(u, v_i) - \delta(t, v_i)\} \le w(u,v) + \max_{v_i \in S} \{\delta(v, v_i) - \delta(t, v_i)\}$$
5. By substituting the definition of $p_1$:
   $$p_1(u) \le w(u,v) + p_1(v) \implies w(u,v) - p_1(u) + p_1(v) \ge 0$$
Thus, $\hat{w}_{p_1}(u,v) \ge 0$ for all edges $(u,v) \in E$. The potential function $p_1(u)$ is feasible. $\blacksquare$

---

### Part c) [7 Points]
**Proposition:** The potential function $p_{2}(u)=\min_{v_{i}\in S}\{\delta(u,v_{i})-\delta(t,v_{i})\}$ is feasible.
**Proof:**
1. Let $(u,v) \in E$ be an arbitrary directed edge. As in part (b), the triangle inequality ensures:
   $$\delta(u, v_i) - \delta(t, v_i) \le w(u,v) + \delta(v, v_i) - \delta(t, v_i) \quad \forall v_i \in S$$
2. Let $f(v_i) = \delta(u, v_i) - \delta(t, v_i)$ and $g(v_i) = w(u,v) + \delta(v, v_i) - \delta(t, v_i)$. Since $f(v_i) \le g(v_i)$ for all $v_i \in S$, the minimum over the set $S$ preserves the inequality:
   $$\min_{v_i \in S} f(v_i) \le \min_{v_i \in S} g(v_i)$$
   $$\min_{v_i \in S} \{\delta(u, v_i) - \delta(t, v_i)\} \le \min_{v_i \in S} \{w(u,v) + \delta(v, v_i) - \delta(t, v_i)\}$$
3. Factor out the constant $w(u,v)$ from the minimization term:
   $$\min_{v_i \in S} \{\delta(u, v_i) - \delta(t, v_i)\} \le w(u,v) + \min_{v_i \in S} \{\delta(v, v_i) - \delta(t, v_i)\}$$
4. Substituting the definition of $p_2$:
   $$p_2(u) \le w(u,v) + p_2(v) \implies w(u,v) - p_2(u) + p_2(v) \ge 0$$
Thus, $\hat{w}_{p_2}(u,v) \ge 0$ for all edges $(u,v) \in E$. The potential function $p_2(u)$ is feasible. $\blacksquare$

---

## Question 2: Degree Sequence Realization (20 Points)

### Algorithm Description
We solve this problem by reduction to the **Maximum Flow** problem on a unit-capacity network.
Given the degree sequence constraints, we construct a flow network $H = (V_H, E_H)$ as follows:
1. **Vertices:**
   - A source vertex $s$ and a sink vertex $t$.
   - For each graph vertex $v_j$ (for $j=1 \dots n$), we create two nodes: $x_j$ (representing the outgoing ports of $v_j$) and $y_j$ (representing the incoming ports of $v_j$).
   - Total number of vertices $|V_H| = 2n + 2$.
2. **Directed Edges and Capacities:**
   - From source $s$ to each out-node $x_j$: add an edge $(s, x_j)$ with capacity $c(s, x_j) = o_j$.
   - From each in-node $y_j$ to sink $t$: add an edge $(y_j, t)$ with capacity $c(y_j, t) = i_j$.
   - For each pair $j, k \in \{1, \dots, n\}$ with $j \ne k$ (to avoid self-loops): add a directed edge $(x_j, y_k)$ with capacity $c(x_j, y_k) = 1$. This represents the potential graph edge $v_j \rightarrow v_k$.
3. **Execution & Reconstruction:**
   - Check if $\sum_{j=1}^n o_j = \sum_{j=1}^n i_j$. If not, return that no such graph exists. Let $C = \sum_{j=1}^n o_j$.
   - Compute the Maximum Flow from $s$ to $t$ on $H$ using **Dinic's Algorithm**.
   - Since all edge capacities are integers, the Max-Flow Integrality Theorem guarantees that there exists an integer maximum flow $f^*$.
   - If the value of the max flow $|f^*| == C$:
     - Construct the directed graph $G = (V, E)$ with $V = \{v_1, \dots, v_n\}$.
     - For each edge $(x_j, y_k)$ in $H$, if the flow $f^*(x_j, y_k) == 1$, add the directed edge $(v_j, v_k)$ to $E$.
     - Return $G$.
   - Else, return that no such graph exists.

### Proof of Correctness
- **Capacity constraints:** The flow on any $(x_j, y_k)$ is either 0 or 1 because its capacity is 1. Thus, at most one edge $v_j \rightarrow v_k$ is created, ensuring no multiple edges. Since $j \neq k$, no self-loop is created. Thus, $G$ is a simple directed graph.
- **Degree satisfaction:**
  - The total flow leaving $x_j$ is $\sum_{k \neq j} f^*(x_j, y_k) = f^*(s, x_j) \le o_j$.
  - The total flow entering $y_k$ is $\sum_{j \neq k} f^*(x_j, y_k) = f^*(y_k, t) \le i_k$.
  - Since the total flow is $C = \sum o_j = \sum i_j$, all source-edges $(s, x_j)$ and sink-edges $(y_k, t)$ must be fully saturated.
  - Therefore, the out-degree of $v_j$ is exactly $o_j$, and the in-degree of $v_k$ is exactly $i_k$.
- **Completeness:** If such a graph $G$ exists, we can set flow $f(x_j, y_k) = 1$ for each edge in $G$, $f(s, x_j) = o_j$, and $f(y_k, t) = i_k$. This flow is valid and has value $C$. Therefore, the max flow in $H$ must be at least $C$. Since the max flow cannot exceed $\sum c(s, x_j) = C$, the max flow is exactly $C$, and Dinic's algorithm will find it.

### Complexity Analysis
- **Network size:** $|V_H| = 2n + 2 \in \Theta(n)$ and $|E_H| = n + n + n(n-1) \in \Theta(n^2)$.
- **Max Flow computation:** Since the capacities on internal edges are all 1, this is a unit-capacity network.
- For unit networks, Dinic's algorithm runs in $O(|E_H| \sqrt{|V_H|})$ time.
- Substituting the sizes: $O(n^2 \sqrt{n}) = O(n^{2.5})$ time.
- **Reconstruction:** Iterating over all $O(n^2)$ internal edges to build the adjacency list takes $O(n^2)$ time.
- **Total Time Complexity:** $O(n^{2.5})$ time.
- **Total Space Complexity:** $O(n^2)$ to store the flow network and the final adjacency list.

---

## Question 3: Triangles Containing Each Edge (20 Points)

### Part a) [12 Points]
**Algorithm for Dense Graphs ($|E| \in \Theta(|V|^2)$):**
1. Let $A$ be the $n \times n$ adjacency matrix of $G$, where $A_{u,v} = 1$ if $(u,v) \in E$ and $0$ otherwise.
2. Compute the matrix product $C = A^2 = A \times A$ using a fast matrix multiplication algorithm (e.g. Strassen's or Coppersmith-Winograd variants) in $O(|V|^\omega)$ time, where $\omega < 2.373$ is the matrix multiplication exponent.
3. For each edge $e = (u,v) \in E$, the number of triangles containing $e$ is exactly $C_{u,v}$.
4. Return a mapping or array where each edge $(u,v) \in E$ is associated with $C_{u,v}$.

**Correctness:**
The entry $C_{u,v}$ in the squared adjacency matrix $A^2$ is defined as:
$$C_{u,v} = \sum_{w \in V} A_{u,w} A_{w,v}$$
Since $A_{u,w} A_{w,v} = 1$ if and only if $A_{u,w}=1$ and $A_{w,v}=1$, each term in the sum is 1 if $w$ is a common neighbor of both $u$ and $v$, and 0 otherwise.
A common neighbor $w$ of $u$ and $v$ forms a triangle $\{u, v, w\}$ in the undirected graph.
Thus, $C_{u,v}$ counts exactly the number of common neighbors, which is the number of triangles containing edge $(u,v)$.

**Complexity:**
- **Matrix Multiplication:** $O(|V|^\omega)$ time.
- **Edge Queries:** $O(|E|)$ to read the values for each edge. Since $|E| \le |V|^2 \in O(|V|^\omega)$, this is dominated by the multiplication step.
- **Total Time Complexity:** $O(|V|^\omega)$ time.
- **Total Space Complexity:** $O(|V|^2)$ to store the adjacency matrix and its square.

---

### Part b) [8 Points]
**Algorithm for Sparse Graphs ($|E| \in O(|V|^{1.5})$):**
We use the **Heavy/Light Vertex Partitioning** method (or direct edge orientation) to count triangles in $O(|E|^{1.5})$ time.
1. Direct the undirected graph $G$ to obtain a directed acyclic graph $D = (V, E_D)$ as follows:
   For each undirected edge $\{u,v\} \in E$:
   - Direct it as $u \rightarrow v$ if $\text{deg}(u) < \text{deg}(v)$, or if $\text{deg}(u) == \text{deg}(v)$ and $u < v$ (lexicographically).
2. For each vertex $u \in V$, let $N^+(u) = \{v \in V \mid u \rightarrow v \in E_D\}$ be its set of out-neighbors.
3. Initialize a global hash map or list of counters `TriangleCount[e] = 0` for each undirected edge $e \in E$.
4. For each vertex $u \in V$:
   - Mark all out-neighbors $w \in N^+(u)$ in a temporary boolean array/set.
   - For each out-neighbor $v \in N^+(u)$:
     - For each out-neighbor $w \in N^+(v)$:
       - If $w$ is marked as an out-neighbor of $u$:
         - We have found a triangle $\{u, v, w\}$ corresponding to directed edges $u \rightarrow v$, $v \rightarrow w$, and $u \rightarrow w$.
         - Increment the counters for the three corresponding undirected edges: $\{u,v\}$, $\{v,w\}$, and $\{u,w\}$.
   - Unmark all out-neighbors $w \in N^+(u)$.
5. Return the counts.

**Correctness:**
Every triangle $\{u, v, w\}$ in $G$ has a unique directed representation in the DAG $D$. Because the vertices are totally ordered by degree and ID, the directed edges between them must follow this topological order. Without loss of generality, let $u \rightarrow v$, $v \rightarrow w$, and $u \rightarrow w$ be the directed edges.
The algorithm will find this triangle exactly once: when it processes $u$ (which has out-edges to $v$ and $w$), it sees that $v$ has an out-edge to $w$, and since $w$ is marked as an out-neighbor of $u$, the triangle is registered.
Since each triangle is counted exactly once, and all three participating edges have their counters incremented by 1, the final counts are correct.

**Complexity Analysis:**
- **Out-Degree Lemma:** For any vertex $u \in V$, its out-degree in $D$ is $\text{deg}^+(u) \in O(\sqrt{|E|})$.
  - *Proof:* Let $v \in N^+(u)$. By construction, $\text{deg}(v) \ge \text{deg}(u)$. If $\text{deg}(u) < \sqrt{|E|}$, then obviously $\text{deg}^+(u) \le \text{deg}(u) < \sqrt{|E|}$. If $\text{deg}(u) \ge \sqrt{|E|}$, then any out-neighbor $v$ must also have degree $\ge \sqrt{|E|}$. Since the sum of all degrees is $2|E|$, there can be at most $2|E|/\sqrt{|E|} = 2\sqrt{|E|}$ vertices of degree $\ge \sqrt{|E|}$. Thus $\text{deg}^+(u) \le 2\sqrt{|E|}$. In both cases, $\text{deg}^+(u) \in O(\sqrt{|E|})$.
- **Runtime:**
  - Directing edges: $O(|V| + |E|)$ time.
  - The nested loops execute $\sum_{u \in V} \sum_{v \in N^+(u)} \text{deg}^+(v)$ operations.
  - Since $\text{deg}^+(v) \in O(\sqrt{|E|})$, the sum is bounded by:
    $$\sum_{u \in V} \sum_{v \in N^+(u)} O(\sqrt{|E|}) = O(\sqrt{|E|}) \sum_{u \in V} \text{deg}^+(u) = O(|E| \sqrt{|E|}) = O(|E|^{1.5})$$
  - Since $|E| \in O(|V|^{1.5})$, the time complexity is:
    $$O((|V|^{1.5})^{1.5}) = O(|V|^{2.25})$$
    Which is faster than dense matrix multiplication ($O(|V|^{2.37})$) for sparse graphs.
- **Total Time Complexity:** $O(|E|^{1.5})$ or $O(|V|^{2.25})$ time.
- **Total Space Complexity:** $O(|V| + |E|)$ to store the DAG.

---

## Question 4: Minimum Spanning Trees in Complete Bipartite Graphs (20 Points)

### Part a) [7 Points]
**Proof:**
Consider the Cut Property of MSTs: *For any cut $(U, V \setminus U)$ of a connected graph, the minimum-weight edge crossing the cut belongs to some MST. If the minimum-weight edge is unique, it belongs to all MSTs.*
1. Define the cut $(U, V \setminus U)$ where $U = \{l_{min}\}$.
2. The edges crossing this cut are all edges incident to $l_{min}$, which are of the form $(l_{min}, r_j)$ for all $r_j \in R$.
3. The weight of any such edge is:
   $$w(l_{min}, r_j) = a_{min} + b_j$$
4. Since $b_{min} = \min_{j} \{b_j\}$ and all $b_j$ are distinct, we have $b_j > b_{min}$ for all $r_j \ne r_{min}$. Thus:
   $$w(l_{min}, r_{min}) = a_{min} + b_{min} < a_{min} + b_j = w(l_{min}, r_j) \quad \forall r_j \ne r_{min}$$
5. Therefore, $(l_{min}, r_{min})$ is the unique minimum-weight edge crossing the cut $(\{l_{min}\}, (L \cup R) \setminus \{l_{min}\})$.
By the Cut Property, $(l_{min}, r_{min})$ must belong to every MST $T = (L \cup R, E_T)$ of $G$. $\blacksquare$

---

### Part b) [8 Points]
**Proof:**
Consider the Cycle Property of MSTs: *For any cycle $C$ in a graph, the strictly heaviest edge on $C$ cannot belong to any MST.*
1. Let $(l_i, r_j) \in E$ be an edge such that $l_i \ne l_{min}$ and $r_j \ne r_{min}$.
2. Consider the 4-cycle $C = (l_i, r_j, l_{min}, r_{min}, l_i)$. The edges in $C$ are:
   - $e_1 = (l_i, r_j)$ with weight $w(e_1) = a_i + b_j$.
   - $e_2 = (r_j, l_{min})$ with weight $w(e_2) = a_{min} + b_j$.
   - $e_3 = (l_{min}, r_{min})$ with weight $w(e_3) = a_{min} + b_{min}$.
   - $e_4 = (r_{min}, l_i)$ with weight $w(e_4) = a_i + b_{min}$.
3. Let's compare $w(e_1)$ with the weights of the other three edges:
   - Since $l_i \ne l_{min}$ and all $a$ values are distinct, $a_i > a_{min}$. Thus:
     $$w(e_1) = a_i + b_j > a_{min} + b_j = w(e_2)$$
   - Since $r_j \ne r_{min}$ and all $b$ values are distinct, $b_j > b_{min}$. Thus:
     $$w(e_1) = a_i + b_j > a_i + b_{min} = w(e_4)$$
   - Since $a_i > a_{min}$ and $b_j > b_{min}$:
     $$w(e_1) = a_i + b_j > a_{min} + b_{min} = w(e_3)$$
4. Since $w(e_1)$ is strictly greater than $w(e_2), w(e_3),$ and $w(e_4)$, the edge $(l_i, r_j)$ is the strictly heaviest edge on the cycle $C$.
By the Cycle Property, $(l_i, r_j)$ cannot belong to any MST of $G$. $\blacksquare$

---

### Part c) [5 Points]
**Algorithm:**
1. Scan array $A$ to find $a_{min}$ and its index $i_{min}$ in $O(p)$ time.
2. Scan array $B$ to find $b_{min}$ and its index $j_{min}$ in $O(q)$ time.
3. Construct the edge set $E_{MST}$:
   - Add $(l_{i_{min}}, r_j)$ for all $1 \le j \le q$.
   - Add $(l_i, r_{j_{min}})$ for all $1 \le i \le p$ where $i \ne i_{min}$.
4. Return $E_{MST}$.

**Proof of Correctness:**
- By Part (b), any edge $(l_i, r_j)$ with $l_i \ne l_{min}$ and $r_j \ne r_{min}$ is the heaviest edge on some cycle and cannot belong to any MST.
- Therefore, only edges incident to $l_{min}$ or $r_{min}$ can belong to the MST.
- The set $E_{MST}$ contains exactly $p + q - 1$ edges.
- Since every vertex $l_i \in L$ is connected to $r_{min}$, and every vertex $r_j \in R$ is connected to $l_{min}$, and $l_{min}$ is connected to $r_{min}$ (which is in $E_{MST}$), the graph $(L \cup R, E_{MST})$ is connected.
- A connected graph on $p+q$ vertices with $p+q-1$ edges is a tree.
- Since it is a tree containing only edges that are allowed to be in an MST, it must be the unique MST of $G$.

**Complexity:**
- **Time Complexity:** $O(p + q)$ to find the minimums and build the output edge list. This is optimal since the output tree has $p+q-1$ edges.
- **Space Complexity:** $O(1)$ auxiliary space (excluding output space).

---

## Question 5: Distributed Vertex Coloring in the LOCAL Model (20 Points)

### Part a) [12 Points]
**Proof:**
1. Let $u \in V$ be a vertex. Let $d(u) \le \Delta$ be the degree of $u$.
2. Each vertex in $N(u) \cup \{u\}$ independently chooses a value from the range $[1, K]$ where $K = \lceil \frac{\Delta}{10\log|V|} \rceil$.
3. Fix $b_u = k$. For any neighbor $w \in N(u)$, the probability that $b_w = k$ is exactly $p = 1/K$.
4. Let $X_w$ be the indicator random variable that $b_w = b_u$. Under the choice of $b_u$, the variables $\{X_w \mid w \in N(u)\}$ are independent Bernoulli random variables with success probability $p = 1/K$.
5. Let $Y_u = \sum_{w \in N(u)} X_w$ be the number of neighbors of $u$ with the same color choice.
6. The expected value is:
   $$\mu = E[Y_u] = \frac{d(u)}{K} \le \frac{\Delta}{K} \le \frac{\Delta}{\frac{\Delta}{10\log|V|}} = 10\log|V|$$
7. We apply the Chernoff bound. For any $\delta > 0$:
   $$P(Y_u \ge (1+\delta)\mu) \le \left(\frac{e^\delta}{(1+\delta)^{1+\delta}}\right)^\mu$$
   Using a standard Chernoff simplification: for $y \ge 6\mu$, $P(Y_u \ge y) \le 2^{-y}$.
   Let's choose $y = 60\log|V| = 6 \cdot 10\log|V| \ge 6\mu$.
   Then:
   $$P(Y_u \ge 60\log|V|) \le 2^{-60\log|V|} = |V|^{-60 \log_2 e} \le |V|^{-60}$$
8. Apply the Union Bound over all $u \in V$:
   $$P(\exists u \in V : Y_u \ge 60\log|V|) \le |V| \cdot |V|^{-60} = |V|^{-59}$$
9. Thus, with high probability (at least $1 - |V|^{-59}$), for all $u \in V$, the number of neighbors sharing the same block is at most $60\log|V| \in O(\log|V|)$. $\blacksquare$

---

### Part b) [8 Points]
**Algorithm Description:**
1. **Case 1: $\Delta < 10\log|V|$**
   - Run the given algorithm $A$ directly on $G$.
   - Since $A$ uses $5\Delta$ colors, this uses $O(\Delta)$ colors and takes $O(\log\Delta) \le O(\log\log|V|)$ rounds.
2. **Case 2: $\Delta \ge 10\log|V|$**
   - **Step 1:** In the first round, each vertex $u \in V$ independently and uniformly chooses a block number $b_u \in [1, K]$ where $K = \lceil \frac{\Delta}{10\log|V|} \rceil$.
   - **Step 2:** Each node $u$ broadcasts its choice $b_u$ to all neighbors.
   - **Step 3:** This partitions the graph into $K$ disjoint induced subgraphs $G_k = G[V_k]$, where $V_k = \{u \in V \mid b_u = k\}$.
   - **Step 4:** By Part (a), with high probability, the maximum degree in each subgraph $G_k$ is at most $\Delta_k \le d_{max} = 60\log|V|$.
   - **Step 5:** We run the given algorithm $A$ on each $G_k$ in parallel.
     - For each block $G_k$, we run algorithm $A$ using a dedicated, disjoint palette of colors:
       $$\text{Palette}_k = \{(k-1) \cdot 5d_{max} + 1, \dots, k \cdot 5d_{max}\}$$
     - Since the maximum degree of $G_k$ is at most $d_{max}$, the palette size $5d_{max}$ is sufficient for algorithm $A$ to legally color $G_k$ w.h.p.

**Correctness:**
- Any two adjacent vertices $u, v$ in $G$ either belong to different blocks (i.e. $b_u \ne b_v$) or the same block (i.e. $b_u = b_v = k$).
  - If $b_u \ne b_v$, they are assigned colors from disjoint palettes, so their colors cannot conflict.
  - If $b_u = b_v = k$, they are colored by algorithm $A$ running on $G_k$. Since $A$ produces a legal coloring, their colors will not conflict.
- Thus, the final coloring is legally valid.

**Palette Size and Color Count:**
The total number of colors used in Case 2 is:
$$K \cdot 5d_{max} = \left\lceil \frac{\Delta}{10\log|V|} \right\rceil \cdot 300\log|V| \le \left(\frac{\Delta}{10\log|V|} + 1\right) 300\log|V| = 30\Delta + 300\log|V|$$
Since we are in Case 2 ($\Delta \ge 10\log|V|$), we have $300\log|V| \le 30\Delta$. Thus:
$$\text{Total Colors} \le 60\Delta \in O(\Delta)$$

**Round Complexity:**
- Step 1 & 2 takes 1 communication round to exchange block IDs.
- Running algorithm $A$ on $G_k$ in parallel takes $O(\log \Delta_k) \le O(\log(d_{max})) = O(\log\log|V|)$ rounds.
- Total Round Complexity: $O(\log\log|V|)$ rounds.

**Success Probability:**
- The partitioning step succeeds in keeping the max degree of all subgraphs $\le d_{max}$ with probability at least $1 - |V|^{-59}$.
- The coloring algorithm $A$ succeeds on each $G_k$ with high probability (at least $1 - 1/|V|^c$ for some $c \ge 2$).
- By union bound over all $K \le |V|$ subgraphs, the probability that any run of $A$ fails is at most $K \cdot |V|^{-c} \le |V|^{1-c}$.
- Therefore, the overall algorithm succeeds with high probability.
