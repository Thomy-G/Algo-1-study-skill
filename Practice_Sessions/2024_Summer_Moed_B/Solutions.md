# Algorithms 1 (89-220) - 2024 Summer Moed B Solutions Manual

This document contains the official, rigorous solutions for the 2024 Summer Moed B exam questions.

---

## Question 1: Longest Contiguous Decreasing Subsequence (20 Points)

### Part a) [10 Points]
**Recursive Formula:**
Let $DP[i]$ be the length of the longest contiguous strictly decreasing subsequence ending at index $i$.
- **Base Case:**
  - $DP[1] = 1$.
- **Recursive Step:**
  For $i > 1$:
  $$DP[i] = egin{cases}
    DP[i-1] + 1 & 	ext{if } a_{i} < a_{i-1} \
    1 & 	ext{otherwise}
  \end{cases}$$
The length of the longest such subsequence in the entire array is $\max_{1 \le i \le n} DP[i]$.

---

### Part b) [10 Points]
**DP Algorithm:**
1. Initialize an array $DP$ of size $n$ with 1.
2. Iterate $i = 2$ to $n$:
   - If $a_i < a_{i-1}$, set $DP[i] = DP[i-1] + 1$.
3. Return the maximum value in $DP$.

**Complexity:**
- **Time Complexity:** $O(n)$ time.
- **Space Complexity:** $O(n)$ space (or $O(1)$ if we only track the current and max lengths).

---

## Question 2: Minimum Graded Weight Spanning Tree (20 Points)

### Algorithm Description
The graded weight of $T$ is $W_d(T) = \sum_{v \in V} w(v) \cdot deg_T(v)$.
Recall that in any tree, the sum of degrees is $2|V| - 2$.
We can rewrite the sum:
$$W_d(T) = \sum_{v \in V} w(v) \cdot deg_T(v) = \sum_{(u,v) \in E_T} (w(u) + w(v))$$
This means that if we define a new edge weight function $w'(u,v) = w(u) + w(v)$ for each edge $(u,v) \in E$, the problem reduces exactly to finding a standard Minimum Spanning Tree of $G$ with edge weights $w'$.
1. Define $w'(u,v) = w(u) + w(v)$ for each edge $(u,v) \in E$.
2. Run Kruskal's or Prim's algorithm on $G$ with weights $w'$.
3. Return the resulting spanning tree.

### Proof of Correctness
The algebraic rewriting proves that the graded weight is exactly equal to the sum of the modified edge weights in any spanning tree. Thus, the tree that minimizes the sum of modified edge weights is exactly the tree that minimizes the graded weight.

### Complexity Analysis
- Constructing modified weights takes $O(|E|)$ time.
- Running Kruskal's algorithm takes $O(|E| \log |V|)$ time.
- **Total Time Complexity:** $O(|E| \log |V|)$ time.

---

## Question 3: SSSP passing through $v_0$ (20 Points)

### Algorithm Description
Any path from $u$ to $v$ passing through $v_0$ has weight $d(u, v_0) + d(v_0, v)$.
1. Run the Bellman-Ford algorithm on the original graph $G$ from source $v_0$ to find $\delta(v_0, x)$ for all $x \in V$.
2. Run the Bellman-Ford algorithm on the reversed graph $G^{rev}$ from source $v_0$ to find $\delta(y, v_0)$ for all $y \in V$.
3. For each pair $u, v \in V$, the shortest path weight passing through $v_0$ is:
   $$D[u, v] = \delta(u, v_0) + \delta(v_0, v)$$
4. Return the matrix $D$.

**Correctness:**
The shortest path from $u$ to $v$ through $v_0$ is the concatenation of the shortest path from $u$ to $v_0$ and the shortest path from $v_0$ to $v$. Since the graph is DAG or has no negative cycles, these subpaths are independent and their sum is the minimum possible.

**Complexity:**
- Running Bellman-Ford twice takes $2 	imes O(|V||E|) = O(|V||E|)$ time.
- Constructing the output matrix takes $O(|V|^2)$ time.
- **Total Time Complexity:** $O(|V||E|)$ time (since $|E| \ge |V|-1$, $|V|^2 \le |V||E|$).

---

## Question 4: Edges crossing Every Min Cut (20 Points)

### Algorithm Description
Let $F = \{e_1, e_2, \dots, e_p\}$.
All edges in $F$ cross every min cut if and only if increasing the capacity of all edges in $F$ by 1 increases the max flow by exactly $|F|$.
1. Run a max-flow algorithm to get the max flow value $F_{max}$.
2. Increase the capacity of each edge $e \in F$ by 1.
3. Run the max-flow algorithm on this modified network to get $F'_{max}$.
4. If $F'_{max} == F_{max} + |F|$, return `True`; otherwise, return `False`.

**Complexity:**
- Running max flow twice takes $O(|V|^2 |E|)$ time.

---

## Question 5: Reservoir Sampling Proof (20 Points)

### Part a) [8 Points]
**Proof for $x_n$:**
- The element $x_n$ is selected to enter the reservoir in the final step with probability $k/n$.
- Once it enters, it is placed in $R$ and is never replaced since it is the last step.
- Thus, the probability that $x_n$ is in $R$ is exactly $k/n$. $lacksquare$

---

### Part b) [12 Points]
**Proof for $x_i$:**
- In step $i$, $x_i$ is placed in the reservoir with probability $k/i$.
- For each subsequent step $j$ (from $i+1$ to $n$):
  - The probability that the element at $x_i$'s slot in the reservoir is replaced is the probability that step $j$ chooses to replace a slot, and chooses $x_i$'s slot specifically:
    $$P(	ext{replaced at step } j) = rac{k}{j} \cdot rac{1}{k} = rac{1}{j}$$
  - The probability that it is **not** replaced at step $j$ is $1 - 1/j = rac{j-1}{j}$.
- The probability that $x_i$ survives until the end of the stream is the product of its initial entry probability and all subsequent survival probabilities:
  $$P(x_i \in R) = rac{k}{i} 	imes \prod_{j=i+1}^n rac{j-1}{j} = rac{k}{i} 	imes \left( rac{i}{i+1} 	imes rac{i+1}{i+2} \dots rac{n-1}{n} ight)$$
- The terms telescope:
  $$P(x_i \in R) = rac{k}{i} 	imes rac{i}{n} = rac{k}{n}$$
- This completes the proof. $lacksquare$
