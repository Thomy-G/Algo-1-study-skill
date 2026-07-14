# Algorithms 1 (89-220) - Theme: Union-Find - Solutions Manual

This document contains the official, rigorous solutions for the Union-Find & Amortized Complexity practice session.

---

## Question 1: Union-Find Heuristics (20 Points)

### Part a) [10 Points]
**Union-by-Rank:**
- During `Union(x, y)`, we find the roots $r_x$ and $r_y$ of the respective sets.
- If $r_x \ne r_y$, we compare their ranks:
  - The root with the smaller rank is made to point to the root with the larger rank.
  - If their ranks are equal, we arbitrarily make one the parent of the other (e.g. $r_x \rightarrow r_y$), and we increment the rank of the new parent by 1 (e.g. $\text{rank}(r_y) = \text{rank}(r_y) + 1$).
- **Why use rank instead of height:** Path compression flattens trees, changing their actual heights. Tracking the actual height of each node would require traversing the entire subtree to update heights after path compression, which is too expensive ($O(n)$ time). Instead, we use "rank", which is an upper bound on the tree height and is only updated during Union operations.

---

### Part b) [10 Points]
**Path Compression:**
- During `Find(x)`, the algorithm traverses the path from $x$ up to its root $r$.
- Once $r$ is found, the algorithm traverses back down the path and updates the parent pointer of $x$ and all of its ancestors along the path to point directly to $r$.
- **Synergy:**
  - **Union-by-Rank alone** guarantees that the maximum height of any tree is $O(\log n)$. But `Find` operations still take $O(\log n)$ in the worst case.
  - **Path Compression alone** can result in long chains if the trees are merged poorly, leading to $O(n)$ worst-case height.
  - **Combined Heuristics:** Union-by-Rank keeps the trees balanced, and Path Compression flattens them extremely quickly. Once a long path is traversed, it is compressed, making all future finds on those nodes run in $O(1)$ time. This combined synergy reduces the amortized cost per operation to $O(\alpha(n))$ (where $\alpha$ is the inverse Ackermann function).

---

## Question 2: Forest Tracing & Path Compression (30 Points)

### Part a) [15 Points]
We trace the 7 `Union` operations step-by-step:
1.  `Union(1, 2)`: Equal rank (0). Make 2 the parent of 1: `1 -> 2`. $\text{rank}(2) = 1$.
2.  `Union(3, 4)`: Equal rank (0). Make 4 the parent of 3: `3 -> 4`. $\text{rank}(4) = 1$.
3.  `Union(5, 6)`: Equal rank (0). Make 6 the parent of 5: `5 -> 6`. $\text{rank}(6) = 1$.
4.  `Union(7, 8)`: Equal rank (0). Make 8 the parent of 7: `7 -> 8`. $\text{rank}(8) = 1$.
5.  `Union(1, 3)`: Find(1) = 2 (rank 1), Find(3) = 4 (rank 1). Equal rank. Make 4 parent of 2: `2 -> 4`. $\text{rank}(4) = 2$.
    - Tree 1: `1 -> 2 -> 4` and `3 -> 4`. Root is 4 (rank 2).
6.  `Union(5, 7)`: Find(5) = 6 (rank 1), Find(7) = 8 (rank 1). Equal rank. Make 8 parent of 6: `6 -> 8`. $\text{rank}(8) = 2$.
    - Tree 2: `5 -> 6 -> 8` and `7 -> 8`. Root is 8 (rank 2).
7.  `Union(1, 5)`: Find(1) = 4 (rank 2), Find(5) = 8 (rank 2). Equal rank. Make 8 parent of 4: `4 -> 8`. $\text{rank}(8) = 3$.

**Resulting Tree Structure:**
- **Root:** 8 (rank 3)
- **Children of 8:** 6, 7, 4
- **Children of 4:** 2, 3
- **Children of 2:** 1
- **Children of 6:** 5

```
          8 (rank 3)
        /  |  \
       6   7   4 (rank 2)
      /       / \
     5       2   3
            /
           1
```

---

### Part b) [15 Points]
We execute `Find(1)` with **Path Compression**:
1.  Find(1) traverses the path: $1 \rightarrow 2 \rightarrow 4 \rightarrow 8$.
2.  The root is 8.
3.  We update the parent pointers of all nodes along the path (except the root) to point directly to 8:
    - `parent[1] = 8`
    - `parent[2] = 8`
    - `parent[4] = 8`

**Resulting Forest Structure:**
- **Root:** 8 (rank 3)
- **Children of 8:** 6, 7, 4, 2, 1
- **Children of 4:** 3
- **Children of 6:** 5

```
         8 (rank 3)
     / /  |  \  \
    6 7   4   2  1
   /     /
  5     3
```

---

## Question 3: Mathematical Properties of Rank (30 Points)

### Part a) [10 Points]
**Proof by Induction on rank $r$:**
- **Base Case ($r = 0$):** A node $x$ of rank 0 has a subtree containing at least $2^0 = 1$ node (which is $x$ itself). The base case holds.
- **Inductive Step:** Assume the property holds for all ranks up to $r-1$. A node $x$ can only achieve rank $r > 0$ when two subtrees with roots of rank $r-1$ are merged.
  - Let $u$ and $v$ be the roots of these two subtrees, where $\text{rank}(u) = \text{rank}(v) = r-1$.
  - When they are merged, one root (say $v$) becomes the parent of the other ($u$), and the rank of $v$ becomes $r$.
  - By the induction hypothesis, $u$'s subtree contains at least $2^{r-1}$ nodes, and $v$'s old subtree contains at least $2^{r-1}$ nodes.
  - The new subtree of $v$ contains all nodes from both subtrees. The total size is:
    $$\text{size}(v_{\text{new}}) = \text{size}(v_{\text{old}}) + \text{size}(u) \ge 2^{r-1} + 2^{r-1} = 2^r$$
Thus, the property holds for rank $r$. By induction, it holds for all ranks. $\blacksquare$

---

### Part b) [10 Points]
**Proof:**
1. From part (a), any node of rank $r$ is the root of a subtree containing at least $2^r$ nodes.
2. We observe that for any two nodes $x, y$ of the same rank $r$, their subtrees must be disjoint.
   - If they were not disjoint, one node (say $x$) would be a descendant of $y$. But in a disjoint-set forest, ranks strictly increase as we go up the tree, meaning $\text{rank}(x) < \text{rank}(y)$, which contradicts both having rank $r$.
3. Since each node of rank $r$ root a disjoint subtree of size at least $2^r$ in a forest containing $n$ elements in total, the number of such nodes $N_r$ must satisfy:
   $$N_r \cdot 2^r \le n \implies N_r \le \frac{n}{2^r}$$
This completes the proof. $\blacksquare$

---

### Part c) [10 Points]
**Proof:**
1. Let $r_{max}$ be the maximum rank of any node in the forest.
2. By part (a), the subtree rooted at this node contains at least $2^{r_{max}}$ nodes.
3. Since the total number of nodes in the entire forest is $n$:
   $$2^{r_{max}} \le n$$
4. Taking the binary logarithm of both sides:
   $$r_{max} \le \log_2 n$$
5. Since the rank must be an integer, we obtain:
   $$r_{max} \le \lfloor \log_2 n \rfloor$$
This completes the proof. $\blacksquare$

---

## Question 4: Amortized Analysis via the Potential Method (20 Points)

### Potential Method Application to Union-Find
In the potential method analysis of Union-Find:
1. We define a potential function $\Phi$ on the forest: $\Phi = \sum_{x \in V} \Phi(x)$.
2. For each node $x$:
   - If $x$ is a root or has rank 0, its potential is 0: $\Phi(x) = 0$.
   - If $x$ is an internal node of rank $r(x)$ with parent $p(x)$, we define its potential based on the gap between its rank and its parent's rank.
3. We group the ranks into intervals using Ackermann's function (or $\log^*$ function).
   - The potential of a node $x$ is high when its parent's rank is in a different group, and drops to 0 when its parent's rank increases to the same group or higher.
4. During a `Find` operation with path compression:
   - We traverse a path of length $L$.
   - Several internal nodes along this path are attached directly to the root. Their parents' ranks increase significantly.
   - This increase in parent ranks causes a large drop in their potential $\Delta \Phi < 0$.
   - The potential drop is large enough to compensate for the actual cost of traversing the path (the actual work $O(L)$ is paid for by the negative change in potential).
   - This leaves an amortized cost of only $O(\alpha(n))$ (or $O(\log^* n)$ depending on the grouping function) per operation.
5. Since the potential starts at 0 and is always non-negative, the total actual cost of any sequence of operations is at most the sum of amortized costs.
