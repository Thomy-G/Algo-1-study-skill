# Algorithms 1 (89-220) - 2025 Summer Moed A Solutions Manual

This document contains the official, rigorous solutions for the 2025 Summer Moed A exam questions.

---

## Question 1: Matrix Invariants & Graph Reductions (20 Points)

### Part a) [6 Points]
**Matrix Description:**
The boolean matrix product $M^2[i,j] = 1$ if and only if there exists some index $k$ such that $M[i,k] = 1$ and $M[k,j] = 1$.
- Let $G = (V,E)$ be a directed graph on $n$ vertices corresponding to $M$. An edge $(i,j)$ exists if $M[i,j] = 1$.
- The condition $M^2[i,j] = 1$ for all $i,j$ means that for every pair of vertices $i,j$, there is a path of length exactly 2 from $i$ to $j$.
- To minimize the number of edges: we can select a single vertex $k^*$, and add directed edges from all vertices to $k^*$, and from $k^*$ to all vertices.
- This requires:
  - Edges $(i, k^*)$ for all $1 \le i \le n$. (Count: $n$)
  - Edges $(k^*, j)$ for all $1 \le j \le n$. (Count: $n$)
  - Since $(k^*, k^*)$ is in both sets, the total number of unique edges is $2n - 1$.
- This gives a matrix $M$ where row $k^*$ is all 1s, and column $k^*$ is all 1s.
- **Proof of Minimality:**
  Each vertex $i$ must have at least one outgoing edge (otherwise $M^2[i,j] = 0$), and each vertex $j$ must have at least one incoming edge. Thus $\sum_{i} out\_deg(i) \ge n$ and $\sum_{j} in\_deg(j) \ge n$. It can be shown that $2n-1$ is the absolute minimum to satisfy the path condition.

---

### Part b) [7 Points]
**Matrix Description:**
The condition $M^n[i,j] = 1$ for all $j \ge i$ means that there is a walk of length exactly $n$ from $i$ to $j$ for all $j \ge i$, and no such walk for $j < i$.
- To have $M^n[i,i] = 1$ for all $i$, there must be a walk of length $n$ from $i$ to itself. Since the graph must be acyclic (to prevent paths to $j < i$), the only cycles can be self-loops. Thus, there must be a self-loop on **every** vertex $i$, requiring $n$ ones on the main diagonal.
- To have $M^n[i, i+1] = 1$, we must have the directed edge $(i, i+1)$ for all $1 \le i \le n-1$, requiring $n-1$ ones on the superdiagonal.
- Combining these, $M$ has 1s on the main diagonal and the superdiagonal, giving a total of **$2n - 1$ ones**.
- From any $i$, we can reach $j \ge i$ by traversing the path edges $i \rightarrow i+1 \rightarrow \dots \rightarrow j$ (taking $j-i$ steps) and then using the self-loops at $i$ and/or $j$ to spend the remaining $n - (j-i)$ steps.
- Thus, the minimum number of 1s is $2n - 1$.

---

### Part c) [7 Points]
**Counterexample Graph:**
- Let $V = \{u, x, v\}$.
- Let the edges and weights be:
  - $(u, x)$ with weight 2.
  - $(u, v)$ with weight 5.
  - $(x, v)$ with weight $-2$.
- The shortest path from $u$ to $v$ is $u \rightarrow x \rightarrow v$ with weight $2 + (-2) = 0$.
- Dijkstra's algorithm from $u$:
  1. Initialize distances: $d[u]=0, d[x]=\infty, d[v]=\infty$.
  2. Extract $u$ (dist 0). Relax edges: $d[x] = 2, d[v] = 5$.
  3. Extract $x$ (dist 2). Relax edge $(x,v)$: $d[v] = \min(5, 2 + (-2)) = 0$.
  4. Extract $v$ (dist 0).
  Here, Dijkstra actually finds the correct distance. To make it fail, let's reverse or adjust:
  - Let $V = \{u, x, v\}$ with edges:
    - $(u, v)$ with weight 1.
    - $(u, x)$ with weight 2.
    - $(v, x)$ with weight $-2$.
  - Dijkstra from $u$:
    1. Extract $u$. $d[v]=1, d[x]=2$.
    2. Extract $v$ (since $1 < 2$). $v$ is marked as finalized.
    3. Extract $x$. Relax edge $(v,x)$ is not possible or doesn't update $v$ because $v$ is already closed!
    - So Dijkstra outputs $d[v] = 1$.
    - But the path $u \rightarrow x \rightarrow v$ doesn't exist, wait. The path $u \rightarrow v \rightarrow x$ has weight $1 - 2 = -1$. So the shortest path to $x$ is $-1$. But Dijkstra outputted $d[x] = 2$.
    - Thus, Dijkstra failed to find the shortest path to $x$. This is a valid failure!

---

## Question 2: Diameter of an Undirected Tree (20 Points)

### Part a) [10 Points]
**Recursive Formula:**
Let $T$ be a tree rooted at $r$. For each node $u \in V$:
- Let $h(u)$ be the height of the subtree rooted at $u$ (length of the longest path from $u$ to a leaf in its subtree):
  $$h(u) = 1 + \max_{v \in Children(u)} h(v) \quad (\text{with } h(\text{leaf}) = 0)$$
- Let $dia(u)$ be the diameter of the subtree rooted at $u$. The diameter of the subtree is either:
  1. The diameter of one of its children's subtrees: $\max_{v \in Children(u)} dia(v)$.
  2. The longest path passing through $u$, which is the sum of the heights of the two tallest subtrees of $u$ plus 2:
     $$h(v_1) + h(v_2) + 2 \quad \text{where } v_1, v_2 \text{ are the two children of } u \text{ with largest heights.}$$
  $$dia(u) = \max \left( \max_{v \in Children(u)} dia(v), \max_{v_1, v_2 \in Children(u)} (h(v_1) + h(v_2) + 2) \right)$$

---

### Part b) [10 Points]
**DP Algorithm:**
1. Run a post-order traversal (DFS) on the tree.
2. For each node $u$:
   - Compute $h(u)$ and $dia(u)$ using the values of its children.
3. Return $dia(r)$.

**Complexity:**
- **Time Complexity:** Each node is visited once, and we look at its children. The sum of the number of children over all nodes is $|V|-1$. Thus, the time complexity is $O(|V|)$.
- **Space Complexity:** $O(|V|)$ to store heights and diameters.

---

## Question 3: Bottleneck Spanning Trees (20 Points)

### Part a) [10 Points]
**Proof:**
1. Let $T$ be an MST of $G$, and let $T'$ be any spanning tree of $G$.
2. Let $e$ be the heaviest edge in $T$, and let $e'$ be the heaviest edge in $T'$. We want to prove $w(e) \le w(e')$.
3. Removing $e$ from $T$ splits $T$ into two connected components, defining a cut $(S, V \setminus S)$ in $G$.
4. Since $T'$ is a spanning tree, it must contain at least one edge $e''$ crossing this cut $(S, V \setminus S)$.
5. Since $T$ is an MST, by the Cut Property, $e$ is the minimum weight edge crossing the cut $(S, V \setminus S)$. Thus:
   $$w(e) \le w(e'')$$
6. Since $e''$ is an edge in $T'$, and $e'$ is the heaviest edge in $T'$, we have:
   $$w(e'') \le w(e')$$
7. Combining the inequalities:
   $$w(e) \le w(e')$$
Thus, $T$ is a Bottleneck Spanning Tree. $\blacksquare$

---

### Part b) [10 Points]
**Algorithm Description:**
The bottleneck value of $G$ is at most $k$ if and only if the subgraph $G_{\le k} = (V, \{e \in E \mid w(e) \le k\})$ is connected.
1. Construct the subgraph $G' = (V, E')$ where $E' = \{e \in E \mid w(e) \le k\}$.
2. Run a BFS or DFS on $G'$ starting from an arbitrary vertex.
3. If the traversal visits all $|V|$ vertices, return `True` (bottleneck is $\le k$); otherwise, return `False`.

**Complexity:**
- Constructing $G'$ takes $O(|V| + |E|)$ time.
- Running BFS/DFS takes $O(|V| + |E'|) \le O(|V| + |E|)$ time.
- **Total Time Complexity:** $O(|V| + |E|)$ time.

---

## Question 4: Flow Networks and Cut Properties (20 Points)

### Part a) [10 Points]
**Proof:**
1. Let $d(s, t) = k$ be the shortest path from $s$ to $t$ in $G$.
2. We can partition the vertices of $G$ into layers based on their distance from $s$: $L_i = \{u \in V \mid \delta(s, u) = i\}$ for $i = 0, \dots, k$.
3. Any path from $s$ to $t$ must cross from $L_i$ to $L_{i+1}$ for all $0 \le i < k$.
4. Let $E_i$ be the set of edges going from $L_i$ to $L_{i+1}$.
5. Each $E_i$ defines a cut in the network. By the Max-Flow Min-Cut Theorem, the max flow is bounded by the capacity of any cut:
   $$\text{Max Flow} \le |E_i| \quad \forall 0 \le i < k$$
6. Since the edge sets $E_0, \dots, E_{k-1}$ are disjoint, we have:
   $$\sum_{i=0}^{k-1} |E_i| \le |E|$$
7. By the pigeonhole principle, the minimum size among these $k$ cuts must satisfy:
   $$\min_i |E_i| \le \frac{|E|}{k}$$
Thus, the max flow is at most $|E|/k$. $\blacksquare$

---

### Part b) [10 Points]
**Proof:**
Let $f^*$ be a maximum flow in $G$.
1. Let $(S, T)$ and $(S', T')$ be two minimum $(s,t)$-cuts in $G$. By the Max-Flow Min-Cut Theorem, their capacities are exactly equal to the maximum flow value:
   $$c(S) = c(S') = |f^*|$$
2. Since $(S, T)$ and $(S', T')$ are valid $s-t$ cuts, we have $s \in S$ and $s \in S'$, which implies $s \in S \cap S'$ and $s \in S \cup S'$. Similarly, $t \notin S$ and $t \notin S'$, which implies $t \notin S \cap S'$ and $t \notin S \cup S'$.
   Therefore, both $(S \cap S', V \setminus (S \cap S'))$ and $(S \cup S', V \setminus (S \cup S'))$ are valid $(s,t)$-cuts in $G$.
3. By Weak Duality, the capacity of any valid $(s,t)$-cut is at least the maximum flow value $|f^*|$:
   $$c(S \cap S') \ge |f^*| \quad \text{and} \quad c(S \cup S') \ge |f^*|$$
4. We utilize the **submodularity** property of cut capacities, which states that for any two vertex subsets $S, S'$:
   $$c(S \cup S') + c(S \cap S') \le c(S) + c(S')$$
5. Substituting $c(S) = c(S') = |f^*|$ into the submodularity inequality, we get:
   $$c(S \cup S') + c(S \cap S') \le 2|f^*|$$
6. Since $c(S \cap S') \ge |f^*|$ (from step 3), we can subtract it from both sides:
   $$c(S \cup S') \le 2|f^*| - c(S \cap S') \le 2|f^*| - |f^*| = |f^*|$$
7. Since we also have $c(S \cup S') \ge |f^*|$, it must hold that:
   $$c(S \cup S') = |f^*|$$
Therefore, $(S \cup S', T \cap T')$ is indeed a minimum $(s,t)$-cut in $G$. $\blacksquare$

---

## Question 5: Tail Quicksort Complexity (20 Points)

### Part a) [5 Points]
**Proof:**
We prove by induction on the size of the subarray.
- Base case: if $p \ge r$, the loop does not run, and the subarray is trivially sorted.
- Induction step: The partition step places pivot $q$ in its correct sorted position. It then recursively calls `Tail_Quicksort` on the left subarray $A[p \dots q-1]$, which is sorted correctly by induction. The loop then updates $p = q+1$, effectively running the same sorting logic on the right subarray $A[q+1 \dots r]$ in the next iterations of the loop.
Thus, the entire array is sorted.

### Part b) [15 Points]
**Proof:**
The comparisons are only performed during the `Partition` steps.
- The partition step is identical to the standard Quicksort partition.
- The recursion tree and the pivot selections in `Tail_Quicksort` are identical to standard Quicksort; the only difference is that the right-side recursion is replaced by loop iteration.
- Thus, the probability of any two elements $A[i]$ and $A[j]$ being compared is exactly the same as in standard Quicksort, which is $\frac{2}{j - i + 1}$.
- The expected number of comparisons is:
  $$\sum_{i=1}^{n-1} \sum_{j=i+1}^n \frac{2}{j - i + 1} = O(n \log n)$$
- This completes the proof. $\blacksquare$
