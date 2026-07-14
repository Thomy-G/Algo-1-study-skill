# Algorithms 1 (89-220) - 2025 Winter Moed A Solutions Manual

This document contains the official, rigorous solutions for the 2025 Winter Moed A exam questions.

---

## Question 1: Constrained Minimum Spanning Trees (20 Points)

### Part a) [6 Points]
**Definition of Minimum Spanning Tree (MST):**
Given a connected, undirected graph $G = (V,E)$ with edge weights $w: E \rightarrow \mathbb{R}$, a spanning tree $T = (V, E_T)$ is a subgraph of $G$ that is a tree (acyclic and connected) and contains all vertices of $V$. A Minimum Spanning Tree (MST) is a spanning tree $T$ whose total edge weight is minimized:
$$w(T) = \sum_{e \in E_T} w(e) \le w(T') \quad \text{for any spanning tree } T' \text{ of } G$$

---

### Part b) [7 Points]
**Algorithm for MST with $deg_T(s) \le 2$:**
Since the degree of $s$ in $T$ is at most 2, $T$ contains either 1 or 2 edges incident to $s$. Let $E(s)$ be the set of edges incident to $s$ in $G$. Since $\text{deg}_G(s) = 8$, $|E(s)| = 8$.
1. Let $\mathcal{S}$ be the set of all subsets of $E(s)$ of size 1 or 2. The number of such subsets is:
   $$|\mathcal{S}| = \binom{8}{1} + \binom{8}{2} = 8 + 28 = 36$$
2. For each subset $E_s \in \mathcal{S}$:
   - Check if $E_s$ forms a cycle (only possible if $|E_s| = 2$ and both edges connect to the same vertex, which is impossible in a simple graph).
   - Construct a modified graph $G' = (V', E')$ by:
     - Removing $s$ and all edges in $E(s)$ from $G$.
     - Contracting the endpoints of the edges in $E_s$ into a single super-node.
   - Run a standard MST algorithm (e.g. Prim's or Kruskal's) on $G'$ to find the minimum spanning forest/tree on the remaining vertices.
   - If the contracted graph is connected, let $T'$ be the MST of $G'$. Then $T' \cup E_s$ is a valid spanning tree of $G$ with degree of $s$ equal to $|E_s| \le 2$. Its weight is $w(T') + \sum_{e \in E_s} w(e)$.
3. Return the spanning tree with the minimum total weight among all 36 candidates.

**Correctness:**
The optimal tree $T_{opt}$ must contain a subset of edges incident to $s$ of size 1 or 2. Since we exhaustively check all 36 possible subsets of size 1 and 2, contract them to guarantee they are chosen, and find the optimal spanning tree on the rest of the graph, we are guaranteed to find the minimum weight spanning tree satisfying $deg_T(s) \le 2$.

**Complexity:**
- There are exactly 36 configurations. For each, we run an MST algorithm on a graph of size $O(|V|)$ vertices and $O(|E|)$ edges.
- Running Kruskal's algorithm takes $O(|E| \log |V|)$ time.
- **Total Time Complexity:** $36 \times O(|E| \log |V|) = O(|E| \log |V|)$ time.
- **Total Space Complexity:** $O(|V| + |E|)$ to store the graphs.

---

### Part c) [7 Points]
**Algorithm for MST with $deg_T(s) \ge 2$:**
1. Compute the standard MST $T$ of $G$ in $O(|E| \log |V|)$ time.
2. If the degree of $s$ in $T$ is already $\ge 2$, return $T$.
3. If the degree of $s$ in $T$ is exactly 1:
   - Let $e_{tree} = (s, u)$ be the unique edge incident to $s$ in $T$.
   - For each of the other 7 edges incident to $s$ in $G$, say $e = (s, v)$:
     - Adding $e$ to $T$ creates a unique cycle $C$.
     - Find the maximum-weight edge $e'$ on the path between $u$ and $v$ in $T$ (this path does not contain $s$ because $\text{deg}_T(s) = 1$).
     - The candidate tree is $T_e = T \cup \{e\} \setminus \{e'\}$, and its weight is $w(T) + w(e) - w(e')$.
   - Choose the edge $e$ that minimizes $w(e) - w(e')$, and return the corresponding tree $T_e$.

**Correctness:**
If the standard MST has $\text{deg}_T(s) = 1$, we must add at least one more edge incident to $s$ to satisfy the constraint $\text{deg}(s) \ge 2$. Adding a second edge $(s, v)$ creates a cycle, which we must break by removing an edge $e'$ from the cycle. To keep the tree minimum, we choose the replacement that minimizes the weight increase $w(s,v) - w(e')$. Since we check all possible remaining edges incident to $s$, this is guaranteed to yield the optimal tree with $\text{deg}_T(s) \ge 2$.

**Complexity:**
- Finding the initial MST takes $O(|E| \log |V|)$ time.
- Finding the cycles and maximum weight edges for 7 candidate edges takes 7 runs of BFS/DFS on the tree $T$, which takes $7 \times O(|V|) = O(|V|)$ time.
- **Total Time Complexity:** $O(|E| \log |V|)$ time.
- **Total Space Complexity:** $O(|V| + |E|)$.

---

## Question 2: Algo-Mania Coupon Collection Strategy (20 Points)

### Part a) [15 Points]
Let $C$ be the random variable representing the total cost of the algorithm. We split the execution into two phases:

**Phase 1: Collecting the first $n-k$ card types using Option 1 (1 NIS each)**
Let $X_i$ be the number of purchases needed to obtain the $i$-th distinct card type, given that $i-1$ distinct types have already been collected.
- The probability of success (getting a new card type) on each draw is:
  $$p_i = \frac{n - (i-1)}{n}$$
- $X_i$ is a geometric random variable with parameter $p_i$, so its expected value is:
  $$E[X_i] = \frac{1}{p_i} = \frac{n}{n - i + 1}$$
- Since each card in Phase 1 costs 1 NIS, the expected cost of Phase 1 is:
  $$E[C_1] = \sum_{i=1}^{n-k} 1 \cdot E[X_i] = \sum_{i=1}^{n-k} \frac{n}{n - i + 1}$$
  By substituting $j = n - i + 1$, the sum becomes:
  $$E[C_1] = n \sum_{j=k+1}^n \frac{1}{j}$$

**Phase 2: Collecting the remaining $k$ card types using Option 2 (5 NIS each)**
Since Option 2 guarantees drawing a card type that we do not currently own, the probability of obtaining a new card type on each purchase is exactly 1.
- We need exactly $k$ purchases, each costing 5 NIS.
- The expected cost of Phase 2 is:
  $$E[C_2] = 5k$$

**Total Expected Cost:**
$$E[C] = E[C_1] + E[C_2] = n \sum_{j=k+1}^n \frac{1}{j} + 5k$$

---

### Part b) [5 Points]
We want to find the integer $k \in \{0, \dots, n\}$ that minimizes the expected cost function $f(k) = n \sum_{j=k+1}^n \frac{1}{j} + 5k$.
Let's analyze the difference $f(k+1) - f(k)$:
$$f(k+1) - f(k) = \left(n \sum_{j=k+2}^n \frac{1}{j} + 5(k+1)\right) - \left(n \sum_{j=k+1}^n \frac{1}{j} + 5k\right) = 5 - \frac{n}{k+1}$$
- The difference is negative when:
  $$5 - \frac{n}{k+1} < 0 \implies k+1 < \frac{n}{5} \implies k < \frac{n}{5} - 1$$
- The difference is positive when:
  $$k > \frac{n}{5} - 1$$
Thus, $f(k)$ decreases as $k$ increases up to $\frac{n}{5}-1$, and increases afterwards. The optimal $k$ that minimizes the expected cost is:
$$k^* = \left\lfloor \frac{n}{5} \right\rfloor \quad \text{or} \quad \left\lceil \frac{n}{5} \right\rceil - 1$$

---

## Question 3: Matrix Row and Column Sums Realization (20 Points)

### Algorithm Description
We solve this by reducing the problem to **Maximum Flow** in a bipartite network.
1. Construct a flow network $H = (V_H, E_H)$ with a source $s$, a sink $t$, $n$ row vertices $\{R_1, \dots, R_n\}$, and $m$ column vertices $\{C_1, \dots, C_m\}$.
2. Add directed edges and capacities:
   - For each $1 \le i \le n$: add $(s, R_i)$ with capacity $r_i$.
   - For each $1 \le j \le m$: add $(C_j, t)$ with capacity $c_j$.
   - For all $1 \le i \le n$ and $1 \le j \le m$: add $(R_i, C_j)$ with capacity 1.
3. Check if $\sum_{i=1}^n r_i == \sum_{j=1}^m c_j$. If not, return `False`. Let this sum be $S$.
4. Run **Dinic's Algorithm** to compute the maximum flow $f^*$ from $s$ to $t$ on $H$.
5. If the value of the max flow is exactly $S$:
   - Construct the binary matrix $M$ of size $n \times m$: set $M[i,j] = 1$ if $f^*(R_i, C_j) == 1$, and $0$ otherwise.
   - Return `True` and $M$.
6. Otherwise, return `False`.

### Proof of Correctness
- **Binary constraints:** Since the capacity of each edge $(R_i, C_j)$ is 1, and capacities are integers, the Integrality Theorem guarantees that there exists an integer maximum flow where $f^*(R_i, C_j) \in \{0, 1\}$. Thus, $M[i,j] \in \{0, 1\}$.
- **Row sums:** The flow leaving $R_i$ is $\sum_j f^*(R_i, C_j) = f^*(s, R_i) \le r_i$. Since the max flow value is $S = \sum r_i$, the edge $(s, R_i)$ must be fully saturated. Thus, the sum of row $i$ is exactly $r_i$.
- **Column sums:** Similarly, the flow entering $C_j$ is $\sum_i f^*(R_i, C_j) = f^*(C_j, t) \le c_j$. Since all sink edges must be saturated, the sum of column $j$ is exactly $c_j$.

### Complexity Analysis
- **Network size:** $|V_H| = n + m + 2$ vertices, $|E_H| = n + m + nm$ edges.
- Since the capacity of all internal edges is 1, the network is a unit-capacity network. Dinic's algorithm runs in $O(|E_H| \sqrt{|V_H|})$ time.
- **Total Time Complexity:** $O(nm \sqrt{n+m})$ time.
- **Total Space Complexity:** $O(nm)$ to store the graph and the output matrix.

---

## Question 4: Min-Plus Matrix Multiplication (20 Points)

### Part a) [14 Points]
**Algorithm for $M=1$:**
When $M = 1$, the entries of $A$ and $B$ are in $\{1, \infty\}$. The min-plus product is:
$$c_{i,j} = \min_k \{a_{i,k} + b_{k,j}\}$$
Since the only finite value is 1, the sum $a_{i,k} + b_{k,j}$ is either $1 + 1 = 2$ (if both are 1) or $\infty$ (if at least one is $\infty$).
1. Create boolean matrices $\hat{A}$ and $\hat{B}$ of size $n \times n$:
   - $\hat{A}_{i,j} = 1$ if $a_{i,j} = 1$, and $0$ if $a_{i,j} = \infty$.
   - $\hat{B}_{i,j} = 1$ if $b_{i,j} = 1$, and $0$ if $b_{i,j} = \infty$.
2. Compute the standard boolean matrix multiplication:
   $$D = \hat{A} \times \hat{B}$$
   using a fast matrix multiplication algorithm (e.g. Strassen's) in $O(n^\omega)$ time.
3. Construct the output matrix $C$:
   - If $D_{i,j} > 0$, set $c_{i,j} = 2$.
   - If $D_{i,j} == 0$, set $c_{i,j} = \infty$.
4. Return $C$.

**Correctness:**
$D_{i,j} = \sum_{k=1}^n \hat{A}_{i,k} \hat{B}_{k,j} > 0$ if and only if there exists some index $k$ such that $\hat{A}_{i,k} = 1$ and $\hat{B}_{k,j} = 1$, which means $a_{i,k} = 1$ and $b_{k,j} = 1$. In this case, the minimum sum is $1 + 1 = 2$. If no such $k$ exists, the sum is always $\infty$.

**Complexity:**
- **Time Complexity:** $O(n^\omega)$ where $\omega < 2.373$ is the matrix multiplication exponent.
- **Space Complexity:** $O(n^2)$ to store the matrices.

---

### Part b) [6 Points]
**Algorithm for Bounded $M \ge 1$:**
1. Initialize an $n \times n$ matrix $C$ with all entries set to $\infty$.
2. For each value $x \in \{1, \dots, M\}$ and $y \in \{1, \dots, M\}$:
   - Construct boolean matrices $A^{(x)}$ and $B^{(y)}$:
     - $A^{(x)}_{i,j} = 1$ if $a_{i,j} = x$, else $0$.
     - $B^{(y)}_{i,j} = 1$ if $b_{i,j} = y$, else $0$.
   - Compute the boolean matrix multiplication $D^{(x,y)} = A^{(x)} \times B^{(y)}$ in $O(n^\omega)$ time.
   - For each pair $i, j$ where $D^{(x,y)}_{i,j} > 0$:
     - Update $c_{i,j} = \min(c_{i,j}, x + y)$.
3. Return $C$.

**Correctness:**
Any finite sum $a_{i,k} + b_{k,j}$ must equal $x + y$ for some $x, y \in \{1, \dots, M\}$. The product $D^{(x,y)}_{i,j} > 0$ indicates that there is at least one $k$ where $a_{i,k} = x$ and $b_{k,j} = y$. By iterating over all possible pairs $(x,y)$ and taking the minimum of $x+y$, we find the true minimum sum for all cells.

**Complexity:**
- There are exactly $M^2$ pairs of $(x,y)$.
- For each pair, we run one matrix multiplication in $O(n^\omega)$ time and update $C$ in $O(n^2)$ time.
- **Total Time Complexity:** $O(M^2 \cdot n^\omega)$ time.
- **Total Space Complexity:** $O(n^2)$ space.

---

## Question 5: RNA Secondary Structure Folding (20 Points)

### Recursive Formula
Let $DP[i, j]$ be the size of the largest RNA pairing for the substring $s_i s_{i+1} \dots s_j$.
- **Base Cases:**
  - $DP[i, j] = 0$ for all $i \ge j$. (A sequence of length 0 or 1 cannot contain any valid pairs).
- **Recursive Step:**
  For $i < j$, we consider the character $s_i$:
  - **Option 1 ($s_i$ is not paired):** The maximum pairing is $DP[i+1, j]$.
  - **Option 2 ($s_i$ is paired with some $s_k$ where $i < k \le j$):** We can pair $s_i$ and $s_k$ if $(s_i, s_k) \in \{AU, UA, CG, GC\}$. Due to the no-crossing constraint, this splits the substring into two independent subproblems: the sub-segment inside the pair ($s_{i+1} \dots s_{k-1}$) and the sub-segment outside ($s_{k+1} \dots s_j$).
  Taking the maximum over all choices:
  $$DP[i, j] = \max \left( DP[i+1, j], \max_{k \in [i+1, j] \text{ s.t. } s_i, s_k \text{ can pair}} \{ 1 + DP[i+1, k-1] + DP[k+1, j] \} \right)$$

### Correctness Explanation
The recursive relation considers all possible fates for the leftmost character $s_i$: either it does not participate in any pair, or it pairs with some character $s_k$ to its right. Since the no-crossing rule prevents any pair from crossing the boundary of $(i, k)$, the subproblems inside and outside are completely independent and can be solved optimally.

### Dynamic Programming Algorithm
1. Initialize an $n \times n$ table $DP$ with 0.
2. Iterate over substring lengths $L = 2$ to $n$:
   - For each starting index $i = 1$ to $n - L + 1$:
     - Let $j = i + L - 1$.
     - Set $DP[i, j] = DP[i+1, j]$.
     - For each $k = i+1$ to $j$:
       - If $s_i$ and $s_k$ can pair:
         - $DP[i, j] = \max(DP[i, j], 1 + DP[i+1, k-1] + DP[k+1, j])$.
3. Return $DP[1, n]$.

### Complexity Analysis
- **Time Complexity:** There are $O(n^2)$ cells in the table. Computing each cell takes $O(n)$ time due to the loop over $k$. Thus, the total time complexity is $O(n^3)$.
- **Space Complexity:** The table of size $n \times n$ requires $O(n^2)$ space.
