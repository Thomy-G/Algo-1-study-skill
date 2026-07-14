# Algorithms 1 (89-220) - 2024 Winter Moed A Solutions Manual

This document contains the official, rigorous solutions for the 2024 Winter Moed A exam questions.

---

## Question 1: FFT, Roots of Unity, and Sparse Multiplication (20 Points)

### Part a) [5 Points]
**Weak Duality Theorem:**
For any feasible solution $x$ to the primal linear program (maximization) and any feasible solution $y$ to the dual linear program (minimization), the primal objective value is at most the dual objective value:
$$c^T x \le b^T y$$

---

### Part b) [5 Points]
**Sum of $n$ roots of unity:**
The sum of the $n$ roots of unity $\omega_n^k = e^{2\pi i k / n}$ for $k = 0, \dots, n-1$ is exactly **0** for any $n > 1$.
**Proof:**
Using the geometric series formula:
$$\sum_{k=0}^{n-1} (\omega_n)^k = rac{1 - (\omega_n)^n}{1 - \omega_n} = rac{1 - 1}{1 - \omega_n} = 0 \quad (	ext{since } \omega_n 
e 1 	ext{ for } n > 1)$$

---

### Part c) [5 Points]
**Product of $n$ roots of unity:**
The product is $(-1)^{n-1}$.
**Proof:**
The roots are the roots of the polynomial $x^n - 1 = 0$. Factoring the polynomial:
$$x^n - 1 = \prod_{k=0}^{n-1} (x - \omega_n^k)$$
Comparing the constant term: the constant term of the right side is $(-1)^n \prod_{k=0}^{n-1} \omega_n^k$. The constant term of $x^n - 1$ is $-1$. Thus:
$$(-1)^n \prod_{k=0}^{n-1} \omega_n^k = -1 \implies \prod_{k=0}^{n-1} \omega_n^k = (-1)^{n-1}$$

---

### Part d) [5 Points]
**Algorithm Description:**
Since $B(x)$ is $O(1)$-sparse, it has at most some constant $d$ non-zero terms: $B(x) = \sum_{j=1}^d b_{k_j} x^{k_j}$.
1. Initialize the product polynomial $C(x)$ of degree $2n$ with all coefficients set to 0.
2. For each non-zero term $b_{k_j} x^{k_j}$ of $B(x)$:
   - For each term $a_i x^i$ of $A(x)$:
     - Increment the coefficient of $C(x)$ at index $i + k_j$ by $a_i \cdot b_{k_j}$.
3. Return $C(x)$.

**Complexity:**
- Since $d$ is $O(1)$, the outer loop runs $O(1)$ times. The inner loop runs $n+1$ times.
- **Total Time Complexity:** $O(n)$ time.
- **Total Space Complexity:** $O(n)$ space.

---

## Question 2: Kruskal's Edge Sorting (20 Points)

**Claim:** True.
**Proof:**
1. Let $T$ be an MST of $G$.
2. We construct the sorted permutation $\pi_T$ as follows:
   - For any two edges $e_1, e_2 \in E$:
     - If $w(e_1) < w(e_2)$, we must place $e_1$ before $e_2$ in $\pi_T$.
     - If $w(e_1) == w(e_2)$, and one of them belongs to $T$ (say $e_1 \in T$ and $e_2 
otin T$), we break the tie by placing $e_1$ before $e_2$ in $\pi_T$.
3. When Kruskal's algorithm runs on this permutation:
   - Since edges in $T$ are processed before any other equal-weight edges, they will not be excluded by cycle formations with equal-weight non-tree edges.
   - Thus, Kruskal's algorithm will greedily select all edges of $T$, returning $T$. $lacksquare$

---

## Question 3: Last Edge in Shortest Path DAG (20 Points)

### Algorithm Description
1. Run SSSP from $s$ on $G$ to get distances $\delta(s, x)$ for all $x \in V$.
2. Check if the edge $(u,v)$ satisfies:
   $$\delta(s, u) + w(u,v) == \delta(s, v)$$
3. If it does, return `True`; otherwise, return `False`.

**Correctness:**
An edge $(u,v)$ is the last edge of some shortest path $P$ from $s$ to $v$ if and only if the shortest path distance to $v$ can be obtained via $u$: $\delta(s,v) = \delta(s,u) + w(u,v)$. This condition is exactly checked by SSSP.

**Complexity:**
- **Time Complexity:** $O(|E| + |V| \log |V|)$ using Dijkstra's algorithm.
- **Space Complexity:** $O(|V| + |E|)$.

---

## Question 4: Balanced Global Min Cut (20 Points)

### Part a) [10 Points]
**Algorithm Description:**
1. Select a vertex $v \in V$ uniformly at random.
2. Since $G$ is $1/3$-balanced, there is a min cut $(S, V \setminus S)$ such that $S$ contains at least $1/3$ of the vertices and at most $2/3$.
3. Run the S-T Min Cut algorithm (using max flow) between $v$ and all other vertices $u \in V \setminus \{v\}$.
4. Return the minimum cut value found.

**Proof of Success Probability:**
With probability at least $1/3$, the randomly chosen $v$ belongs to $S$. With probability at least $1/3$, another chosen $u$ belongs to $V \setminus S$. Thus, running S-T Min Cut will find the global min cut. The probability of success is at least a constant.

**Complexity:**
- Running S-T Min Cut $n$ times takes $O(n \cdot |V|^2 |E|) = O(|V|^3 |E|)$ time.

---

### Part b) [10 Points]
To achieve high probability, we repeat the random selection $C \log |V|$ times.
- **Total Time Complexity:** $O(|V|^3 |E| \log |V|)$ time.

---

## Question 5: Dynamic Programming Edit Distance (20 Points)

### Recursive Formula
Let $DP[i, j]$ be the maximum alignment score between $S[1 \dots i]$ and $T[1 \dots j]$.
- **Base Cases:**
  - $DP[0, 0] = 0$.
  - $DP[i, 0] = -i$.
  - $DP[0, j] = -j$.
- **Recursive Step:**
  $$DP[i, j] = \max egin{cases}
    DP[i-1, j-1] + 1 & 	ext{if } s_i == t_j \
    DP[i-1, j-1] - 1 & 	ext{if } s_i 
e t_j \
    DP[i-1, j] - 1 & 	ext{Gap in } T \
    DP[i, j-1] - 1 & 	ext{Gap in } S
  \end{cases}$$

### DP Complexity
- **Time Complexity:** $O(n m)$.
- **Space Complexity:** $O(n m)$.
