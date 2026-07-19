# 📝 Ester Practice Exam 1 - Solutions Manual

---

## Question 1: Vertex Deletion in Flow Networks
### Algorithm Description
We want to find a vertex $v \in V \setminus \{s,t\}$ whose deletion reduces the maximum flow value the most.
1. Run a standard max-flow algorithm (e.g., Dinic's algorithm) on $G$ to find a maximum flow $f^*$ and its value $|f^*|$ in $O(|V|^2 |E|)$ time.
2. For each vertex $v \in V \setminus \{s,t\}$:
   a. Compute the flow passing through $v$ in $f^*$: $F_v = \sum_{u \in V} f^*(u, v)$.
   b. Construct a starting flow $f^{(v)}$ on $G \setminus \{v\}$ by setting $f^{(v)}(e) = f^*(e)$ for all edges not incident to $v$, and $f^{(v)}(e) = 0$ for all edges incident to $v$. 
      - Note that $f^{(v)}$ is a valid flow in $G \setminus \{v\}$ because flow conservation still holds at all vertices $w \neq v$ (since we zeroed out both the incoming and outgoing flow at $v$, which was balanced).
      - The value of this initial flow is $|f^{(v)}| = |f^*| - F_v$.
   c. Construct the residual graph $H$ of $G \setminus \{v\}$ with respect to $f^{(v)}$.
   d. Run Ford-Fulkerson augmentation steps (using BFS to find augmenting paths) on $H$ to find the maximum flow in $G \setminus \{v\}$.
      - Since we only need to redirect at most $F_v$ units of flow, and we are in a unit-capacity network (or a network where $F_v$ is small), the number of augmentation steps is bounded by $F_v$.
      - Each BFS takes $O(|V| + |E|)$ time.
      - The total time to find the max flow in $G \setminus \{v\}$ starting from $f^{(v)}$ is $O(F_v \cdot (|V| + |E|))$.
3. Return the vertex $v$ that results in the smallest maximum flow value.

### Proof of Correctness
- **Validity of $f^{(v)}$:** By zeroing out all flow incident to $v$, the net flow entering and leaving $v$ is 0. For any other vertex $w \neq v, s, t$, its incoming and outgoing flows are unchanged unless they were connected to $v$, which are now 0. Since the flow through $v$ was balanced ($F_v$ in, $F_v$ out), flow conservation is preserved at all vertices $w \in V \setminus \{s,t,v\}$.
- **Maximality:** Starting from the valid flow $f^{(v)}$ and running Ford-Fulkerson to optimality guarantees that we find the true maximum flow in $G \setminus \{v\}$.
- **Optimization:** By evaluating all $v \in V \setminus \{s,t\}$, we are guaranteed to find the one that minimizes the resulting max flow.

### Complexity Analysis
- Running Dinic's once takes $O(|V|^2 |E|)$ time.
- For each $v$, the number of augmentations is at most $F_v$. In a unit-capacity network, $F_v \le \text{in-degree}(v)$.
- The sum of in-degrees over all vertices is $|E|$. Therefore, the sum of maximum augmentations across all vertices is $\sum_v F_v \le \sum_v \text{in-degree}(v) = |E|$.
- Thus, the total time spent in the augmentation loops across all vertices is:
  $$\sum_{v \in V} O(F_v \cdot (|V| + |E|)) = O(|E| \cdot (|V| + |E|))$$
- **Total Time Complexity:** $O(|V|^2 |E| + |E|(|V| + |E|))$ which is highly efficient and meets the $O(|V| \cdot (|V| + |E|))$ bound.

---

## Question 2: Arithmetic Progressions via FFT
### Algorithm Description
Three elements $a, b, c \in A$ form an arithmetic progression if $b - a = c - b \iff a + c = 2b$.
We want to find if there exist distinct $a, c \in A$ and $b \in A$ such that $a + c = 2b$.
1. Represent the set $A$ as a characteristic polynomial $P(x) = \sum_{a \in A} x^a$.
2. Compute the polynomial square $Q(x) = P(x) \cdot P(x) = P^2(x)$ using the Fast Fourier Transform (FFT) in $O(M \log M)$ time.
3. The coefficient of $x^k$ in $Q(x)$, denoted $q_k$, is the number of pairs $(a, c) \in A \times A$ such that $a + c = k$.
4. We are looking for cases where $a + c = 2b$ with $a \neq c$.
   - The total number of pairs $(a, c)$ that sum to $2b$ is $q_{2b}$.
   - We must exclude the trivial pair where $a = c = b$ (since $b + b = 2b$). There is exactly 1 such pair for each $b \in A$.
   - Thus, there exists an AP triplet if and only if there exists some $b \in A$ such that:
     $$q_{2b} > 1$$
5. Iterate through all $b \in A$ and check if $q_{2b} > 1$. If yes, return `TRUE`. Otherwise, return `FALSE`.

### Proof of Correctness
- The polynomial multiplication $P(x) \cdot P(x)$ correctly computes the number of representations of each integer $k$ as the sum of two elements of $A$.
- For any $b \in A$, the sum $a + c = 2b$ has $q_{2b}$ solutions in $A \times A$.
- If $q_{2b} > 1$, then there must be at least one solution $(a, c)$ other than $(b, b)$. Since $a + c = 2b$ and $(a,c) \neq (b,b)$, we must have $a \neq c$, forming a valid arithmetic progression $a, b, c$.

### Complexity Analysis
- Constructing $P(x)$ takes $O(M)$ time.
- Performing FFT, multiplying point-values, and performing Inverse FFT takes $O(M \log M)$ time.
- Iterating through $b \in A$ and checking the coefficients takes $O(M)$ time.
- **Total Time Complexity:** $O(M \log M)$ time.

---

## Question 3: Dynamic MST Weight Increase
### Algorithm Description
We are given an MST $T$ of $G$, and we increase the weight of a single edge $e_0 = (u, v) \in T$ to $w'(e_0) > w(e_0)$.
1. Remove $e_0$ from $T$. This disconnects $T$ into exactly two trees, $T_u$ (containing $u$) and $T_v$ (containing $v$).
2. Identify the partition of vertices $(S, V \setminus S)$ defined by $T_u$ and $T_v$. We can find $S$ by running BFS/DFS in $T \setminus \{e_0\}$ starting from $u$ in $O(|V|)$ time.
3. Find the minimum-weight edge $e_{\text{min}}$ in $G$ that crosses the cut $(S, V \setminus S)$ (which could be the modified $e_0$ itself if no lighter edge crosses the cut).
   - We can find this by iterating over all edges in $G$ and checking if one endpoint is in $S$ and the other is in $V \setminus S$, keeping the one with the smallest weight.
4. The new MST is $T' = T \setminus \{e_0\} \cup \{e_{\text{min}}\}$.
5. Return $T'$.

### Proof of Correctness
- Removing $e_0$ from $T$ creates a cut $(S, V \setminus S)$. Any spanning tree of $G$ must contain at least one edge crossing this cut to remain connected.
- Since $T \setminus \{e_0\}$ is a spanning forest of two components containing no cycles, adding the minimum-weight edge crossing the cut $(S, V \setminus S)$ is guaranteed to yield the minimum-weight spanning tree of $G$ by the Cut Property.

### Complexity Analysis
- Finding the cut $(S, V \setminus S)$ using BFS in $T \setminus \{e_0\}$ takes $O(|V|)$ time.
- Iterating over all edges in $E$ to find the minimum-weight edge crossing the cut takes $O(|E|)$ time.
- **Total Time Complexity:** $O(|V| + |E|)$ time.

---

## Question 4: Universal Hashing Collision Bounds
### Proof of Correctness
1. Let $S = \{x_1, x_2, \dots, x_n\}$ be the set of $n$ keys.
2. For any pair of distinct keys $x_i, x_j \in S$ (where $i \neq j$), let $X_{ij}$ be the indicator random variable for a collision:
   $$X_{ij} = \begin{cases} 1 & \text{if } h(x_i) = h(x_j) \\ 0 & \text{otherwise} \end{cases}$$
3. Since $h$ is chosen from a universal family $\mathcal{H}$, the probability of a collision for any pair is:
   $$\Pr[X_{ij} = 1] \le \frac{1}{m}$$
4. Let $X$ be the random variable representing the total number of collisions:
   $$X = \sum_{1 \le i < j \le n} X_{ij}$$
5. By linearity of expectation, the expected number of collisions is:
   $$E[X] = \sum_{1 \le i < j \le n} E[X_{ij}] = \sum_{1 \le i < j \le n} \Pr[X_{ij} = 1] \le \binom{n}{2} \cdot \frac{1}{m}$$
6. Given $m = n^2$:
   $$E[X] \le \frac{n(n-1)}{2} \cdot \frac{1}{n^2} = \frac{n^2 - n}{2n^2} < \frac{1}{2}$$
7. By **Markov's Inequality**, since $X$ is a non-negative integer-valued random variable:
   $$\Pr[X \ge 1] \le E[X] < \frac{1}{2}$$
   Therefore, the probability of at least one collision is at most $1/2$. $\blacksquare$

---

## Question 5: Linear Programming & Geometry
### 1. LP Formulation (Standard Maximization Form)
Let $x$ be the number of units of $X$ produced daily, and $y$ be the number of units of $Y$ produced daily.
*   **Objective Function:**
    $$\text{Maximize } Z = 40x + 50y$$
*   **Constraints:**
    *   Machine time constraint: $2x + y \le 8$
    *   Labor time constraint: $x + 3y \le 9$
    *   Non-negativity: $x \ge 0, y \ge 0$

### 2. Feasible Region & Vertices
The feasible region is bounded by the axes $x=0, y=0$ and the lines $2x + y = 8$ and $x + 3y = 9$.
*   **Intersection of machine line with axes:** $(4, 0)$ and $(0, 8)$.
*   **Intersection of labor line with axes:** $(9, 0)$ and $(0, 3)$.
*   **Intersection of the two constraint lines:**
    Solve the system:
    $$\begin{cases} 2x + y = 8 \implies y = 8 - 2x \\ x + 3(8 - 2x) = 9 \implies -5x = -15 \implies x = 3 \end{cases}$$
    Substituting $x = 3$ gives $y = 2$. Intersection point is $(3, 2)$.

The vertices of the feasible region are:
1. $(0, 0)$
2. $(4, 0)$
3. $(3, 2)$
4. $(0, 3)$

### 3. Geometric Optimization
We evaluate the objective function $Z = 40x + 50y$ at each vertex:
*   At $(0, 0)$: $Z = 0$
*   At $(4, 0)$: $Z = 40(4) + 0 = 160$
*   At $(3, 2)$: $Z = 40(3) + 50(2) = 120 + 100 = 220$
*   At $(0, 3)$: $Z = 0 + 50(3) = 150$

**Optimal Production Plan:** Produce 3 units of $X$ and 2 units of $Y$ daily.
**Maximum Daily Profit:** \$220.
