# 📝 Ester Practice Exam 2 - Solutions Manual

---

## Question 1: Task Scheduling via Flow with Demands
### Reduction to Circulation with Demands and Lower Bounds
We can model this problem as a circulation problem with demands and lower bounds.
1. **Network Construction:**
   - Create a source vertex $s$ and a sink vertex $t$.
   - For each task $i \in \{1,\dots,n\}$, create a vertex $T_i$.
   - For each worker $j \in \{1,\dots,m\}$, create a vertex $W_j$.
2. **Edges and Constraints (Lower bounds $l(e)$ and capacities $c(e)$):**
   - For each task $i$, add a directed edge $(s, T_i)$ with:
     * Lower bound: $l(s, T_i) = d_i$
     * Capacity: $c(s, T_i) = d_i$
     This forces exactly $d_i$ hours of flow to be sent to task $i$.
   - For each worker $j$, add a directed edge $(W_j, t)$ with:
     * Lower bound: $l(W_j, t) = 0$
     * Capacity: $c(W_j, t) = c_j$
     This limits the total hours worked by worker $j$ to at most $c_j$.
   - For each pair $(i, j) \in S$, add a directed edge $(T_i, W_j)$ with:
     * Lower bound: $l(T_i, W_j) = 0$
     * Capacity: $c(T_i, W_j) = h_{ij}$
     This limits worker $j$ to at most $h_{ij}$ hours on task $i$.
   - Add a feedback edge $(t, s)$ with $l(t, s) = 0$ and $c(t, s) = \infty$.
3. **Solving:**
   - This circulation problem with demands and lower bounds can be solved by converting it to a standard maximum flow problem using the standard transformation:
     * Introduce a demand source $s_d$ and demand sink $t_d$.
     * For each vertex $v$, let $D(v)$ be the sum of lower bounds on incoming edges minus the sum of lower bounds on outgoing edges.
     * If $D(v) > 0$, add an edge $(s_d, v)$ with capacity $D(v)$. If $D(v) < 0$, add an edge $(v, t_d)$ with capacity $-D(v)$.
     * For each original edge $e = (u,v)$, set its capacity to $c(e) - l(e)$.
     * Find the maximum flow from $s_d$ to $t_d$. If the max flow value equals the sum of positive $D(v)$, then a feasible assignment exists and can be extracted directly from the edge flows.

### Proof of Correctness
- **Capacity Constraints:** The capacity $c(W_j, t) = c_j$ ensures no worker exceeds their maximum capacity. The capacity $c(T_i, W_j) = h_{ij}$ ensures worker $j$ spends at most $h_{ij}$ hours on task $i$.
- **Task Demands:** The lower bound and capacity on $(s, T_i)$ are both $d_i$, forcing exactly $d_i$ flow to pass through $T_i$.
- **Flow Conservation:** The flow leaving $T_i$ must equal the sum of flows sent to workers $W_j$. Thus, the total hours assigned to task $i$ is exactly $d_i$.

---

## Question 2: String Matching with Wildcards via FFT
### Algorithm Description
We represent the alphabet characters as integers, e.g., $a \rightarrow 1, b \rightarrow 2, \dots$ and the wildcard `*` as 0.
A match of pattern $P[0 \dots m-1]$ in text $T$ starting at index $i$ occurs if and only if for all $0 \le j \le m-1$:
$$P[j] = 0 \quad \text{or} \quad T[i+j] = P[j]$$
This condition is equivalent to the sum being 0:
$$S(i) = \sum_{j=0}^{m-1} T[i+j] \cdot P[j] \cdot (T[i+j] - P[j])^2 = 0$$
Since all terms in the sum are non-negative, the sum is 0 if and only if every term is 0.
Expanding the term inside the sum:
$$T[i+j] \cdot P[j] \cdot (T[i+j]^2 - 2T[i+j]P[j] + P[j]^2) = T[i+j]^3 P[j] - 2T[i+j]^2 P[j]^2 + T[i+j] P[j]^3$$
We can compute this sum for all $i$ by reversing the pattern coefficients (setting $P'[j] = P[m - 1 - j]$) and performing three polynomial multiplications:
1. $A_1(x) = \sum_{k=0}^{n-1} T[k]^3 x^k$ and $B_1(x) = \sum_{j=0}^{m-1} P'[j] x^j$. Product is $C_1(x)$.
2. $A_2(x) = \sum_{k=0}^{n-1} T[k]^2 x^k$ and $B_2(x) = \sum_{j=0}^{m-1} (P'[j])^2 x^j$. Product is $C_2(x)$.
3. $A_3(x) = \sum_{k=0}^{n-1} T[k] x^k$ and $B_3(x) = \sum_{j=0}^{m-1} (P'[j])^3 x^j$. Product is $C_3(x)$.

For each index $i \in [0, n - m]$, the value $S(i)$ is the coefficient of $x^{m-1+i}$ in the combined polynomial:
$$S(i) = C_1[m-1+i] - 2C_2[m-1+i] + C_3[m-1+i]$$
We compute the products using FFT. To achieve $O(n \log m)$ time, we divide the text $T$ into overlapping blocks of size $2m$ and run the FFT-based multiplication on each block.

### Complexity Analysis
- Dividing $T$ into $n/m$ blocks of size $2m$.
- For each block, running 3 FFT multiplications of size $2m$ takes $O(m \log m)$ time.
- Total time: $\frac{n}{m} \cdot O(m \log m) = O(n \log m)$ time.

---

## Question 3: Prim's Algorithm with 1-2 Weights
### Algorithm Description & Priority Queue Modification
In Prim's algorithm, we maintain a priority queue of vertices not yet in the MST, keyed by their minimum distance to the current tree.
Since edge weights are only 1 and 2, the key of any vertex in the priority queue can only take values in $\{1, 2, \infty\}$.
1. **Priority Queue Structure:**
   - We replace the standard binary or Fibonacci heap with an array of three doubly-linked lists (buckets):
     * `bucket[1]`: contains all vertices with key 1.
     * `bucket[2]`: contains all vertices with key 2.
     * `bucket[3]`: contains all vertices with key $\infty$.
   - Maintain an array of pointers to the vertices in the lists to support $O(1)$ deletion.
2. **Operations:**
   - **`EXTRACT-MIN`:** 
     * If `bucket[1]` is not empty, remove and return its first element.
     * Else, if `bucket[2]` is not empty, remove and return its first element.
     * Else, remove and return the first element of `bucket[3]`.
     This operation takes $O(1)$ time since we only check the heads of the lists.
   - **`DECREASE-KEY(v, new_key)`:**
     * Remove $v$ from its current list.
     * Insert $v$ at the head of `bucket[new_key]`.
     This operation takes $O(1)$ time because we have direct pointers to $v$ in the doubly-linked lists.

### Complexity Analysis
- Initializing the buckets takes $O(|V|)$ time.
- We perform exactly $|V|$ `EXTRACT-MIN` operations, costing $O(|V|)$ time in total.
- We perform at most $|E|$ `DECREASE-KEY` operations, costing $O(|E|)$ time in total.
- **Total Time Complexity:** $O(|V| + |E|)$ time.

---

## Question 4: Karger's Min-Cut Analysis
### 1. Single-Run Success Probability Proof
1. Let $C$ be a minimum cut of $G$, and let $k = |C|$ be its capacity.
2. The degree of any vertex in $G$ must be at least $k$ (otherwise the cut separating that single vertex would be smaller than $k$). Thus, the total number of edges is:
   $$|E| = \frac{1}{2} \sum_{v \in V} \text{deg}(v) \ge \frac{nk}{2}$$
3. In the first contraction step, we choose a random edge to contract. The probability that we contract an edge belonging to the min-cut $C$ is:
   $$\Pr[\text{contract edge in } C] = \frac{k}{|E|} \le \frac{k}{nk/2} = \frac{2}{n}$$
   Thus, the probability of *not* contracting a min-cut edge in the first step is at least $1 - \frac{2}{n} = \frac{n-2}{n}$.
4. In step $i$ (when there are $n-i+1$ vertices remaining), the probability of not contracting a min-cut edge is at least:
   $$1 - \frac{2}{n-i+1} = \frac{n-i-1}{n-i+1}$$
5. The algorithm successfully finds $C$ if it never contracts a min-cut edge. The probability of this is:
   $$\Pr[\text{success}] \ge \prod_{i=1}^{n-2} \frac{n-i-1}{n-i+1} = \frac{n-2}{n} \cdot \frac{n-3}{n-1} \cdot \frac{n-4}{n-2} \dots \frac{2}{4} \cdot \frac{1}{3} = \frac{2}{n(n-1)} \quad \blacksquare$$

### 2. Number of Runs for High Probability
Let $p \ge \frac{2}{n(n-1)} \approx \frac{2}{n^2}$ be the success probability of a single run.
- The probability that a single run fails is $1 - p \le 1 - \frac{2}{n^2}$.
- The probability that all $T$ independent runs fail is:
  $$\Pr[\text{all fail}] \le \left(1 - \frac{2}{n^2}\right)^T$$
- Using the inequality $1 - x \le e^{-x}$:
  $$\Pr[\text{all fail}] \le e^{-\frac{2T}{n^2}}$$
- We want the probability of at least one success to be at least $1 - 1/e$, which means the probability of all failing is at most $1/e = e^{-1}$:
  $$e^{-\frac{2T}{n^2}} \le e^{-1} \iff \frac{2T}{n^2} \ge 1 \iff T \ge \frac{n^2}{2}$$
  Thus, we need at least $T = \lceil n^2 / 2 \rceil$ runs.

---

## Question 5: Seidel's Randomized LP
### 1. Algorithm Description
1. **Random Permutation:** Randomly permute the constraints: $h_1, h_2, \dots, h_n$.
2. **Incremental Step:** Solve the LP incrementally. Let $x_{i-1}$ be the optimal solution under the first $i-1$ constraints.
3. **Constraint Evaluation:** When constraint $h_i$ ($a_i^T x \le b_i$) is added:
   - If $a_i^T x_{i-1} \le b_i$ ($x_{i-1}$ satisfies $h_i$), then the optimal solution remains unchanged: $x_i = x_{i-1}$.
   - If $a_i^T x_{i-1} > b_i$, the new optimal solution $x_i$ must lie on the boundary hyperplane $H_i = \{ x \mid a_i^T x = b_i \}$.
4. **Recursive Reduction:** Project the objective function and the remaining $i-1$ constraints onto the $(d-1)$-dimensional hyperplane $H_i$. Solve this new $(d-1)$-dimensional LP recursively.

### 2. Proof of Expected $O(d! \cdot n)$ Runtime
- Let $T(d, n)$ be the expected time to solve a $d$-dimensional LP with $n$ constraints.
- When constraint $h_i$ is added, the probability that the optimal solution changes (requiring a recursive call in $d-1$ dimensions) is at most $d/i$ by **backwards analysis** (since the optimal solution in $d$ dimensions is defined by at most $d$ tight constraints, and since the permutation is random, the probability that the $i$-th constraint is one of these $d$ constraints is at most $d/i$).
- The recurrence relation for the expected time is:
  $$T(d, n) \le T(d, n-1) + O(d) + \frac{d}{n} \cdot T(d-1, n-1)$$
- Unrolling this recurrence yields:
  $$T(d, n) \le O(d \cdot n) + \sum_{i=1}^n \frac{d}{i} T(d-1, i-1)$$
- By induction on $d$, it can be proven that $T(d, n) \le C_d \cdot n$, where $C_d = O(d!)$. Thus:
  $$T(d, n) = O(d! \cdot n) \quad \blacksquare$$

### 3. Base Case ($d=1$)
When $d=1$, the problem is in a single variable $x$. The constraints are simply of the form $x \ge l_i$ or $x \le r_i$.
- We can solve this in $O(n)$ time by finding:
  $$L = \max_i l_i \quad \text{and} \quad R = \min_i r_i$$
- If $L \le R$, the optimal solution is either $R$ or $L$ (depending on whether we maximize or minimize the objective function). If $L > R$, the LP is infeasible.
