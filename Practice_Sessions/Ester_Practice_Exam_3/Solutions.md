# 📝 Ester Practice Exam 3 - Solutions Manual

---

## Question 1: Max Vertex-Disjoint Paths

### Part a) [10 Points]
To transform the vertex-disjointness constraints into edge-capacity constraints:
1. **Vertex Splitting:** For each vertex $v \in V \setminus \{s,t\}$, split it into two vertices: $v_{in}$ and $v_{out}$, and add a directed edge $(v_{in}, v_{out})$ with capacity:
   $$c'(v_{in}, v_{out}) = 1$$
   Keep $s$ and $t$ unsplit.
2. **Edge Construction:** For each original directed edge $(u, v) \in E$ in $G$:
   - If $u = s$ and $v = t$, add $(s, t)$ with capacity 1.
   - If $u = s$ and $v \neq t$, add $(s, v_{in})$ with capacity 1 (or $\infty$).
   - If $u \neq s$ and $v = t$, add $(u_{out}, t)$ with capacity 1 (or $\infty$).
   - If $u \neq s$ and $v \neq t$, add $(u_{out}, v_{in})$ with capacity 1 (or $\infty$).
This guarantees that at most 1 unit of flow can pass through any vertex $v$, preventing path overlap.

### Part b) [5 Points]
- **Path Extraction:** Once the maximum flow $f^*$ is computed, we can extract the paths from $G'$ using path decomposition.
  * Start a DFS from $s$ in $G'$ traveling only along edges $e$ that carry a flow of $f^*(e) = 1$.
  * As soon as the DFS reaches $t$, we have found a path. We decrease the flow by 1 along the path edges (effectively setting their flow to 0) and record the path.
  * Repeat this process until no more flow path from $s$ to $t$ exists.
- **Complexity:** Each path can be found in $O(|V| + |E|)$ time using DFS. Since there are at most $|V|$ disjoint paths, the extraction takes at most $O(|V| \cdot (|V| + |E|))$ time. Since flow is integer and acyclic, this can be optimized to $O(|V| + |E|)$ total time by maintaining the DFS recursion stack.

### Part c) [5 Points]
- The new network $G' = (V', E')$ has $|V'| \le 2|V|$ vertices and $|E'| \le |E| + |V|$ edges.
- Since all capacities in $G'$ except the feedback edge are unit (capacity 1), the network is a unit network.
- Dinic's algorithm on a unit network runs in $O(|V'|^{1/2} |E'|)$ time.
- Substituting $|V'|$ and $|E'|$:
  $$\text{Total Time Complexity} = O(|V|^{1/2} |E|) \text{ time.}$$

---

## Question 2: Polynomial Interpolation in $O(n \log^2 n)$

### Part a) [7 Points]
1. We construct a binary tree of polynomials where the leaves are $M_i(x) = x - x_i$ for $1 \le i \le n$.
2. We compute the parent polynomials recursively:
   $$M_{L, R}(x) = M_{L, M}(x) \cdot M_{M+1, R}(x) \quad \text{where } M = \lfloor (L+R)/2 \rfloor$$
   using FFT to multiply the polynomials.
3. The root of this product tree is $M(x) = \prod_{i=1}^n (x - x_i)$.
- **Complexity:** The tree has $O(\log n)$ levels. At level $j$, we perform $2^j$ multiplications of size $n / 2^j$, each taking $O((n/2^j) \log (n/2^j))$ time. The work per level is $O(n \log n)$, giving a total construction time of $O(n \log^2 n)$.

### Part b) [7 Points]
By Lagrange's formula, the coefficient weights are $a_i = y_i / M'(x_i)$. To evaluate $M'(x_i)$ for all $x_i$:
1. Compute the derivative $M'(x)$ analytically in $O(n)$ time.
2. Evaluate $M'(x_i)$ at all $x_i$ using **fast multipoint evaluation**:
   - At the root, we compute $M'(x) \pmod{M_{1, n/2}(x)}$ and $M'(x) \pmod{M_{n/2+1, n}(x)}$.
   - We recursively pass the remainder polynomials down the product tree.
   - At the leaves, the polynomials are of degree 0, representing the values $M'(x_i)$.
- **Complexity:** At each node of the division tree, we perform polynomial division in $O(k \log k)$ time (using fast division). The total evaluation time is $O(n \log^2 n)$ time.

### Part c) [6 Points]
We compute the final sum using divide-and-conquer:
$$P(x) = M(x) \sum_{i=1}^n \frac{a_i}{x - x_i}$$
Define `Solve(L, R)` to compute the sum $\sum_{i=L}^R \frac{a_i}{x - x_i} = \frac{N_{L, R}(x)}{D_{L, R}(x)}$:
- **Base Case:** For $L=R$, return $N_{L, L}(x) = a_L$ and $D_{L, L}(x) = x - x_L$.
- **Recursive Step:** Let $M = \lfloor (L+R)/2 \rfloor$. Compute:
  $$N_{L, R}(x) = N_{L, M}(x) \cdot D_{M+1, R}(x) + N_{M+1, R}(x) \cdot D_{L, M}(x)$$
  $$D_{L, R}(x) = D_{L, M}(x) \cdot D_{M+1, R}(x)$$
  using FFT for polynomial multiplications.
The recurrence relation for the runtime of this step is:
$$T(n) = 2T(n/2) + O(n \log n)$$
By the Master Theorem, this solves to $T(n) = O(n \log^2 n)$ time.

---

## Question 3: Bottleneck Spanning Tree Tester

### Part a) [10 Points]
To test if there exists a spanning tree of $G$ with bottleneck weight at most $W$:
1. Construct the subgraph $G_W = (V, E_W)$ containing only the edges of weight at most $W$:
   $$E_W = \{ e \in E \mid w(e) \le W \}$$
2. Run a standard Breadth-First Search (BFS) or Depth-First Search (DFS) on $G_W$ starting from an arbitrary vertex $s \in V$.
3. If the search visits all $|V|$ vertices, return `TRUE`. Otherwise, return `FALSE`.
- **Complexity:** Constructing $G_W$ takes $O(|E|)$ time. BFS/DFS takes $O(|V| + |E|)$ time. Thus, the total time complexity is $O(|V| + |E|)$ time.

### Part b) [10 Points]
- **Forward Direction ($\implies$):** If the tester returns `TRUE`, then $G_W$ is a connected graph spanning all vertices $V$. Any connected graph contains at least one spanning tree $T \subseteq E_W$. Since $T$ contains only edges from $E_W$, the weight of every edge in $T$ is at most $W$. Thus, $T$ is a spanning tree with bottleneck weight at most $W$.
- **Backward Direction ($\impliedby$):** If there exists a spanning tree $T$ of $G$ with bottleneck weight at most $W$, then every edge $e \in T$ has weight $w(e) \le W$, meaning $T \subseteq E_W$. Since $T$ is connected and spans all vertices $V$, the graph $G_W$ must also be connected. Therefore, a BFS/DFS starting from any vertex $s$ will successfully visit all $|V|$ vertices, and the tester will return `TRUE`.

---

## Question 4: Chernoff vs. Chebyshev Bounds

### Part a) [7 Points]
Let $X \sim \text{Binomial}(n, 1/2)$ be the number of heads. We have $\mu = E[X] = n/2$ and $\text{Var}(X) = n/4$.
We want to bound $\Pr[X \ge 3n/4] = \Pr[X - n/2 \ge n/4]$.
By Chebyshev's Inequality:
$$\Pr[X - n/2 \ge n/4] \le \Pr[|X - n/2| \ge n/4] \le \frac{\text{Var}(X)}{(n/4)^2} = \frac{n/4}{n^2/16} = \frac{4}{n}$$

### Part b) [7 Points]
Using the Chernoff Bound for independent Poisson trials with $\delta = 0.5$ (since $(1+\delta)\mu = 1.5(n/2) = 3n/4$):
$$\Pr[X \ge (1+\delta)\mu] \le \left( \frac{e^\delta}{(1+\delta)^{1+\delta}} \right)^\mu$$
Substituting $\delta = 0.5$ and $\mu = n/2$:
$$\Pr[X \ge 3n/4] \le \left( \frac{e^{0.5}}{1.5^{1.5}} \right)^{n/2} \approx (0.896)^{n/2} \approx (0.946)^n$$
Alternatively, using the simpler form $e^{-\delta^2 \mu / 3}$:
$$\Pr[X \ge 3n/4] \le e^{-\frac{0.25 \cdot (n/2)}{3}} = e^{-\frac{n}{24}}$$

### Part c) [6 Points]
- **Asymptotic Comparison:** Chebyshev's bound decays polynomially ($O(1/n)$), while Chernoff's bound decays exponentially ($O(e^{-n/24})$).
- **Tighter Bound:** Chernoff's bound is exponentially tighter than Chebyshev's bound as $n \to \infty$.
- **Mathematical Reason:** Chebyshev's inequality only utilizes the first two moments of the distribution (expected value and variance). In contrast, Chernoff's bound utilizes the moment-generating function ($E[e^{tX}]$), which incorporates *all* higher moments of the distribution. This leverages the mutual independence of the coin flips to show that large deviations from the mean are exponentially rare.

---

## Question 5: Welzl's Smallest Enclosing Disc

### Part a) [8 Points]
Welzl's algorithm takes a point set $P$ and a boundary set $R$ containing up to 3 points:
`Welzl(P, R)`:
1. If $P = \emptyset$ or $|R| = 3$:
   - Return the unique circle defined by the support set $R$.
2. Choose a point $p \in P$ uniformly at random.
3. Compute the smallest enclosing disc $D = \text{Welzl}(P \setminus \{p\}, R)$.
4. If $p \in D$, return $D$.
5. If $p \notin D$, return $\text{Welzl}(P \setminus \{p\}, R \cup \{p\})$.

### Part b) [7 Points]
Let $T(n, i)$ be the expected time to solve the problem for $|P|=n$ and $|R|=i$.
- For $i=3$: $T(n, 3) = O(n)$ (base case).
- For $i < 3$:
  $$T(n, i) \le T(n-1, i) + O(1) + \Pr[p \notin D_{P \setminus \{p\}, R}] \cdot T(n-1, i+1)$$
- By backwards analysis, the smallest enclosing disc is uniquely defined by a support set of size at most $3 - i$. Since $p$ is chosen uniformly at random, the probability that $p$ is in the support set (and thus $p \notin D_{P \setminus \{p\}, R}$) is at most $\frac{3-i}{n}$.
- The recurrence relation becomes:
  $$T(n, i) \le T(n-1, i) + O(1) + \frac{3-i}{n} \cdot T(n-1, i+1)$$
- Solving by induction: $T(n, i) \le C_i \cdot n$ for some constant $C_i$. Thus, the expected runtime for $i=0$ is $T(n, 0) = O(n)$ time.

### Part c) [5 Points]
- **Support Set Role:** The support set is the minimal subset of points in $P$ that uniquely determines the smallest enclosing disc. Points outside this set can be moved or deleted without affecting the disc.
- **Maximum Size is 3:** In 2D space, a circle has 3 degrees of freedom (center coordinates $x, y$ and radius $r$). Thus, it is uniquely determined by either 2 boundary points (defining the diameter) or 3 boundary points (defining the circumcircle of a triangle). Therefore, the support set size is at most 3.
