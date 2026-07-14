# Algorithms 1 (89-220) - Theme: Linear Programming - Solutions Manual

This document contains the official, rigorous solutions for the Linear Programming & Welzl's Algorithm practice session.

---

## Question 1: LP Formulations and Feasible Regions (30 Points)

### Part a) [10 Points]
**LP Formulation in Standard Maximization Form:**
Let $x_1$ be the number of Dijkstra-Bots produced daily, and $x_2$ be the number of FFT-Bots produced daily.
*   **Objective Function:** Maximize the daily profit:
    $$\text{Maximize } Z = 30x_1 + 40x_2$$
*   **Constraints:**
    - RAM constraint: $2x_1 + x_2 \le 8$
    - CPU constraint: $x_1 + 3x_2 \le 9$
    - Non-negativity constraints: $x_1 \ge 0, \quad x_2 \ge 0$

---

### Part b) [10 Points]
**Feasible Region and Vertices:**
The feasible region is bounded by the axes $x_1 = 0, x_2 = 0$ and the two lines $2x_1 + x_2 = 8$ and $x_1 + 3x_2 = 9$.
1.  **Intersection with axes:**
    - Line 1: $2x_1 + x_2 = 8 \implies$ intersects $x_1$-axis at $(4,0)$ and $x_2$-axis at $(0,8)$.
    - Line 2: $x_1 + 3x_2 = 9 \implies$ intersects $x_1$-axis at $(9,0)$ and $x_2$-axis at $(0,3)$.
2.  **Intersection of the two constraint lines:**
    Solve the system:
    $$\begin{cases} 2x_1 + x_2 = 8 \\ x_1 + 3x_2 = 9 \end{cases}$$
    Multiply the second equation by 2:
    $$2x_1 + 6x_2 = 18$$
    Subtract the first equation:
    $$5x_2 = 10 \implies x_2 = 2$$
    Substitute back to find $x_1$:
    $$x_1 + 3(2) = 9 \implies x_1 = 3$$
    The intersection point is $(3,2)$.
3.  **Vertices of the Feasible Region:**
    The vertices defining the corners of the convex feasible region are:
    - $V_1 = (0, 0)$
    - $V_2 = (4, 0)$
    - $V_3 = (3, 2)$
    - $V_4 = (0, 3)$

---

### Part c) [10 Points]
**Optimal Solution:**
We evaluate the objective function $Z(x_1, x_2) = 30x_1 + 40x_2$ at each vertex:
- $Z(0, 0) = 30(0) + 40(0) = 0$ NIS
- $Z(4, 0) = 30(4) + 40(0) = 120$ NIS
- $Z(3, 2) = 30(3) + 40(2) = 90 + 80 = 170$ NIS
- $Z(0, 3) = 30(0) + 40(3) = 120$ NIS

**Conclusion:**
The optimal production plan is to produce **3 Dijkstra-Bots** and **2 FFT-Bots** daily, yielding a maximum profit of **170 NIS**.

---

## Question 2: Seidel's Randomized LP Algorithm (35 Points)

### Part a) [15 Points]
**Recursive Step in Seidel's Algorithm:**
1. Seidel's algorithm randomly permutes the constraints.
2. In step $i$, we recursively find the optimal vertex $v_{i-1}$ for the LP with the first $i-1$ constraints.
3. We then check if the $i$-th constraint $h_i$ is satisfied by $v_{i-1}$.
   - **Case 1 (Satisfied):** If $v_{i-1} \in h_i$, then $v_i = v_{i-1}$. No work is needed.
   - **Case 2 (Violated):** If $v_{i-1} \notin h_i$, the new optimal vertex $v_i$ must lie exactly on the boundary hyperplane of $h_i$. We project the objective vector and the first $i-1$ constraints onto the $(d-1)$-dimensional boundary of $h_i$, and recursively solve this $(d-1)$-dimensional LP.

---

### Part b) [20 Points]
**Recurrence Relation:**
Let $T(d, n)$ be the expected runtime.
$$T(d, n) \le T(d, n-1) + O(d) + P(\text{violation}) \cdot T(d-1, n-1)$$
**Backward Analysis:**
The optimal solution $v_n$ of the LP is uniquely determined by a set of at most $d$ tight constraints. The probability that the randomly chosen constraint $h$ is one of these $d$ defining constraints (meaning its removal would change the optimal solution, which is the definition of violation in backward analysis) is at most:
$$P(\text{violation}) \le \frac{d}{n}$$
Thus:
$$T(d, n) \le T(d, n-1) + O(d) + \frac{d}{n} T(d-1, n-1)$$
Solving this recurrence by induction on $d$:
- For $d=1$: $T(1, n) \le T(1, n-1) + O(1) \in O(n)$.
- Assuming $T(d-1, n) \le C_{d-1} n$:
  $$T(d, n) \le T(d, n-1) + O(d) + \frac{d}{n} (C_{d-1} n) = T(d, n-1) + O(d) + d \cdot C_{d-1}$$
  Summing this over $n$ steps:
  $$T(d, n) \le n \cdot (O(d) + d \cdot C_{d-1}) \in O(n)$$
For any constant dimension $d$, the expected runtime is $O(n)$ (linear). $\blacksquare$

---

## Question 3: Welzl's Smallest Enclosing Disc (35 Points)

### Part a) [15 Points]
**Role of the Support Set $R$:**
- The support set $R$ contains the points that are forced to lie on the boundary of the smallest enclosing disc.
- A circle in 2D space is uniquely defined by at most 3 points (either 2 points forming the diameter, or 3 points passing through the boundary).
- Therefore, the boundary set $R$ can contain at most 3 points. Any additional point would be redundant or mathematically impossible to lie on the boundary of a single circle.

---

### Part b) [20 Points]
**Proof of expected $O(n)$ runtime:**
Let $T(i, j)$ be the expected runtime of `Welzl(P, R)` where $i = |P|$ is the number of points and $j = |R|$ is the size of the support set.
- The recurrence relation is:
  $$T(i, j) = T(i-1, j) + O(1) + P(\text{point outside disc}) \cdot T(i-1, j+1)$$
- **Backward Analysis:**
  The smallest enclosing disc of the $i$ points is uniquely determined by at most $3 - j$ points of $P$ that lie on its boundary. The probability that the randomly chosen point $p$ removed in the step is one of these defining points is at most $\frac{3-j}{i}$.
  Thus:
  $$T(i, j) \le T(i-1, j) + O(1) + \frac{3-j}{i} T(i-1, j+1)$$
- Since $j$ can reach at most 3, the recursion terminates at $j=3$ where $T(i, 3) = O(i)$.
- Solving this from $j=3$ downwards:
  - $T(i, 3) \in O(i)$
  - $T(i, 2) \le T(i-1, 2) + O(1) + \frac{1}{i} O(i) \implies T(i, 2) \in O(i)$
  - $T(i, 1) \le T(i-1, 1) + O(1) + \frac{2}{i} O(i) \implies T(i, 1) \in O(i)$
  - $T(i, 0) \le T(i-1, 0) + O(1) + \frac{3}{i} O(i) \implies T(i, 0) \in O(i)$
Thus, Welzl's algorithm runs in expected $O(n)$ time. $\blacksquare$
