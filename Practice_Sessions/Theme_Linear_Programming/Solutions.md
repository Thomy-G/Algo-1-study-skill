# 📝 Practice Session Solutions - Linear Programming

---

## Question 1: LP Formulations and Dualities

### Part a) [10 Points]
To formulate the Maximum Flow problem as an LP:
*   **Variables:** Let $f(u, v) \ge 0$ represent the flow on each directed edge $(u,v) \in E$.
*   **Objective Function:** Maximize the total flow leaving the source $s$:
    $$\text{Maximize } \sum_{v: (s, v) \in E} f(s, v) - \sum_{v: (v, s) \in E} f(v, s)$$
*   **Constraints:**
    1. **Capacity Constraints:** The flow on any edge cannot exceed its capacity:
       $$f(u, v) \le c(u, v) \quad \forall (u, v) \in E$$
    2. **Flow Conservation:** For any vertex other than the source $s$ and sink $t$, the incoming flow must equal the outgoing flow:
       $$\sum_{w: (w, u) \in E} f(w, u) = \sum_{v: (u, v) \in E} f(u, v) \quad \forall u \in V \setminus \{s, t\}$$
    3. **Non-negativity:**
       $$f(u, v) \ge 0 \quad \forall (u, v) \in E$$

### Part b) [10 Points]
To formulate the Single-Source Shortest Paths (SSSP) problem as an LP:
*   **Variables:** Let $d_v$ represent the shortest path distance estimate from $s$ to $v$ for each $v \in V$.
*   **Objective Function:** To push the distance estimates as high as possible until they hit the boundary constraints (the true shortest paths), we maximize the sum of all distance variables:
    $$\text{Maximize } \sum_{v \in V} d_v$$
*   **Constraints:**
    1. **Source Distance Constraint:** The distance to the source is 0:
       $$d_s \le 0$$
    2. **Triangle Inequality Constraints:** For any directed edge $(u, v) \in E$, the distance to $v$ cannot exceed the distance to $u$ plus the edge weight:
       $$d_v - d_u \le w(u, v) \quad \forall (u, v) \in E$$

### Part c) [10 Points]
*   **Dual LP Formulation:**
    Let $f(u, v) \ge 0$ be the dual variable associated with the triangle inequality constraint $d_v - d_u \le w(u, v)$.
    *   **Objective Function:**
        $$\text{Minimize } \sum_{(u, v) \in E} w(u, v) f(u, v)$$
    *   **Constraints:**
        For each vertex $v \neq s$, $d_v$ appears in constraints of the form $d_v - d_u \le w(u,v)$ (incoming) with coefficient $+1$, and $d_w - d_v \le w(v,w)$ (outgoing) with coefficient $-1$. Thus:
        $$\sum_{u: (u, v) \in E} f(u, v) - \sum_{w: (v, w) \in E} f(v, w) = 1 \quad \forall v \in V \setminus \{s\}$$
        For the source $s$:
        $$\sum_{w: (s, w) \in E} f(s, w) - \sum_{u: (u, s) \in E} f(u, s) = -(n - 1)$$
        $$f(u, v) \ge 0 \quad \forall (u, v) \in E$$
*   **Interpretation:**
    This dual LP represents the **Minimum Cost Flow** problem, where we want to send exactly 1 unit of flow from the source $s$ to every other vertex $v \in V \setminus \{s\}$ (a total of $n-1$ units leaving $s$) along the edges of the graph, while minimizing the total cost (defined by edge weights $w(u,v)$).

---

## Question 2: Seidel's Randomized LP Algorithm

### Part a) [10 Points]
- **Incremental violated step:**
  When constraint $h_i$ ($a_i^T x \le b_i$) is added, we evaluate if the current optimal solution $x_{i-1}$ satisfies $h_i$.
  - If $a_i^T x_{i-1} \le b_i$, the optimum remains unchanged: $x_i = x_{i-1}$.
  - If $a_i^T x_{i-1} > b_i$, the new optimal solution $x_i$ must lie on the boundary hyperplane $H_i = \{x \mid a_i^T x = b_i\}$.
- **Dimensional Reduction (Projection):**
  We project the objective function $c^T x$ and the remaining $i-1$ constraints onto the $(d-1)$-dimensional hyperplane $H_i$. We do this by expressing one of the variables in terms of the other $d-1$ variables using the equation $a_i^T x = b_i$ and substituting it into the remaining constraints and objective function. This yields a $(d-1)$-dimensional LP with $i-1$ constraints, which we solve recursively.

### Part b) [15 Points]
- Let $T(d, n)$ be the expected runtime of Seidel's algorithm in $d$ dimensions with $n$ constraints.
- **Backwards Analysis:** 
  The optimal solution $x_i$ in $d$ dimensions is uniquely determined by a basis of at most $d$ tight constraints.
  Since the permutation of the constraints is random, the probability that the $i$-th constraint is one of these $d$ basis constraints (which would trigger a recursive call in $d-1$ dimensions) is at most $d/i$.
- **Recurrence Relation:**
  $$T(d, n) \le T(d, n-1) + O(d) + \frac{d}{n} \cdot T(d-1, n-1)$$
- **Induction Proof:**
  We prove $T(d, n) \le C_d \cdot n$ where $C_d = O(d!)$ by induction on $d$:
  * **Base Case:** For $d=1$, $T(1, n) = O(n)$ (linear search).
  * **Inductive Step:** Assume $T(d-1, n) \le C_{d-1} n$. Then:
    $$T(d, n) \le T(d, n-1) + O(d) + \frac{d}{n} \left( C_{d-1} n \right) = T(d, n-1) + O(d) + d C_{d-1}$$
    Summing this recurrence from 1 to $n$:
    $$T(d, n) \le \sum_{i=1}^n \left( O(d) + d C_{d-1} \right) = \left( O(d) + d C_{d-1} \right) n$$
  * Thus, $C_d = O(d) + d C_{d-1}$. This recurrence solves to $C_d = O(d!)$.
  * Therefore, $T(d, n) = O(d! \cdot n)$ expected time. $\blacksquare$

### Part c) [10 Points]
1. **Base Case $d=1$:** The LP is in a single variable $x$. Constraints are inequalities of the form $x \ge l_i$ and $x \le r_i$. We solve this in $O(n)$ time by finding $L = \max_i l_i$ and $R = \min_i r_i$. If $L \le R$, the optimal solution is either $R$ or $L$ (depending on maximization/minimization); if $L > R$, the LP is infeasible.
2. **Base Case $n=0$:** There are no constraints. We return the optimal solution of the unconstrained LP (which is either at infinity or bounded by some initial bounding box) in $O(d)$ time.

---

## Question 3: Welzl's Randomized Smallest Enclosing Disc

### Part a) [10 Points]
Welzl's algorithm takes two inputs:
1. $P$: A set of points in the 2D plane.
2. $R$: A boundary support set containing up to 3 points that must lie on the boundary of the disc.
- **Algorithm flow:**
  `Welzl(P, R)`:
  * If $P = \emptyset$ or $|R| = 3$:
    * Return the unique circle defined by the support set $R$.
  * Choose a point $p \in P$ uniformly at random.
  * Compute the smallest enclosing disc $D = \text{Welzl}(P \setminus \{p\}, R)$.
  * If $p \in D$, return $D$.
  * If $p \notin D$, then $p$ must lie on the boundary of the smallest enclosing disc of $P$. Return $\text{Welzl}(P \setminus \{p\}, R \cup \{p\})$.

### Part b) [15 Points]
- Let $T(n, i)$ be the expected time to solve with $|P|=n$ and $|R|=i$.
- **Recurrence Relation:**
  $$T(n, i) \le T(n-1, i) + O(1) + \Pr[p \notin D_{P \setminus \{p\}, R}] \cdot T(n-1, i+1)$$
- **Backwards Analysis:** The smallest enclosing disc of $P$ with boundary $R$ is uniquely determined by a support set of at most $3 - i$ points of $P$. Since $p$ is chosen uniformly at random, the probability that $p$ is in this support set is at most $\frac{3-i}{n}$.
- The recurrence becomes:
  $$T(n, i) \le T(n-1, i) + O(1) + \frac{3-i}{n} \cdot T(n-1, i+1)$$
- We solve by induction on $3-i$:
  * **For $i=3$:** $T(n, 3) = T(n-1, 3) + O(1) = O(n)$ (base case).
  * **For $i=2$:**
    $$T(n, 2) \le T(n-1, 2) + O(1) + \frac{1}{n} O(n) = T(n-1, 2) + O(1) \implies T(n, 2) = O(n)$$
  * **For $i=1$:**
    $$T(n, 1) \le T(n-1, 1) + O(1) + \frac{2}{n} O(n) = T(n-1, 1) + O(1) \implies T(n, 1) = O(n)$$
  * **For $i=0$:**
    $$T(n, 0) \le T(n-1, 0) + O(1) + \frac{3}{n} O(n) = T(n-1, 0) + O(1) \implies T(n, 0) = O(n)$$
  Thus, Welzl's algorithm runs in expected $O(n)$ time. $\blacksquare$

### Part c) [10 Points]
- **Support Set Concept:** The support set is the minimal subset of points in $P$ that uniquely determines the smallest enclosing disc. Deleting any point not in the support set does not change the resulting smallest enclosing disc.
- **Why maximum size is 3:** A circle in 2D is defined by 3 parameters (center $x, y$ and radius $r$). Thus, it has 3 degrees of freedom and is uniquely determined by at most 3 boundary points.
- **Base Case calculations:**
  * **$|R|=1$ ($R=\{p_1\}$):** The disc is the point $p_1$ itself (radius 0).
  * **$|R|=2$ ($R=\{p_1, p_2\}$):** The disc has the segment $p_1 p_2$ as its diameter. Center is $C = (p_1 + p_2)/2$, radius is $r = \|p_1 - p_2\|/2$.
  * **$|R|=3$ ($R=\{p_1, p_2, p_3\}$):** The disc is the circumcircle of the triangle $p_1 p_2 p_3$. The center is the circumcenter (intersection of the perpendicular bisectors of the triangle edges), and the radius is the distance from the circumcenter to any of the three points.
