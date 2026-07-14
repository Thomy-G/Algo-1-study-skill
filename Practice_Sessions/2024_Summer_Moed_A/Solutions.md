# Algorithms 1 (89-220) - 2024 Summer Moed A Solutions Manual

This document contains the official, rigorous solutions for the 2024 Summer Moed A exam questions.

---

## Question 1: Minimum Vertex Cover on a Tree (20 Points)

### Part a) [10 Points]
**Recursive Formula:**
Let $T$ be a tree rooted at $r$. For each node $u$:
- Let $VC_{in}(u)$ be the size of the minimum vertex cover of the subtree rooted at $u$, given that $u$ **is included** in the cover.
- Let $VC_{out}(u)$ be the size of the minimum vertex cover of the subtree rooted at $u$, given that $u$ **is not included** in the cover.
- If $u$ is a leaf:
  - $VC_{in}(u) = 1$
  - $VC_{out}(u) = 0$
- If $u$ is an internal node:
  - If $u$ is included, its children $v$ can either be included or not:
    $$VC_{in}(u) = 1 + \sum_{v \in Children(u)} \min(VC_{in}(v), VC_{out}(v))$$
  - If $u$ is not included, all its children $v$ **must** be included in the cover to cover the edges $(u,v)$:
    $$VC_{out}(u) = \sum_{v \in Children(u)} VC_{in}(v)$$
The size of the minimum vertex cover of $T$ is $\min(VC_{in}(r), VC_{out}(r))$.

---

### Part b) [10 Points]
**DP Algorithm:**
1. Perform a post-order traversal (DFS) of $T$.
2. For each node $u$ visited:
   - Compute $VC_{in}(u)$ and $VC_{out}(u)$ using the DP values of its children.
3. Return $\min(VC_{in}(r), VC_{out}(r))$.

**Complexity:**
- **Time Complexity:** $O(n)$ as we visit each vertex and edge once.
- **Space Complexity:** $O(n)$ to store the values.

---

## Question 2: Reverse-Delete MST Algorithm (20 Points)

### Part a) [5 Points]
**Proof:**
1. The algorithm starts with a connected graph $G$ and only removes an edge if the graph remains connected. Thus, the final graph is connected.
2. Suppose the final graph contains a cycle $C$. The algorithm iterates over all edges. The last edge of $C$ processed would have been removed since its removal would not disconnect the graph (as the rest of the cycle provides an alternative path). This is a contradiction.
3. Thus, the final graph is connected and acyclic, which is a spanning tree. $lacksquare$

---

### Part b) [15 Points]
**Proof:**
1. Let $e$ be an edge removed by the algorithm.
2. Since $e$ was removed, it must have been part of some cycle $C$ in the graph when it was processed.
3. Since we process edges in non-increasing order of their weights, and $e$ is the edge currently being processed, all other edges in $C$ that have not been processed yet must have weight at most $w(e)$.
4. Edges of $C$ that were already processed and kept must also have weight at least $w(e)$, but since they are in the cycle, $e$ is the heaviest edge on $C$.
5. By the Cycle Property of MSTs, the heaviest edge on any cycle cannot belong to any MST.
Thus, the algorithm only removes edges that do not belong to any MST. The remaining spanning tree is the MST. $lacksquare$

---

## Question 3: Dynamic SSSP Updates (20 Points)

### Part a) [10 Points]
**Algorithm Description:**
For any $u \in V \setminus \{v\}$, the shortest path from $u$ to $v$ in $G$ must end with a direct edge from some vertex $x$ to $v$:
$$\delta_G(u, v) = \min \left( w(u,v), \min_{x \in V \setminus \{v\}} \{ \delta_{G'}(u, x) + w(x, v) \} ight)$$
1. For each $u \in V \setminus \{v\}$, compute the minimum over all $x$ of $\delta_{G'}(u, x) + w(x, v)$.
2. Set $\delta_G(v, v) = 0$.

**Complexity:**
- For each of the $n-1$ vertices $u$, we search over $n-1$ vertices $x$.
- **Time Complexity:** $O(n^2)$ time.

---

### Part b) [10 Points]
**Algorithm Description:**
For any pair $s, t \in V \setminus \{v\}$, the shortest path in $G$ either:
1. Does not pass through $v$: its weight is $\delta_{G'}(s, t)$.
2. Passes through $v$: its weight is $\delta_G(s, v) + \delta_G(v, t)$.
We update:
$$\delta_G(s, t) = \min( \delta_{G'}(s, t), \delta_G(s, v) + \delta_G(v, t) )$$
For pairs involving $v$, we use the precomputed values directly.

**Complexity:**
- We update $n^2$ entries in $O(1)$ time each.
- **Time Complexity:** $O(n^2)$ time.

---

## Question 4: Two Edges crossing Every Min Cut (20 Points)

### Algorithm Description
Both $e_1 = (u_1, v_1)$ and $e_2 = (u_2, v_2)$ cross every min cut if and only if:
1. $e_1$ crosses every min cut.
2. $e_2$ crosses every min cut in the modified network where $e_1$ is not present or has altered capacity.
Actually, a more direct way:
1. Run max flow on $G$ to get value $F$. Both $e_1$ and $e_2$ must be saturated in this flow.
2. Increase the capacity of both $e_1$ and $e_2$ by 2: $c'(e_1) = c(e_1) + 2$ and $c'(e_2) = c(e_2) + 2$.
3. Run max flow on the modified network $G'$ to get value $F'$.
4. If $F' == F + 4$, return `True`; otherwise, return `False`.

**Proof of Correctness:**
If $e_1$ and $e_2$ cross every min cut, then increasing their capacity by 2 increases the capacity of every min cut by $2 + 2 = 4$. Thus, the max flow value must increase by exactly 4. If there is even one min cut not containing both, the capacity increase will be strictly less than 4.

**Complexity:**
- Running max flow twice takes $O(|V|^2 |E|)$ time.

---

## Question 5: Randomized Matrix Equality Verification (20 Points)

### Part a) [15 Points]
**Freivalds' Algorithm Variant:**
1. Generate a random binary vector $r$ of size $n 	imes 1$ where each entry is chosen uniformly at random from $\{0, 1\}$.
2. Compute:
   - $x = D \cdot r \pmod 2$
   - $y = C \cdot x \pmod 2 = (C \cdot D) \cdot r \pmod 2$
   - $z = B \cdot r \pmod 2$
   - $w = A \cdot z \pmod 2 = (A \cdot B) \cdot r \pmod 2$
3. If $y == w$, return `True`; otherwise, return `False`.

**Proof of Correctness:**
If $A \cdot B = C \cdot D$, then $(A \cdot B) r = (C \cdot D) r$ always, so it returns `True` (100% correct).
If $A \cdot B 
e C \cdot D$, by Freivalds' theorem, the probability that $(A \cdot B) r = (C \cdot D) r \pmod 2$ is at most 0.5. Thus, the error probability is at most 0.5.

**Complexity:**
- Matrix-vector multiplication takes $O(n^2)$ time.
- **Total Time Complexity:** $O(n^2)$ time.

---

### Part b) [5 Points]
To succeed with high probability, we repeat the check $k = C \log n$ times with independent random vectors.
- **Total Time Complexity:** $O(n^2 \log n)$ time.
