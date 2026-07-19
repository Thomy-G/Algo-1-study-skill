# 📝 Ester Practice Exam 2 - Solutions Manual

---

## Question 1: Task Scheduling via Flow with Demands

### Part a) [6 Points]
We model this scheduling problem as a network flow problem using a directed graph $G' = (V', E')$:
1. **Vertices ($V'$):**
   - Create a source vertex $s$ and a sink vertex $t$.
   - For each task $i \in \{1,\dots,n\}$, create a task vertex $T_i$.
   - For each worker $j \in \{1,\dots,m\}$, create a worker vertex $W_j$.
2. **Directed Edges ($E'$) and Capacities ($c(e)$):**
   - For each task $i$, add a directed edge $(s, T_i)$ with capacity $d_i$.
   - For each worker $j$, add a directed edge $(W_j, t)$ with capacity $c_j$.
   - For each pair $(i, j) \in S$, add a directed edge $(T_i, W_j)$ with capacity $h_{ij}$.
   - Add a feedback edge $(t, s)$ with capacity $\infty$.

### Part b) [7 Points]
To enforce that task $i$ receives **exactly** $d_i$ hours, we set the lower bound of flow on edge $(s, T_i)$ to $d_i$ (i.e. $l(s, T_i) = d_i$). Since its capacity is also $d_i$, the flow on this edge must be exactly $d_i$.
To solve this circulation problem with demands and lower bounds, we perform the standard transformation to a standard maximum flow network:
1. Introduce a demand source $s_d$ and demand sink $t_d$.
2. For each vertex $v \in V'$, calculate its net demand $D(v) = \sum_{e \text{ into } v} l(e) - \sum_{e \text{ out of } v} l(e)$:
   - If $D(v) > 0$, add an edge $(s_d, v)$ with capacity $D(v)$.
   - If $D(v) < 0$, add an edge $(v, t_d)$ with capacity $-D(v)$.
3. For each original edge $e \in E'$, set its new capacity to $c'(e) = c(e) - l(e)$.
4. Run a standard maximum flow algorithm from $s_d$ to $t_d$.

### Part c) [7 Points]
- **Feasibility:** A feasible task assignment exists if and only if we can find a flow that satisfies all lower bounds and capacities.
- **Max Flow Equivalence:** The transformation guarantees that the lower bounds are satisfied if and only if the maximum flow in the transformed network saturates all edges leaving $s_d$ (i.e., the max flow value equals $\sum_{v: D(v)>0} D(v)$).
- **Extraction:** If a saturating flow is found, the actual flow value $f^*(T_i, W_j)$ on the edge $(T_i, W_j)$ represents the exact number of hours worker $j$ spends on task $i$.
  * Since $f(T_i, W_j) \le h_{ij}$, the individual task constraints are satisfied.
  * Since $f(W_j, t) \le c_j$, no worker exceeds their capacity.
  * Since $f(s, T_i) = d_i$, each task $i$ is fully completed.

---

## Question 2: String Matching with Wildcards via FFT

### Part a) [6 Points]
We represent characters in $\Sigma$ as positive integers, and the wildcard `*` as 0. 
A match of $P$ in $T$ starting at index $i$ occurs if and only if for all $0 \le j \le m-1$, either $P[j] = 0$ or $T[i+j] = P[j]$. This can be written as:
$$P[j] \cdot T[i+j] \cdot (T[i+j] - P[j])^2 = 0 \quad \forall 0 \le j \le m-1$$
Since each term in the sum is non-negative, a match occurs at index $i$ if and only if the sum $S(i)$ is exactly 0:
$$S(i) = \sum_{j=0}^{m-1} T[i+j] \cdot P[j] \cdot (T[i+j] - P[j])^2 = 0$$

### Part b) [8 Points]
Expanding the term inside the sum:
$$T[i+j] \cdot P[j] \cdot (T[i+j]^2 - 2T[i+j]P[j] + P[j]^2) = T[i+j]^3 P[j] - 2T[i+j]^2 P[j]^2 + T[i+j] P[j]^3$$
To compute this sum for all $i$ using polynomial multiplication, we reverse the pattern coefficients by setting $P'[j] = P[m - 1 - j]$. The sum becomes a convolution:
$$S(i) = \sum_{j=0}^{m-1} T[i+j]^3 P'[m-1-j] - 2 \sum_{j=0}^{m-1} T[i+j]^2 (P'[m-1-j])^2 + \sum_{j=0}^{m-1} T[i+j] (P'[m-1-j])^3$$
This is computed via 3 polynomial multiplications using FFT:
1. $C_1(x) = A_1(x) \cdot B_1(x)$, where $A_1(x) = \sum T[k]^3 x^k$ and $B_1(x) = \sum P'[j] x^j$.
2. $C_2(x) = A_2(x) \cdot B_2(x)$, where $A_2(x) = \sum T[k]^2 x^k$ and $B_2(x) = \sum (P'[j])^2 x^j$.
3. $C_3(x) = A_3(x) \cdot B_3(x)$, where $A_3(x) = \sum T[k] x^k$ and $B_3(x) = \sum (P'[j])^3 x^j$.
The value $S(i)$ is the coefficient of $x^{m-1+i}$ in $C_1(x) - 2C_2(x) + C_3(x)$.

### Part c) [6 Points]
If we run FFT on the entire text $T$ of size $n$, the runtime is $O(n \log n)$. 
To achieve $O(n \log m)$, we partition the text $T$ into $n/m$ overlapping blocks of size $2m$. We run the FFT multiplications on each block of size $2m$ (which takes $O(m \log m)$ time per block).
The total runtime is:
$$\frac{n}{m} \cdot O(m \log m) = O(n \log m) \text{ time.}$$

---

## Question 3: Prim's Algorithm with 1-2 Weights

### Part a) [10 Points]
In Prim's algorithm, we maintain a priority queue of vertices keyed by their minimum distance to the current tree. Since edge weights are only 1 and 2, the keys in the priority queue can only be 1, 2, or $\infty$.
We modify the priority queue using an array of three doubly-linked lists (buckets):
- `bucket[1]` contains vertices with key 1.
- `bucket[2]` contains vertices with key 2.
- `bucket[3]` contains vertices with key $\infty$.
We also maintain an array of pointers to the elements in the lists to support $O(1)$ deletion.
- **`EXTRACT-MIN`:** Remove and return the first element of `bucket[1]`. If it is empty, check `bucket[2]`. If both are empty, check `bucket[3]`. This takes $O(1)$ time.
- **`DECREASE-KEY(v, new_key)`:** Remove $v$ from its current bucket and insert it at the head of `bucket[new_key]`. This takes $O(1)$ time.

### Part b) [10 Points]
- **Complexity Analysis:**
  * Initializing the buckets takes $O(|V|)$ time.
  * We perform exactly $|V|$ `EXTRACT-MIN` operations, costing $|V| \cdot O(1) = O(|V|)$ time.
  * For each vertex extracted, we relax its outgoing edges. There are at most $|E|$ edge relaxations, each triggering a `DECREASE-KEY` costing $O(1)$ time.
  * Total time complexity: $O(|V| + |E|)$ time.
  * Space complexity: $O(|V|)$ to store the buckets.

---

## Question 4: Karger's Min-Cut Analysis

### Part a) [12 Points]
1. Let $C$ be a minimum cut of $G$ with capacity $k = |C|$.
2. The degree of any vertex in $G$ must be at least $k$, so the total number of edges satisfies $|E| \ge \frac{nk}{2}$.
3. In the first step (with $n$ vertices), the probability that we contract an edge in $C$ is at most:
   $$\frac{k}{|E|} \le \frac{k}{nk/2} = \frac{2}{n}$$
   The probability of *not* contracting a min-cut edge is at least $1 - 2/n = \frac{n-2}{n}$.
4. In step $i$ (when $n-i+1$ vertices remain), the probability of not contracting a min-cut edge is at least $\frac{n-i-1}{n-i+1}$.
5. The probability that we never contract a min-cut edge over all $n-2$ steps is:
   $$\Pr[\text{success}] \ge \prod_{i=1}^{n-2} \frac{n-i-1}{n-i+1} = \frac{n-2}{n} \cdot \frac{n-3}{n-1} \cdot \frac{n-4}{n-2} \dots \frac{2}{4} \cdot \frac{1}{3} = \frac{2}{n(n-1)}$$

### Part b) [8 Points]
Let $p \ge \frac{2}{n(n-1)} \approx \frac{2}{n^2}$ be the success probability of a single run.
- The probability that all $T$ independent runs fail is:
  $$\Pr[\text{all fail}] \le (1 - p)^T \le \left(1 - \frac{2}{n^2}\right)^T \le e^{-\frac{2T}{n^2}}$$
- We want this probability to be at most $1/e = e^{-1}$:
  $$e^{-\frac{2T}{n^2}} \le e^{-1} \iff \frac{2T}{n^2} \ge 1 \iff T \ge \frac{n^2}{2}$$
  Thus, we need at least $T = \lceil n^2 / 2 \rceil$ runs.

---

## Question 5: Seidel's Randomized LP

### Part a) [8 Points]
1. **Random Permutation:** Randomly permute the constraints: $h_1, h_2, \dots, h_n$.
2. **Incremental Step:** Let $x_{i-1}$ be the optimal solution under the first $i-1$ constraints.
3. **Evaluation:** When constraint $h_i$ ($a_i^T x \le b_i$) is added:
   - If $a_i^T x_{i-1} \le b_i$, then $x_i = x_{i-1}$.
   - If $a_i^T x_{i-1} > b_i$, the new optimal solution $x_i$ must lie on the boundary hyperplane $H_i = \{ x \mid a_i^T x = b_i \}$.
4. **Recursion:** Project the objective function and the remaining $i-1$ constraints onto the $(d-1)$-dimensional hyperplane $H_i$. Solve this new $(d-1)$-dimensional LP recursively.

### Part b) [7 Points]
- By backwards analysis, the optimal solution in $d$ dimensions is defined by at most $d$ tight constraints. Since the permutation is random, the probability that the $i$-th constraint is one of these $d$ constraints is at most $d/i$.
- The recurrence relation for the expected time $T(d, n)$ is:
  $$T(d, n) \le T(d, n-1) + O(d) + \frac{d}{n} \cdot T(d-1, n-1)$$
- Unrolling this recurrence:
  $$T(d, n) \le O(d \cdot n) + \sum_{i=1}^n \frac{d}{i} T(d-1, i-1)$$
- By induction on $d$, it can be shown that $T(d, n) \le C_d \cdot n$, where $C_d = O(d!)$.
  $$T(d, n) = O(d! \cdot n) \quad \blacksquare$$

### Part c) [5 Points]
When $d=1$, the problem is in a single variable $x$. The constraints are $x \ge l_i$ or $x \le r_i$.
- We solve it in $O(n)$ time by finding $L = \max_i l_i$ and $R = \min_i r_i$.
- If $L \le R$, the optimal solution is either $R$ or $L$ (depending on maximization/minimization). If $L > R$, the LP is infeasible.
