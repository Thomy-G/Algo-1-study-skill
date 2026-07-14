---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-07-11
---
# Module 10 - Linear Programming in Low Dimension, Seidel's Randomized LP, and Smallest Enclosing Discs

**MOC:** [[Algorithms 1 MOC]]  
**Course:** [[Algorithms 1]]  

# Algorithms 1: Ultimate Study Guide

## Module 10: Linear Programming in Low Dimension & Smallest Enclosing Discs

This module compiles the geometric aspects of Linear Programming (LP) in low dimensions and Welzl's smallest enclosing discs algorithm, spanning **Syllabus Week 12**. It details Seidel's randomized incremental LP algorithm, higher-dimensional extensions, and the class of LP-type problems.

---

### 1. Foundational Vocabulary & Problem Geometry

#### 1.1 Geometric Primitives & Definitions
To understand low-dimensional linear programming and computational geometry, we define the following spatial primitives in $\mathbb{R}^d$:

*   **Hyperplane (על-מישור):** A flat $(d-1)$-dimensional subspace in $\mathbb{R}^d$. Formally, it is the set of points $\vec{x}$ satisfying the linear equation:
    $$\vec{a} \cdot \vec{x} = b$$
    where $\vec{a} \in \mathbb{R}^d \setminus \{\vec{0}\}$ is the normal vector, and $b \in \mathbb{R}$. In 2D, a hyperplane is simply a straight line ($ax + by = c$).
*   **Half-space (חצי מרחב) / Half-plane (חצי מישור):** One of the two regions into which a hyperplane divides space. Formally, a closed half-space is the set of points satisfying a linear inequality:
    $$\vec{a} \cdot \vec{x} \le b \quad \text{or} \quad \vec{a} \cdot \vec{x} \ge b$$
    In 2D, this is called a **half-plane**, which is bounded by a 2D line. A constraint in a linear program is geometrically represented as a closed half-space.
*   **Convexity (קמירות):** A set $S \subseteq \mathbb{R}^d$ is **convex** if, for any two points $p, q \in S$, the entire line segment connecting them lies completely inside $S$:
    $$\lambda p + (1 - \lambda)q \in S \quad \forall \lambda \in [0, 1]$$
    *   *Examples:* Polygons, discs, and half-spaces are convex. A bean-like or star shape is non-convex.
*   **Intersection of Half-spaces:** The intersection of any collection of convex sets is always convex. Since half-spaces are convex, the feasible region of any linear program (which is the intersection of the constraint half-spaces) is a **convex set**.
*   **Polyhedron (פאון):** A convex set formed by the intersection of a finite number of closed half-spaces.
*   **Polytope (פוליטופ):** A bounded polyhedron (meaning it does not extend infinitely in any direction, lying entirely within some sphere of finite radius).
*   **Ray (קרן) / Half-line:** A portion of a line starting at a point $p$ and extending infinitely in a single direction $\vec{v}$:
    $$\{\vec{p} + \lambda \vec{v} \mid \lambda \ge 0\}$$
*   **Line Segment (קטע):** The set of points connecting two endpoints $p$ and $q$:
    $$\{\lambda p + (1 - \lambda)q \mid \lambda \in [0, 1]\}$$

#### 1.2 Linear Program Definition
A **Linear Program (LP)** is an optimization problem where we seek to maximize (or minimize) a linear objective function subject to a set of linear inequality constraints:
$$\text{Maximize } \vec{c} \cdot \vec{x} = c_1 x_1 + c_2 x_2 + \dots + c_d x_d$$
$$\text{Subject to: } a_{i,1} x_1 + \dots + a_{i,d} x_d \le b_i \quad \text{for } i = 1, \dots, n$$

*   **Feasible Region ($C$):** The intersection of the half-spaces defined by the constraints. Since each half-space is convex, the feasible region is a **convex polytope**.
*   **Feasible Solution:** Any point $\vec{x}$ lying inside the feasible region $C$.
*   **Infeasible LP:** An LP where the feasible region is empty ($C = \emptyset$).
*   **Unbounded LP:** An LP where the objective function can grow infinitely large within the feasible region ($f_{\vec{c}}(\vec{x}) \to \infty$).
*   **Objective Vector Assumption:** Without loss of generality, we can assume the objective vector $\vec{c}$ points in the negative $y$-direction (equivalent to rotating the coordinate axis system). Our goal is to find the lowest feasible point in the vertical direction.

#### 1.3 Geometrical Classifications of Solutions
For a 2D linear program, the solution fits one of four outcomes:
1.  **Infeasible:** $C = \emptyset$.
2.  **Unbounded:** There exists a ray $\rho = \{p + \lambda \vec{d} \mid \lambda > 0\} \subseteq C$ such that $\vec{d} \cdot \vec{c} > 0$.
3.  **Non-unique optimal:** The objective vector $\vec{c}$ is orthogonal to a boundary edge of $C$. Any point on this edge is optimal.
4.  **Unique optimal:** A single extreme vertex $v \in C$ that maximizes $\vec{c} \cdot \vec{v}$.

> [!NOTE]
> To handle non-unique solutions and guarantee a single well-defined optimal vertex, we enforce a **lexicographical comparison**: if two vertices have the same objective value, we choose the lexicographically smaller one (equivalent to rotating the objective vector $\vec{c}$ slightly).


---

### 2. 1-Dimensional Linear Programming

A 1-dimensional LP seeks to find a value $x \in \mathbb{R}$ that maximizes a linear objective function subject to interval constraints:
$$\text{Maximize } c \cdot x$$
$$\text{Subject to: } x \ge \sigma_l \quad (\text{left-bounded constraints})$$
$$x \le \sigma_r \quad (\text{right-bounded constraints})$$

#### 2.1 The $O(n)$ Direct Solver
1.  Initialize $x_{\text{left}} = -\infty$ and $x_{\text{right}} = \infty$.
2.  Iterate through all constraints:
    *   For each left constraint $x \ge \sigma_i$: update $x_{\text{left}} = \max(x_{\text{left}}, \sigma_i)$.
    *   For each right constraint $x \le \sigma_i$: update $x_{\text{right}} = \min(x_{\text{right}}, \sigma_i)$.
3.  **Feasibility Check:** If $x_{\text{left}} > x_{\text{right}}$, the 1D program is **infeasible**.
4.  **Optimality:** If feasible, the optimal point is either $x_{\text{left}}$ or $x_{\text{right}}$, depending on the sign of the objective coefficient $c$ (if $c > 0$, the optimum is $x_{\text{right}}$; if $c < 0$, it is $x_{\text{left}}$).
5.  **Complexity:** Single pass scanning of $n$ elements $\implies \mathbf{O(n)}$ time.

---

### 3. 2-Dimensional Bounded Linear Programming

In low-dimensional settings (where the dimension $d$ is a small constant, e.g., $d=2$), geometric incremental algorithms are far simpler and faster than general operations-research solvers (like Simplex).

#### 3.1 Bounding the Region
To prevent unbounded LPs from causing initialization failures, we add two artificial constraints:
$$M_{1}=\begin{cases}
x\le M & c_{x}>0\\
x\ge-M & c_{x} \le 0
\end{cases}, \quad
M_{2}=\begin{cases}
y\le M & c_{y}>0\\
y\ge-M & c_{y} \le 0
\end{cases}$$
for a sufficiently large constant $M$. This acts as a bounding box ensuring that $C_0 = M_1 \cap M_2$ is bounded and has a unique optimal vertex $v_0$.

#### 3.2 The Incremental Step Invariant
Let $H_i = \{M_1, M_2, h_1, \dots, h_i\}$ be the set containing the bounding constraints and the first $i$ constraints. Let $C_i$ be the feasible region and $v_i$ be the optimal vertex of $C_i$.
*   **Lemma (Incremental Update):**
    1.  **Case A (No change):** If $v_{i-1} \in h_i$, then $v_i = v_{i-1}$.
    2.  **Case B (Reduction to 1D):** If $v_{i-1} \notin h_i$, then either $C_i = \emptyset$ (infeasible), or the new optimal vertex $v_i$ must lie on the bounding line $\ell_i$ of the half-plane $h_i$.

##### Geometrical Proof of Case B by Contradiction:
1.  Assume for sake of contradiction that the new optimal point $v_i$ does not lie on the boundary line $\ell_i$. This means $v_i$ lies strictly in the interior of the half-plane $h_i$ (so $v_i \notin \ell_i$).
2.  Consider the line segment connecting the old optimal point $v_{i-1}$ and the new optimal point $v_i$:
    $$S = \{ \lambda v_{i-1} + (1-\lambda) v_i \mid \lambda \in [0, 1] \}$$
3.  Since both $v_{i-1}$ and $v_i$ are in the feasible region $C_{i-1}$, and $C_{i-1}$ is convex, the entire segment $S$ lies completely inside $C_{i-1}$.
4.  Because the objective function $f_{\vec{c}}(p) = \vec{c} \cdot p$ is linear, its value changes at a constant rate along the straight line segment $S$. Since $v_{i-1}$ is the unique maximum point in $C_{i-1}$, the value of the objective function must strictly decrease as we walk along the segment from $v_{i-1}$ towards $v_i$.
5.  Since the old point $v_{i-1}$ violates the new constraint ($v_{i-1} \notin h_i$) and the new point $v_i$ satisfies it ($v_i \in h_i$), the segment connecting them must cross the boundary line $\ell_i$ of $h_i$ at some point $q \in S \cap \ell_i$.
6.  Because $q$ lies on the segment $S$ strictly between $v_{i-1}$ and $v_i$, it is closer to the maximum point $v_{i-1}$ than $v_i$ is. Since the objective value decreases monotonically along the path $v_{i-1} \to q \to v_i$, we have:
    $$f_{\vec{c}}(q) > f_{\vec{c}}(v_i)$$
7.  Since $q \in C_{i-1}$ and $q \in \ell_i \subset h_i$, it satisfies all constraints of $C_i$, meaning $q \in C_i$ is a feasible point.
8.  Thus, we have found a feasible point $q \in C_i$ that has a strictly higher objective value than our supposed optimal point $v_i$. This is a logical contradiction.
9.  Therefore, the assumption was false, and the new optimal point $v_i$ must lie on the bounding line $\ell_i$. $\blacksquare$

*   **1D Solver Reduction:** To find the optimal point $v_i$ on $\ell_i$, we intersect all previous $i-1$ half-plane constraints $h_j$ with the line $\ell_i$. Since each $h_j$ is a 2D half-space, its intersection with the 1D line $\ell_i$ is a 1D ray (a bounded half-line). Assuming $\ell_i$ is not vertical (parameterized by $x$), we compute:
    $$x_{\text{left}} = \max \{ x(\ell_i \cap h_j) \mid 1 \le j < i, \text{where } \ell_i \cap h_j \text{ is left-bounded} \}$$
    $$x_{\text{right}} = \min \{ x(\ell_i \cap h_j) \mid 1 \le j < i, \text{where } \ell_i \cap h_j \text{ is right-bounded} \}$$
    *   If $x_{\text{left}} > x_{\text{right}}$, then $C_i = \emptyset$ (report infeasibility).
    *   Otherwise, the optimal point $v_i$ is the endpoint of the segment $[x_{\text{left}}, x_{\text{right}}]$ that maximizes the objective function.

```
    Case A: v_{i-1} satisfies h_i            Case B: v_{i-1} violates h_i
                                               New optimum v_i MUST lie on boundary l_i
           \       /                                \   l_i   /
            \     /                                  \   *v_i/
             *v_{i-1} = v_i                           \     /
          --------------                               \   /
              h_i                                  ----*v_{i-1}-------
                                                       h_i
```

#### 3.3 The Solver Loop
1.  Start with optimal vertex $v_0$ of $C_0$.
2.  For $i = 1$ to $n$:
    *   If $v_{i-1}$ satisfies $h_i$: set $v_i = v_{i-1}$ (takes $O(1)$ time).
    *   Otherwise ($v_{i-1}$ violates $h_i$):
        *   Solve a 1D LP on the line $\ell_i$ using the previous $i-1$ constraints to restrict the feasible interval along $\ell_i$.
        *   If the 1D LP is infeasible, report that the 2D LP is **infeasible** and terminate.
        *   Otherwise, set $v_i$ to the new 1D optimal point.

#### 3.4 Worst-Case Time Complexity
In the worst case, every new constraint violates the previous optimal vertex, triggering a 1D solver execution at each step:
$$T(n) = \sum_{i=1}^n O(i) = \mathbf{O(n^2)} \quad \text{time.}$$

---

### 4. Seidel's Randomized $O(n)$ Expected Time Algorithm

The $O(n^2)$ worst-case bottleneck is entirely order-dependent. If we shuffle the constraints, we can prevent pathological orderings.

#### 4.1 Algorithm Description
1.  Initialize $v_0$ as the corner of $C_0 = m_1 \cap m_2$.
2.  Compute a **random permutation** $h_1, \dots, h_n$ of the input constraints $H$.
3.  Iterate through the permuted constraints:
    *   If $v_{i-1} \in h_i$: set $v_i = v_{i-1}$.
    *   Else: solve the 1D LP on $\ell_i$ subject to $H_{i-1}$. If infeasible, terminate. Otherwise, update $v_i$.
4.  Return $v_n$.

#### 4.2 Backwards Analysis Proof
We bound the expected running time by analyzing the algorithm in reverse:
1.  Let $X_i$ be an indicator random variable:
    $$X_i = I\{v_{i-1} \notin h_i\}$$
2.  The total time spent solving 1D LPs is $\sum_{i=1}^n O(i) \cdot X_i$. By linearity of expectation:
    $$E[\text{Total Time}] = \sum_{i=1}^n O(i) \cdot E[X_i] = \sum_{i=1}^n O(i) \cdot \Pr(v_{i-1} \notin h_i)$$
3.  **Backwards Invariant:** Fix the set of the first $i$ constraints $H_i$. This determines the feasible region $C_i$ and the unique optimal vertex $v_i$.
4.  The optimal vertex $v_i$ is at the intersection of at least 2 constraint lines in $H_i$.
5.  If we transition backwards from step $i$ to step $i-1$, we remove a random constraint $h_i$ from $H_i$.
6.  The optimal vertex changes (meaning $v_{i-1} \neq v_i$) if and only if the removed constraint $h_i$ was one of the constraints defining $v_i$.
7.  Since $h_i$ is chosen uniformly at random from the $i$ constraints in $H_i$, the probability that we remove one of the defining constraints of $v_i$ is at most:
    $$\Pr(v_{i-1} \notin h_i) \le \frac{2}{i}$$
8.  Substituting this back:
    $$E[\text{Total Time}] \le \sum_{i=1}^n O(i) \cdot \frac{2}{i} = \sum_{i=1}^n O(1) = \mathbf{O(n)} \quad \text{expected time.}$$

---

### 5. Handling Unbounded LPs and Finding Certificates

Instead of using artificial bounds $M$, we can detect unbounded LPs and find **certificates** of boundedness.

#### 5.1 Boundedness Condition
By Lemma 4.9, a 2D LP $(H, \vec{c})$ is unbounded if and only if:
1.  There exists a direction vector $\vec{d}$ such that $\vec{d} \cdot \vec{c} > 0$ and $\vec{d} \cdot \vec{\eta}(h) \ge 0$ for all $h \in H$ (where $\vec{\eta}(h)$ is the inward normal of $h$).
2.  The program restricted to the tight constraints $H' = \{h \in H \mid \vec{\eta}(h) \cdot \vec{d} = 0\}$ is feasible.

#### 5.2 Certificate Extraction
If the direction vector $\vec{d}$ does not exist, the LP is bounded. The 1D search for $\vec{d}$ will fail because the intersection of the constraints on the direction line is empty.
*   The empty intersection is caused by a pair of constraints $h_j, h_k$ whose normal directions oppose each other relative to $\vec{c}$.
*   These two half-planes $h_j, h_k$ act as **certificates** of boundedness. They guarantee that the sub-program $(\{h_j, h_k\}, \vec{c})$ is bounded and has a unique optimal vertex.
*   **Safe Execution:** We can run the randomized incremental algorithm by initializing $C_2 = h_j \cap h_k$ and setting $v_2$ as their intersection point, bypassing the need for artificial $M$-wedges.

---

### 6. Higher Dimensional Linear Programming

For a $d$-dimensional space, Seidel's algorithm generalizes recursively:
1.  Verify if the $d$-dimensional LP is bounded. If so, identify $d$ certificate half-spaces $h_1, \dots, h_d$ that bound the solution.
2.  Set the initial vertex $v_d$ as the intersection of the bounding hyperplanes $g_1, \dots, g_d$.
3.  Permute the remaining $n - d$ constraints randomly.
4.  For $i = d + 1$ to $n$:
    *   If $v_{i-1} \in h_i$: set $v_i = v_{i-1}$.
    *   Else ($v_{i-1} \notin h_i$): the new optimum must lie on the $(d-1)$-dimensional hyperplane $g_i$ bounding $h_i$.
    *   To find $v_i$, project the objective vector onto $g_i$, intersect all previous $i-1$ constraints with $g_i$ (producing a $(d-1)$-dimensional program), and call the solver recursively.

#### 6.1 Complexity Analysis & Recurrence Proof
Using backwards analysis, the probability that a randomly chosen constraint $h_i$ is one of the $d$ hyperplanes defining the optimal vertex is at most $\frac{d}{i}$. This yields the recurrence relation for the expected runtime of Seidel's algorithm in $d$ dimensions:
$$T(d,n) \le O(d \cdot n) + \sum_{i=1}^n \frac{d}{i} \cdot T(d-1, i-1)$$

##### Inductive Proof that $T(d,n) = O(d! \cdot n)$:
1.  **Inductive Hypothesis:** Assume that for dimension $d-1$ and any number of constraints, the recurrence holds:
    $$T(d-1, i-1) \le c' \cdot (d-1)! \cdot (i-1)$$
    for some constant $c'$.
2.  Substitute the hypothesis into the recurrence relation for $T(d,n)$:
    $$T(d,n) \le c_1 \cdot d \cdot n + \sum_{i=1}^n \frac{d}{i} \cdot c_2 \cdot (d-1)! \cdot (i-1)$$
3.  Group the constants and factorial terms:
    $$T(d,n) \le c_1 \cdot d \cdot n + c_2 \cdot d! \sum_{i=1}^n \frac{i-1}{i}$$
4.  Since $\frac{i-1}{i} < 1$ for all $i \ge 1$, we bound the summation:
    $$\sum_{i=1}^n \frac{i-1}{i} < \sum_{i=1}^n 1 = n$$
5.  This yields:
    $$T(d,n) \le c_1 \cdot d \cdot n + c_2 \cdot d! \cdot n$$
6.  Since $d! \cdot n$ grows much faster than $d \cdot n$, the factorial term dominates:
    $$T(d,n) = \mathbf{O(d! \cdot n)} \quad \text{expected time.} \quad \blacksquare$$


---

### 7. LP-Type Problems: Smallest Enclosing Discs (Welzl's Algorithm)

The randomized incremental framework applies to a broader class of optimization problems called **LP-type problems**. These are problems characterized by:
1.  **Monotonicity:** Adding constraints cannot improve (reduce) the optimal value.
2.  **Locality:** If the optimum for a subset is valid for a new constraint, it remains the optimum for the expanded set.

#### 7.1 Smallest Enclosing Disc (Welzl's Algorithm)
Given $n$ points in the plane, find the unique disc of minimum radius that encloses them.
*   **The Invariant (Lemma 4.14):**
    *   Let $D_{i-1}$ be the smallest enclosing disc of the first $i-1$ points.
    *   If the next point $p_i \in D_{i-1}$, the disc remains the same ($D_i = D_{i-1}$).
    *   If $p_i \notin D_{i-1}$, the point $p_i$ must lie on the **boundary** of the new smallest enclosing disc $D_i$.

```
    Case 1: p_i lies inside D_{i-1}            Case 2: p_i lies outside D_{i-1}
                                               New disc D_i must have p_i on boundary
            .------.                                 .------.
          /          \                             /          \  p_i
         |    *p_i    |  D_{i-1} = D_i            |     *      *---
          \          /                             \          /  \ \
            '------'                                 '------'     '-- D_i
```

#### 7.2 Welzl's Recursive Structure
To enforce boundary constraints, the solver carries a set $R$ of points that must lie on the boundary ($|R| \le 3$, since 3 points uniquely define a circle):

```
WELZL(P, R):
    if P is empty or |R| == 3:
        return disc defined by R (if |R| < 2, trivial cases)
    Choose a random point p from P
    D = WELZL(P \ {p}, R)
    if p lies inside D:
        return D
    else:
        return WELZL(P \ {p}, R U {p})
```

#### 7.3 Backwards Analysis Proof
1.  At step $i$, the smallest enclosing disc is defined by at most 3 boundary points.
2.  If we remove a random point $p_i$, the disc changes only if $p_i$ was one of the boundary points.
3.  The probability of the boundary changing is at most $3/i$.
4.  This yields a nested expected running time of:
    $$E[T(n)] = \mathbf{O(n)} \quad \text{expected time and linear storage.}$$

---

### 8. Summary Table of Linear Programming & Geometric Bounds

| Problem / Algorithm | Dimension ($d$) | Deterministic Time | Randomized Expected Time | Success / Error Metrics |
| :--- | :--- | :--- | :--- | :--- |
| **1D Linear Program** | $d = 1$ | $O(n)$ | $\mathbf{O(n)}$ | Trivial, exact. |
| **2D Incremental LP** | $d = 2$ | $O(n^2)$ worst case | $\mathbf{O(n)}$ expected | $0$ error probability. |
| **d-Dim Seidel's LP** | Arbitrary $d$ | $O(n^{\lfloor d/2 \rfloor})$ | $\mathbf{O(d! \cdot n)}$ expected | $0$ error probability. |
| **Smallest Enclosing Disc** | $d = 2$ (circle) | $O(n \log n)$ | $\mathbf{O(n)}$ expected | $0$ error (Welzl's). |

---
## 🔗 Navigation
**Previous:** [[Module 9 - Randomized Verification, Advanced Probabilistic Invariants, and Distributed Models]] | **Next:** [[ ]]
