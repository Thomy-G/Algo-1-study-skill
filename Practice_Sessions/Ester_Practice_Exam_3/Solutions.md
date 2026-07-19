# 📝 Ester Practice Exam 3 - Solutions Manual

---

## Question 1: Max Vertex-Disjoint Paths
### Reduction to Standard Max Flow
To find the maximum number of vertex-disjoint paths from $s$ to $t$ in a directed graph $G = (V,E)$, we convert the vertex-disjointness constraints into edge-capacity constraints in a new flow network $G' = (V', E')$.
1. **Vertex Splitting:**
   - For each vertex $v \in V \setminus \{s,t\}$, split it into two vertices: $v_{in}$ and $v_{out}$.
   - Add a directed edge $(v_{in}, v_{out})$ with capacity $c'(v_{in}, v_{out}) = 1$.
   - Keep $s$ and $t$ unsplit (or split them with capacity $\infty$).
2. **Edge Construction:**
   - For each directed edge $(u, v) \in E$:
     * If $u = s$ and $v = t$, add $(s, t)$ with capacity 1.
     * If $u = s$ and $v \neq t$, add $(s, v_{in})$ with capacity 1 (or $\infty$).
     * If $u \neq s$ and $v = t$, add $(u_{out}, t)$ with capacity 1 (or $\infty$).
     * If $u \neq s$ and $v \neq t$, add $(u_{out}, v_{in})$ with capacity 1 (or $\infty$).
3. **Solving:**
   - Run Dinic's maximum flow algorithm on $G'$ to find the maximum flow $f^*$ from $s$ to $t$.
   - The value of this maximum flow $|f^*|$ is the maximum number of vertex-disjoint paths.
   - Using path decomposition (DFS on edges with flow 1), we can extract the $|f^*|$ paths in $O(|V| + |E|)$ time.

### Proof of Correctness
- **Vertex-Disjointness:** Since the capacity of the edge $(v_{in}, v_{out})$ is exactly 1, and capacities are integers, at most 1 unit of flow can pass through any vertex $v \in V \setminus \{s,t\}$. Thus, no two paths can share a vertex, ensuring they are vertex-disjoint.
- **Max Paths:** By the Integrality Theorem, if capacities are integers, there exists an integer maximum flow. Since all capacities are 1, this flow corresponds to a set of edge-disjoint paths in $G'$ which map directly to vertex-disjoint paths in $G$.

### Complexity Analysis
- $|V'| \le 2|V|$, $|E'| \le |E| + |V|$.
- Dinic's algorithm on a unit network runs in $O(|V'|^{1/2} |E'|) = O(|V|^{1/2} |E|)$ time.
- **Total Time Complexity:** $O(|V|^{1/2} |E|)$ time.

---

## Question 2: Polynomial Interpolation in $O(n \log^2 n)$
### Algorithm Description
We use a divide-and-conquer approach combined with the Fast Fourier Transform (FFT).
1. **Product Tree Construction:**
   - We construct a binary tree of polynomials where the leaves are $M_i(x) = x - x_i$.
   - For any node covering the range $[L, R]$, the polynomial is:
     $$M_{L, R}(x) = M_{L, M}(x) \cdot M_{M+1, R}(x) \quad \text{where } M = \lfloor (L+R)/2 \rfloor$$
   - The root polynomial is $M(x) = \prod_{i=1}^n (x - x_i)$.
2. **Derivative Evaluation:**
   - Compute the derivative $M'(x)$ in $O(n)$ time.
   - Evaluate $M'(x_i)$ for all $x_i$ using fast multipoint evaluation. This takes $O(n \log^2 n)$ time by recursively computing the remainder of $M'(x)$ modulo the polynomials in the product tree.
   - Let $a_i = y_i / M'(x_i)$ for all $i$.
3. **Divide-and-Conquer Summation:**
   - We want to compute:
     $$P(x) = M(x) \sum_{i=1}^n \frac{a_i}{x - x_i}$$
   - Define a recursive function `Solve(L, R)` that returns the numerator $N_{L, R}(x)$ and denominator $D_{L, R}(x) = M_{L, R}(x)$ of the sum $\sum_{i=L}^R \frac{a_i}{x - x_i}$:
     * If $L = R$, return $N_{L, L}(x) = a_L$ and $D_{L, L}(x) = x - x_L$.
     * Else, let $M = \lfloor (L+R)/2 \rfloor$. Compute:
       $$(N_1, D_1) = \text{Solve}(L, M) \quad \text{and} \quad (N_2, D_2) = \text{Solve}(M+1, R)$$
     * Return:
       $$N_{L, R}(x) = N_1(x) \cdot D_2(x) + N_2(x) \cdot D_1(x)$$
       $$D_{L, R}(x) = D_1(x) \cdot D_2(x)$$
       Multiply these polynomials using FFT.
4. **Final Step:** The final polynomial is $P(x) = N_{1, n}(x)$.

### Complexity Analysis
- The depth of the recursion tree is $O(\log n)$.
- At level $j$ of the tree, we perform $2^j$ multiplications of polynomials of degree $n / 2^j$.
- Using FFT, multiplying two polynomials of degree $k$ takes $O(k \log k)$ time.
- The work done at each level of the tree is $2^j \cdot O((n/2^j) \log (n/2^j)) = O(n \log n)$.
- Summing over all $O(\log n)$ levels, the total time is:
  $$\text{Total Time} = O(\log n) \cdot O(n \log n) = O(n \log^2 n)$$

---

## Question 3: Bottleneck Spanning Tree Tester
### 1. Algorithm Description
To test if there exists a spanning tree of $G$ with bottleneck weight at most $W$:
1. Construct the subgraph $G_W = (V, E_W)$ containing only the edges of weight at most $W$:
   $$E_W = \{ e \in E \mid w(e) \le W \}$$
2. Run a standard Breadth-First Search (BFS) or Depth-First Search (DFS) on $G_W$ starting from an arbitrary vertex $s \in V$.
3. If the search visits all $|V|$ vertices, return `TRUE`. Otherwise, return `FALSE`.

### 2. Proof of Correctness
- **Forward Direction ($\implies$):** If the tester returns `TRUE`, then $G_W$ is a connected graph spanning all vertices $V$. By definition, any connected graph contains at least one spanning tree $T \subseteq E_W$. Since $T$ contains only edges from $E_W$, the weight of every edge in $T$ is at most $W$. Thus, $T$ is a spanning tree with bottleneck weight at most $W$.
- **Backward Direction ($\impliedby$):** If there exists a spanning tree $T$ of $G$ with bottleneck weight at most $W$, then every edge $e \in T$ has weight $w(e) \le W$, meaning $T \subseteq E_W$. Since $T$ is connected and spans all vertices $V$, the graph $G_W$ must also be connected. Therefore, a BFS/DFS starting from any vertex $s$ will successfully visit all $|V|$ vertices, and the tester will return `TRUE`.

---

## Question 4: Chernoff vs. Chebyshev Bounds
We want to bound $\Pr[X \ge 3n/4]$ for $X \sim \text{Binomial}(n, 1/2)$.
We have $E[X] = \mu = n/2$ and $\text{Var}(X) = \sigma^2 = n/4$.
Let $Y = X - n/2$. We want to bound $\Pr[Y \ge n/4]$.

### 1. Chebyshev's Inequality
By Chebyshev's Inequality:
$$\Pr[Y \ge n/4] \le \Pr[|Y| \ge n/4] \le \frac{\text{Var}(X)}{(n/4)^2} = \frac{n/4}{n^2/16} = \frac{4}{n}$$

### 2. Chernoff Bounds
By the standard upper Chernoff bound for independent Poisson trials (with $\delta = 0.5$ since $(1+\delta)\mu = 1.5(n/2) = 3n/4$):
$$\Pr[X \ge (1+\delta)\mu] \le \left( \frac{e^\delta}{(1+\delta)^{1+\delta}} \right)^\mu$$
Substituting $\delta = 0.5$ and $\mu = n/2$:
$$\Pr[X \ge 3n/4] \le \left( \frac{e^{0.5}}{1.5^{1.5}} \right)^{n/2} = \left( \frac{\sqrt{e}}{1.5\sqrt{1.5}} \right)^{n/2} \approx (0.896)^{n/2} \approx (0.946)^n$$
Alternatively, using the simpler form $e^{-\delta^2 \mu / 3}$:
$$\Pr[X \ge 3n/4] \le e^{-\frac{0.25 \cdot (n/2)}{3}} = e^{-\frac{n}{24}}$$

### 3. Comparison
- **Chebyshev's Bound** yields a polynomial decay: $O(1/n)$.
- **Chernoff's Bound** yields an exponential decay: $O(a^n)$ where $a \approx 0.946 < 1$ (or $e^{-n/24}$).
- **Why Chernoff is Tighter:** Chernoff's bound is exponentially tighter than Chebyshev's bound as $n \to \infty$. This is because Chebyshev only uses the second moment (variance), whereas Chernoff uses the moment-generating function ($E[e^{tX}]$), which implicitly utilizes *all* higher moments of the distribution. This leverages the strong independence of the coin flips to show that large deviations from the mean are exponentially rare.

---

## Question 5: Welzl's Smallest Enclosing Disc Algorithm
### 1. Algorithm Description
Welzl's algorithm takes a set of points $P$ and a boundary set $R$ (containing up to 3 points that must lie on the boundary of the disc).
`Welzl(P, R)`:
1. If $P = \emptyset$ or $|R| = 3$:
   - Return the unique circle defined by the support set $R$ (if $|R|=0$ it is empty, if $|R|=1$ it is the point, if $|R|=2$ it has the two points as diameter, if $|R|=3$ it is the circumcircle).
2. Choose a point $p \in P$ uniformly at random.
3. Compute the smallest enclosing disc $D = \text{Welzl}(P \setminus \{p\}, R)$.
4. If $p \in D$, return $D$.
5. If $p \notin D$, then $p$ must lie on the boundary of the smallest enclosing disc of $P$ with boundary $R$. Return $\text{Welzl}(P \setminus \{p\}, R \cup \{p\})$.

### 2. Proof of expected $O(n)$ runtime (Backwards Analysis)
Let $T(n, i)$ be the expected time to solve the problem for $|P|=n$ and $|R|=i$.
- If $i = 3$, no further points can be added to $R$. The algorithm immediately hits the base case. Thus, $T(n, 3) = O(1) \cdot n = O(n)$.
- For $i < 3$:
  $$T(n, i) \le T(n-1, i) + O(1) + \Pr[p \notin D_{P \setminus \{p\}, R}] \cdot T(n-1, i+1)$$
- By **backwards analysis**, the smallest enclosing disc of $P$ with boundary $R$ is uniquely determined by a support set of size at most $3 - i$ from the points in $P$. Since the point $p$ is chosen uniformly at random from the $n$ points, the probability that $p$ belongs to this support set (and thus $p \notin D_{P \setminus \{p\}, R}$) is at most:
  $$\Pr[p \notin D] \le \frac{3 - i}{n}$$
- Substituting this probability:
  $$T(n, i) \le T(n-1, i) + O(1) + \frac{3 - i}{n} \cdot T(n-1, i+1)$$
- We prove by induction that $T(n, i) \le C_i \cdot n$ for some constants $C_i$.
  * For $i=3$: $T(n, 3) \le C_3 n$ (holds).
  * Assume $T(n, i+1) \le C_{i+1} n$. Then:
    $$T(n, i) \le T(n-1, i) + O(1) + \frac{3-i}{n} \cdot C_{i+1} n = T(n-1, i) + O(1) + (3-i)C_{i+1}$$
    Summing this recurrence from 1 to $n$:
    $$T(n, i) \le O(n) + n \cdot (3-i)C_{i+1} = O(n)$$
    Therefore, the expected runtime for $i=0$ is $T(n, 0) = O(n)$. $\blacksquare$

### 3. Support Set Role
- The **support set** is the minimal subset of points in $P$ that uniquely determines the smallest enclosing disc.
- Removing any point not in the support set does not change the smallest enclosing disc.
- In 2D space, a circle is uniquely determined by either 2 points (as a diameter) or 3 points (passing through their circumcircle). Thus, the maximum size of the support set is 3.
