# Algorithms 1 (89-220) - 2026 Moed B Solutions Manual

This document contains the official, rigorous solutions for the 2026 Moed B exam questions.

---

## Question 1: DFS Edge Classifications & Properties (20 Points)

### Part a) [8 Points]
**Definitions of DFS Edge Types:**
During a DFS traversal on a directed graph $G = (V,E)$, each vertex is assigned a discovery time $d[v]$ and a finishing time $f[v]$. When exploring a directed edge $(u,v) \in E$, the edge is classified based on the state of $v$:
1. **Tree Edge:** If $v$ was white (undiscovered) when $(u,v)$ was traversed. This edge belongs to the DFS forest.
2. **Back Edge:** If $v$ was gray (currently active ancestor of $u$) when $(u,v)$ was traversed. In terms of intervals: $[d[u], f[u]] \subset [d[v], f[v]]$.
3. **Forward Edge:** If $v$ was black (fully processed) and $v$ is a descendant of $u$ in the DFS tree. In terms of intervals: $[d[v], f[v]] \subset [d[u], f[u]]$.
4. **Cross Edge:** If $v$ was black and $v$ is neither an ancestor nor a descendant of $u$. The intervals are disjoint, and specifically $f[v] < d[u]$ (since $v$ was finished before $u$ was discovered).

---

### Part b) [6 Points]
**Proposition:** If $e$ is a cross edge in DFS run $R$, then it is guaranteed *not* to be a tree edge in DFS run $R'$.
**Disproof (Counterexample):**
Let $G = (V,E)$ where $V = \{A, B\}$ and $E = \{(B,A)\}$.
- **DFS Run $R$:** The outer loop of DFS processes $A$ first, then $B$.
  - Start DFS at $A$: $A$ is colored gray, then black. $d[A]=1, f[A]=2$.
  - Start DFS at $B$: $B$ is colored gray. When exploring the edge $(B,A)$, vertex $A$ is already black. Since $A$ is not an ancestor/descendant of $B$ in $B$'s DFS tree, $(B,A)$ is classified as a **cross edge**.
- **DFS Run $R'$:** The outer loop of DFS processes $B$ first.
  - Start DFS at $B$: $B$ is colored gray. When exploring edge $(B,A)$, vertex $A$ is white. We recursively call DFS on $A$.
  - Thus, $(B,A)$ is classified as a **tree edge**.
This counterexample disproves the claim. $\blacksquare$

---

### Part c) [6 Points]
**Proposition:** If $e$ is a cross edge in DFS run $R$, then it is guaranteed *not* to be a forward edge in DFS run $R'$.
**Disproof (Counterexample):**
Let $G = (V,E)$ where $V = \{A, B, C\}$ and $E = \{(A, B), (B, C), (A, C)\}$.
- **DFS Run $R$:** The outer loop of DFS visits $C, B, A$ in that order. For $A$, neighbor $C$ is explored before neighbor $B$.
  - Start DFS at $C$: $d[C]=1, f[C]=2$.
  - Start DFS at $B$: $B$ has no outgoing edges. $d[B]=3, f[B]=4$.
  - Start DFS at $A$: $d[A]=5$.
    - Exploring edge $(A,C)$: $C$ is black, and they are in different trees. $(A,C)$ is a **cross edge**.
    - Exploring edge $(A,B)$: $B$ is black. $(A,B)$ is a **cross edge**.
- **DFS Run $R'$:** The outer loop of DFS visits $A$ first. For $A$, neighbor $B$ is explored before neighbor $C$.
  - Start DFS at $A$: $d[A]=1$. Explore neighbor $B$.
  - From $B$: explore neighbor $C$. $C$ is white. $d[C]=3, f[C]=4, f[B]=5$.
  - Back to $A$: explore neighbor $C$. $C$ is black. Since $C$ is a descendant of $A$ (as $[d[C], f[C]] \subset [d[A], f[A]]$) and $(A,C)$ is not a tree edge, $(A,C)$ is classified as a **forward edge**.
This counterexample disproves the claim. $\blacksquare$

---

## Question 2: Used Edge in Shortest Paths (20 Points)

### Algorithm Description
We determine if $(u,v)$ is a used edge by finding the shortest path distances and the number of shortest paths.
1. Run **Dijkstra's Algorithm** from the source $s$ on the original graph $G$ to compute for all vertices $x \in V$:
   - $\delta(s, x)$: the shortest path distance from $s$ to $x$.
   - $P_s[x]$: the number of shortest paths from $s$ to $x$ (capped at 2 to avoid integer overflow).
2. Run **Dijkstra's Algorithm** from the target $t$ on the reversed graph $G^{rev}$ to compute for all vertices $y \in V$:
   - $\delta(y, t)$: the shortest path distance from $y$ to $t$.
   - $P_t[y]$: the number of shortest paths from $y$ to $t$ (capped at 2).
3. Check the following conditions:
   - **Condition 1:** $\delta(s, u) + w(u,v) + \delta(v, t) == \delta(s, t)$ (Checks if $(u,v)$ lies on at least one shortest path).
   - **Condition 2:** $P_s[u] \times P_t[v] \ge 2$ (Checks if the combination of prefix paths from $s$ to $u$ and suffix paths from $v$ to $t$ yields at least 2 distinct shortest paths).
4. If both conditions hold, return `True`; otherwise, return `False`.

### Path Counting Relaxation Rule in Dijkstra
When relaxing an edge $(x, y)$ in SSSP from $s$:
*   If $d[x] + w(x,y) < d[y]$:
    - $d[y] = d[x] + w(x,y)$
    - $P_s[y] = P_s[x]$
*   If $d[x] + w(x,y) == d[y]$:
    - $P_s[y] = \min(2, P_s[y] + P_s[x])$

### Proof of Correctness
- Any path from $s$ to $t$ containing $(u,v)$ can be decomposed into a path $s \rightsquigarrow u$, the edge $(u,v)$, and a path $v \rightsquigarrow t$.
- The length of such a path is the sum of the lengths of the parts. It is a shortest path if and only if its length is exactly $\delta(s,t)$, which is guaranteed by Condition 1.
- Since the choice of the prefix path $s \rightsquigarrow u$ and suffix path $v \rightsquigarrow t$ are independent, the total number of shortest paths from $s$ to $t$ containing $(u,v)$ is exactly $P_s[u] \times P_t[v]$.
- Thus, at least 2 such paths exist if and only if $P_s[u] \times P_t[v] \ge 2$.

### Complexity Analysis
- Running Dijkstra's algorithm on $G$ and $G^{rev}$ takes $O(|E| + |V| \log |V|)$ time using a Fibonacci heap.
- Evaluating the final condition takes $O(1)$ time.
- **Total Time Complexity:** $O(|E| + |V| \log |V|)$ time.
- **Total Space Complexity:** $O(|V| + |E|)$ to store the graph, its reverse, and SSSP arrays.

---

## Question 3: Binary Array Skip Score Optimization (20 Points)

### Algorithm Description
We can compute the scores for all $k$ simultaneously by reducing the problem to polynomial multiplication.
1. Represent the binary array $A$ as a polynomial $A(x)$ of degree $n-1$:
   $$A(x) = \sum_{i=0}^{n-1} a_i x^i$$
2. Let $B(x)$ be the polynomial corresponding to the reversed array $A$:
   $$B(x) = \sum_{j=0}^{n-1} a_{n-1-j} x^j$$
3. Multiply the two polynomials using the **Fast Fourier Transform (FFT)**:
   $$C(x) = A(x) \cdot B(x)$$
4. The coefficient $c_d$ of $C(x)$ at index $d = n - 1 - k$ is exactly $Score(A, k)$ for $1 \le k < n$.
5. Scan the coefficients $c_{n-1-k}$ for all $1 \le k < n$ and find the index $k^*$ that maximizes this value.
6. Return $k^*$ and the maximum score.

### Proof of Correctness
By definition of polynomial multiplication, the coefficient of $x^d$ in $C(x) = A(x) \cdot B(x)$ is:
$$c_d = \sum_{i=0}^d a_i b_{d-i}$$
For $d = n - 1 - k$, and substituting $b_j = a_{n-1-j}$:
$$c_{n-1-k} = \sum_{i=0}^{n-1-k} a_i a_{n-1-(n-1-k-i)} = \sum_{i=0}^{n-1-k} a_i a_{i+k}$$
This is exactly the definition of $Score(A,k)$. Thus, the FFT multiplication computes all skip scores correctly.

### Complexity Analysis
- Constructing the polynomials of size $2n$ takes $O(n)$ time.
- Multiplying the polynomials using forward and inverse FFT takes $O(n \log n)$ time.
- Scanning the results to find the maximum takes $O(n)$ time.
- **Total Time Complexity:** $O(n \log n)$ time.
- **Total Space Complexity:** $O(n)$ space to store the FFT arrays.

---

## Question 4: Bucket Sort with $m$ Buckets (20 Points)

### Algorithm Description
1. Divide the interval $[0, 1)$ into $m$ equal-sized buckets: $B_0, B_1, \dots, B_{m-1}$, where bucket $B_i$ covers the range $[\frac{i}{m}, \frac{i+1}{m})$.
2. Distribute the $n$ input elements into the buckets: element $A[j]$ goes to bucket $B_{\lfloor m \cdot A[j] \rfloor}$.
3. Sort each bucket individually using **Insertion Sort**.
4. Concatenate the sorted buckets to obtain the final sorted array.

### Expected Runtime Analysis
Let $n_i$ be the random variable representing the number of elements that fall into bucket $B_i$.
Since the input elements are drawn independently and uniformly from $[0, 1)$, each element has a probability $p = 1/m$ of falling into bucket $B_i$. Thus, $n_i$ follows a Binomial distribution: $n_i \sim \text{Binomial}(n, 1/m)$.

The total runtime $T(n)$ of the algorithm is composed of three parts:
1. **Distribution Phase:** Placing each of the $n$ elements into its corresponding bucket takes $O(1)$ time per element, yielding $O(n)$ time in total.
2. **Bucket Overhead (Initialization & Concatenation):** Initializing the $m$ empty buckets and performing the final concatenation of the $m$ sorted buckets takes $O(m)$ time.
3. **Sorting Phase:** Sorting each bucket $i$ containing $n_i$ elements using Insertion Sort takes $O(n_i^2)$ time.

Thus, the total runtime is:
$$T(n) = O(n + m) + \sum_{i=0}^{m-1} O(n_i^2)$$

By applying **Linearity of Expectation**, the expected total runtime is:
$$E[T(n)] = E\left[ O(n + m) + \sum_{i=0}^{m-1} O(n_i^2) \right] = O(n + m) + \sum_{i=0}^{m-1} E[O(n_i^2)]$$

To calculate the expected value of $n_i^2$, we use the variance and expectation formulas for a Binomial random variable $n_i$:
*   $E[n_i] = n \cdot p = \frac{n}{m}$
*   $\text{Var}(n_i) = n \cdot p \cdot (1 - p) = \frac{n}{m} \left(1 - \frac{1}{m}\right)$

Since $\text{Var}(n_i) = E[n_i^2] - (E[n_i])^2$, we can solve for $E[n_i^2]$:
$$E[n_i^2] = \text{Var}(n_i) + (E[n_i])^2 = \frac{n}{m} \left(1 - \frac{1}{m}\right) + \left(\frac{n}{m}\right)^2 = \frac{n}{m} - \frac{n}{m^2} + \frac{n^2}{m^2}$$

Now, we sum this expected cost over all $m$ buckets:
$$\sum_{i=0}^{m-1} E[O(n_i^2)] = m \cdot O\left(\frac{n}{m} - \frac{n}{m^2} + \frac{n^2}{m^2}\right) = O\left(n - \frac{n}{m} + \frac{n^2}{m}\right) = O\left(n + \frac{n^2}{m}\right)$$

Combining this back into our total expectation:
$$E[T(n)] = O(n + m) + O\left(n + \frac{n^2}{m}\right) = O\left(n + m + \frac{n^2}{m}\right)$$

Since $m \in o(n)$, it holds that $m < n$ and the term $\frac{n^2}{m}$ strictly dominates $n$ (because $\frac{n}{m} \to \infty$). Therefore, the asymptotic expected runtime simplifies to:
$$E[T(n)] \in \Theta\left(\frac{n^2}{m}\right)$$

### Comparison with Standard Bucket Sort
- In standard Bucket Sort, $m = n$ buckets are used, which yields an expected runtime of $O(n + \frac{n^2}{n}) = O(n)$ time.
- When $m \in o(n)$, the expected runtime becomes $\Theta(\frac{n^2}{m})$. Since $m$ is asymptotically smaller than $n$, the ratio $n/m \rightarrow \infty$, meaning the expected runtime is strictly superlinear and significantly slower than standard Bucket Sort.

---

## Question 5: Modified Coupon Collector (20 Points)

### Part a) [8 Points]
**Proof:**
At any step, let $U$ be the number of uncollected coupons. Since we are in the phase where $U = 2^k$ for some $k$, the number of collected coupons is $n - 2^k$.
We sum the probabilities of all coupons in the sample space:
1. For each of the $2^k$ uncollected coupons, the probability is $\frac{1}{2^{k+1}}$. The sum is:
   $$\Sigma_{uncollected} = 2^k \cdot \frac{1}{2^{k+1}} = \frac{1}{2}$$
2. For each of the $n - 2^k$ collected coupons, the probability is $\frac{1}{2n - 2^{k+1}}$. The sum is:
   $$\Sigma_{collected} = (n - 2^k) \cdot \frac{1}{2n - 2^{k+1}} = \frac{n - 2^k}{2(n - 2^k)} = \frac{1}{2}$$
Adding the two parts together:
$$\Sigma_{total} = \Sigma_{uncollected} + \Sigma_{collected} = \frac{1}{2} + \frac{1}{2} = 1$$
Thus, the sum of probabilities is always 1, and the distribution is valid. $\blacksquare$

---

### Part b) [12 Points]
**Proof:**
1. Let $i-1$ be the number of collected coupons. The number of uncollected coupons is $U = n - i + 1$.
2. Let $k$ be the index of the phase. This means $2^{k-1} < U \le 2^k$.
3. The probability of drawing a new coupon $p_i$ is the sum of the probabilities of all uncollected coupons:
   $$p_i = U \cdot \frac{1}{2^{k+1}}$$
4. Since $U > 2^{k-1}$, we have:
   $$p_i > 2^{k-1} \cdot \frac{1}{2^{k+1}} = \frac{1}{4}$$
   Thus, $p_i \ge 1/4$ for all $1 \le i < n$.

**Upper Bound on Expected Number of Draws:**
Let $X_i$ be the number of draws needed to collect the $i$-th coupon after $i-1$ coupons have been collected.
- $X_i$ is a geometric random variable with parameter $p_i$.
- The expected number of draws for this step is $E[X_i] = \frac{1}{p_i}$.
- Since $p_i \ge 1/4$, we have $E[X_i] \le 4$.
The total expected number of draws to collect all $n$ coupons is:
$$E[X] = \sum_{i=1}^n E[X_i] \le \sum_{i=1}^n 4 = 4n$$
Thus, the expected number of draws is bounded by $4n \in O(n)$ (linear time). $\blacksquare$
