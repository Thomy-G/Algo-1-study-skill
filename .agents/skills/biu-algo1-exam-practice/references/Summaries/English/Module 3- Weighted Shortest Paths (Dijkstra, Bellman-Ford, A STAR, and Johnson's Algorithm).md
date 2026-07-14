---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-17
---
# Module 3- Weighted Shortest Paths (Dijkstra, Bellman-Ford, A STAR, and Johnson's Algorithm)

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 3: Weighted Shortest Paths (Dijkstra, Bellman-Ford, A*, and Johnson's Algorithm)

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering weighted graph traversal optimization and path metric engineering. Mastery of these concepts is essential for successfully solving single-source and all-pairs shortest path problems in past exams.

### 1. Mathematical Foundations & Structural Relaxations

In past exams, you are frequently required to analyze convergence paths or leverage invariant distance fields. Shortest path computations rely entirely on the foundational principle of structural edge relaxation.

#### 1.1 Core Mathematical Mechanics

Let $G = (V, E)$ be a directed graph with a real-valued weight function $w: E \to \mathbb{R}$. The shortest-path distance from source $s$ to vertex $v$ is formally denoted by $\delta(s, v)$.

- **The Triangle Inequality (אי-שוויון המשולש):** For any edge $(u, v) \in E$:
    
    $$\delta(s, v) \le \delta(s, u) + w(u, v)$$
    
- **The Relaxation Primitive (פעולת ההרפיה):** For any edge $(u, v)$, the distance estimate $d[v]$ and predecessor tracking pointer $\pi[v]$ are updated via the following rule:
    
    Plaintext
    
    ```
    RELAX(u, v, w):
        if d[v] > d[u] + w(u, v):
            d[v] = d[u] + w(u, v)
            pi[v] = u
    ```
    

#### 1.2 Essential Convergence Lemmas

To construct rigorous proofs for open-ended exam questions, you must memorize these functional behavior bounds:

- **Upper-Bound Property:** $d[v] \ge \delta(s, v)$ holds at all times during execution. Once $d[v]$ equals $\delta(s, v)$, its value can never change.
    
- **No-Path Property:** If there is no path from $s$ to $v$, then $d[v] = \delta(s, v) = \infty$ remains an unalterable invariant.
    
- **Path-Relaxation Property:** If $p = \langle v_0, v_1, \dots, v_k \rangle$ is a shortest path from $s = v_0$ to $v_k$, and the edges of $p$ are relaxed in the exact sequence $(v_0, v_1), (v_1, v_2), \dots, (v_{k-1}, v_k)$, then $d[v_k] = \delta(s, v_k)$ is guaranteed to hold regardless of any interleaved relaxation steps on other edges.
    

### 2. Core Single-Source Algorithms (SSSP)

#### 2.1 Dijkstra's Algorithm (אלגוריתם דייקסטרה)

- **Core Constraint:** Requires all edge weights to be strictly non-negative: $w(u, v) \ge 0$.
    
- **Operational Strategy:** A greedy strategy that extracts the vertex with the minimum distance estimate $d[u]$ from a priority queue $Q$, fixes its final distance, and relaxes all of its outgoing edges.
    

Plaintext

```
DIJKSTRA(G, w, s):
    INITIALIZE-SINGLE-SOURCE(G, s) // d[s]=0, d[v]=inf for v!=s
    S = ∅
    Q = BUILD-PRIORITY-QUEUE(V)    // Keyed by d[u]
    while Q != ∅:
        u = EXTRACT-MIN(Q)
        S = S ∪ {u}
        for each v in Adj[u]:
            RELAX(u, v, w)
            if d[v] was updated:
                DECREASE-KEY(Q, v, d[v])
```

##### Priority Queue Complexity Tradeoffs

The execution performance of Dijkstra's algorithm depends directly on the underlying priority queue implementation:

|**Priority Queue Implementation**|**EXTRACT-MIN Cost**|**DECREASE-KEY Cost**|**Total Time Complexity**|
|---|---|---|---|
|**Unsorted Array**|$O(\|V\|)$|$O(1)$|$O(\|V\|^2)$|
|**Binary Heap (Min-Heap)**|$O(\log\|V\|)$|$O(\log\|V\|)$|$O(\|E\| \log \|V\|)$|
|**Fibonacci Heap**|$O(\log\|V\|)$ amortized|$O(1)$ amortized|$O(\|E\| + \|V\| \log \|V\|)$|

#### 2.2 The Bellman-Ford Algorithm (אלגוריתם בלמן-פורד)

- **Core Capability:** Handles arbitrary negative edge weights and formally detects the reachability of **negative-weight cycles**.
    
- **Operational Strategy:** Runs a comprehensive sweep across all $|E|$ edges exactly $|V|-1$ times.
    

Plaintext

```
BELLMAN-FORD(G, w, s):
    INITIALIZE-SINGLE-SOURCE(G, s)
    for i = 1 to |V| - 1:
        for each edge (u, v) in E:
            RELAX(u, v, w)
            
    for each edge (u, v) in E:
        if d[v] > d[u] + w(u, v):
            return FALSE // Negative-weight cycle detected!
    return TRUE
```

- **Complexity:** Time: $\mathbf{O(\|V\| \cdot \|E\|)}$, Space: $\Theta(|V|)$.
    

#### 2.3 Directed Acyclic Graph Shortest Paths (DAG-SSSP)

- **Core Constraint:** The graph must contain zero structural cycles (a DAG). It can handle negative weights.
- **Operational Strategy:** Linearly orders the graph via topological sorting, then relaxes all outgoing edges from each vertex in that exact sequence.

```plaintext
DAG-SSSP(G, w, s):
    Topologically sort the vertices of G to obtain order list L
    INITIALIZE-SINGLE-SOURCE(G, s)
    for each vertex u in L:
        for each v in Adj[u]:
            RELAX(u, v, w)
```
- **Complexity:** Time: $\mathbf{\Theta(\|V\| + \|E\|)}$, Space: $\Theta(|V|)$.

### 3. A* Search & Potential Reweighting Primitives

#### 3.1 The A* Search Framework

A* accelerates SSSP toward a target vertex $t$ by combining the actual cost from the source, $d(s, u)$, with a heuristic estimation function, $h(u)$, which approximates the remaining distance to $t$.

- **Modified Edge Weights:** A* runs Dijkstra's algorithm using a modified weight field:
    
    $$w_h(u, v) = w(u, v) + h(v) - h(u)$$

```plaintext
A-STAR(G, w, s, t, h):
    for each vertex u in V:
        d[u] = inf
        pi[u] = NIL
    d[s] = 0
    Q = BUILD-PRIORITY-QUEUE(V) // Keyed by f[u] = d[u] + h(u)
    while Q != ∅:
        u = EXTRACT-MIN(Q)
        if u == t:
            return d[t] // Target reached!
        for each v in Adj[u]:
            if d[v] > d[u] + w(u, v):
                d[v] = d[u] + w(u, v)
                pi[v] = u
                if v in Q:
                    DECREASE-KEY(Q, v, d[v] + h(v))
                else:
                    INSERT(Q, v, d[v] + h(v))
```

- **Admissibility Constraint (קבילות):** A heuristic $h(u)$ is admissible if it never overestimates the true remaining cost to the destination: $0 \le h(u) \le \delta(u, t)$ for all $u \in V$.
- **Consistency/Monotonicity Constraint (עקביות):** A heuristic $h(u)$ is consistent if it satisfies the triangle inequality for all edges $(u, v) \in E$:
    $$h(u) \le w(u, v) + h(v)$$
    _(Note: Any consistent heuristic is automatically admissible)._
    

#### 3.2 Johnson's Algorithm for All-Pairs Shortest Paths (APSP)

Johnson's algorithm computes all-pairs shortest paths on a graph with arbitrary edge weights in $O(|V|^2 \log |V| + |V||E|)$ time. It does this by creating a valid potential function to eliminate negative edge weights, allowing it to run Dijkstra's algorithm from every vertex.

##### Step-by-Step Reweighting Formula Flow

1. **Construct a Dummy Source Network ($G'$):** Add a virtual vertex $s'$ to the graph, along with directed edges $(s', v)$ of weight $0$ to every vertex $v \in V$.
2. **Compute the Potential Field ($h$):** Run Bellman-Ford from $s'$ on $G'$. If a negative cycle is detected, abort execution. Otherwise, set the vertex potential $h(u) = \delta(s', u)$ for all $u \in V$.
3. **Enforce Feasible Reweighting (פוטנציאל פיזיבילי):** Reweight every edge in $G$ to create a non-negative weight function $\hat{w}$:
    $$\hat{w}(u, v) = w(u, v) + h(u) - h(v) \ge 0$$
4. **Run All-Pairs Dijkstra:** Run Dijkstra's algorithm from each vertex $u \in V$ using the non-negative weight function $\hat{w}$.
5. **Reconstruct Authentic Distances:** Restore the true shortest-path distances by reversing the potential transformation:
    $$\delta(u, v) = \hat{\delta}(u, v) - h(u) + h(v)$$

```plaintext
JOHNSON(G, w):
    G' = (V ∪ {s'}, E ∪ {(s', v) : v in V})
    for each v in V:
        w(s', v) = 0
    if BELLMAN-FORD(G', w, s') == FALSE:
        error "Negative-weight cycle detected!"
    else:
        for each v in V:
            h(v) = delta(s', v) // computed by Bellman-Ford
        for each edge (u, v) in E:
            w_hat(u, v) = w(u, v) + h(u) - h(v)
        allocate D matrix of size |V| x |V|
        for each u in V:
            run DIJKSTRA(G, w_hat, u) to compute delta_hat(u, v) for all v in V
            for each v in V:
                D[u, v] = delta_hat(u, v) - h(u) + h(v)
        return D
```
    

### 4. Exam-Style Applications & Problem Solving Techniques

#### 4.1 Paradigm A: Even/Odd Parity Constraint SSSP Optimization (Past BIU Exams)

> [!IMPORTANT]
> 
> **Exam Target:** When a shortest path problem introduces a structural constraint—such as requiring the path to have an even number of edges, an odd number of edges, or exactly $k$ colored edges—solve it by **multiplying the vertex state space** to create an explicit, unrolled layered graph.

#### 4.2 Worked Example: Multi-Metric Path-Weight Balancing

Suppose you are given a directed graph $G = (V, E)$ with a non-negative edge weight function $w: E \to \mathbb{R}^+$. Every edge $e \in E$ is also assigned a color: either **Red** or **Blue**. Design an algorithm running in $O(|V| \log |V| + |E|)$ time that finds the weight of the shortest path from a source $s$ to a target $t$ such that the path contains an **even number of Red edges** and an **odd number of Blue edges**.

##### Step-by-Step Problem Resolution:

1. **Construct a State-Expanded Layered Graph ($G^*$):**
    
    We can model the required parity constraints by splitting each original vertex $u \in V$ into four distinct states in $G^*$:
    
    $$u^* = (u, \text{RedParity}, \text{BlueParity}) \quad \text{where } \text{Parity} \in \{0, 1\}$$
    
    - State $(u, 0, 0)$: Even Red edges, Even Blue edges encountered so far.
        
    - State $(u, 0, 1)$: Even Red edges, Odd Blue edges encountered so far.
        
    - State $(u, 1, 0)$: Odd Red edges, Even Blue edges encountered so far.
        
    - State $(u, 1, 1)$: Odd Red edges, Odd Blue edges encountered so far.
        
2. **Map the Transformed Edge Set ($E^*$):**
    
    For every original directed edge $e = (u, v) \in E$ with weight $w(u, v)$, map its transitions across the parity layers in $G^*$:
    
    - **If $e$ is Red:** It flips the Red parity bit while preserving the Blue parity bit. Add these four edges to $E^*$:
        
        $$\big((u, 0, 0), (v, 1, 0)\big), \quad \big((u, 0, 1), (v, 1, 1)\big), \quad \big((u, 1, 0), (v, 0, 0)\big), \quad \big((u, 1, 1), (v, 0, 1)\big)$$
        
        All four edges inherit the original weight: $w(u, v)$.
        
    - **If $e$ is Blue:** It flips the Blue parity bit while preserving the Red parity bit. Add these four edges to $E^*$:
        
        $$\big((u, 0, 0), (v, 0, 1)\big), \quad \big((u, 0, 1), (v, 0, 0)\big), \quad \big((u, 1, 0), (v, 1, 1)\big), \quad \big((u, 1, 1), (v, 1, 0)\big)$$
        
        All four edges inherit the original weight: $w(u, v)$.
        
3. **Execute Dijkstra's Algorithm on the Layered Graph:**
    
    - **Source Initialization:** Set the starting vertex to $(s, 0, 0)$ with an initial distance of $0$. All other state vertices are initialized to $\infty$.
        
    - Run Dijkstra's algorithm using a Min-Priority Queue on the expanded graph $G^* = (V^*, E^*)$.
        
4. **Extract the Target Solution State:**
    
    The target solution must satisfy the target parities (Even Red $\implies 0$, Odd Blue $\implies 1$). Locate and return the calculated distance value stored at state vertex:
    
    $$\text{Result} = d\big[(t, 0, 1)\big]$$
    
5. **Complexity Analysis Verification:**
    
    - $|V^*| = 4|V| = O(|V|)$ vertices.
        
    - $|E^*| = 4|E| = O(|E|)$ edges.
        
    - Running Dijkstra's algorithm via a binary heap takes $O(|E^*| \log |V^*|) = \mathbf{O(\|E\| \log \|V\|)}$, satisfying the target runtime bound perfectly.
        

#### 4.3 Paradigm B: Incremental Shortest Path Corrections (2024 Moed A / Moed B)

> [!NOTE]
> 
> **Exam Target:** If an edge weight changes or a vertex is removed after you have already computed all shortest paths, do not recompute the paths from scratch using an expensive algorithm. Instead, utilize your precomputed distance matrix to find the updated paths in $O(|V|^2)$ or $O(|V|)$ time.

#### 4.4 Worked Example: Single-Vertex Edge Restructuring Recovery

You are given a directed graph $G = (V, E)$ with non-negative edge weights. A target vertex $v^* \in V$ is completely removed from the graph, along with all of its adjacent edges. You are given a precomputed matrix $D'$ containing the exact all-pairs shortest path distances for the vertex-induced subgraph $G' = G[V \setminus \{v^*\}]$. Design an algorithm running in **$O(|V|^2)$ time** that reconstructs the authentic all-pairs shortest path distance matrix $D$ for the _original_ graph $G$ (reintroducing $v^*$ and its adjacent edges).

##### Step-by-Step Problem Resolution:

1. **Isolate the Subproblem Vectors:**
    
    To reconstruct the complete matrix $D$, we need to find the shortest paths that pass through the reintroduced vertex $v^*$. We can do this by computing three components:
    
    - Vector $\text{To\_}v^*[u]$: The shortest path distance from any vertex $u \in V \setminus \{v^*\}$ to $v^*$.
        
    - Vector $\text{From\_}v^*[u]$: The shortest path distance from $v^*$ to any vertex $u \in V \setminus \{v^*\}$.
        
    - Scalar $\delta(v^*, v^*)$: The shortest path distance from $v^*$ to itself, initialized to $0$.
        
2. **Compute Inbound and Outbound Distance Vectors:**
    
    Any path to or from $v^*$ can be broken down into a single direct edge connected to $v^*$ preceded or followed by a shortest path contained entirely within $G'$.
    
    - For every vertex $u \in V \setminus \{v^*\}$, evaluate all outgoing options to solve $\text{To\_}v^*[u]$ in $O(|V|)$ time:
        
        $$\text{To\_}v^*[u] = \min_{x \in \text{In-Neighbors}(v^*)} \big\{ D'[u, x] + w(x, v^*) \big\}$$
        
    - For every vertex $u \in V \setminus \{v^*\}$, evaluate all incoming options to solve $\text{From\_}v^*[u]$ in $O(|V|)$ time:
        
        $$\text{From\_}v^*[u] = \min_{y \in \text{Out-Neighbors}(v^*)} \big\{ w(v^*, y) + D'[y, u] \big\}$$
        
3. **Reconstruct the Matrix using the Precomputed Potential Paths:**
    
    We can now construct the final matrix $D$ for all pairs $(i, j) \in V \times V$. For any pair of vertices $i$ and $j$, the shortest path either passes through $v^*$ or it does not:
    
    $$D[i, j] = \min \big\{ D'[i, j], \ \text{To\_}v^*[i] + \text{From\_}v^*[j] \big\}$$
    
    _(Note: If $i = v^*$ or $j = v^*$, simply substitute the corresponding vector component directly).*
    
4. **Complexity Analysis Verification:**
    
    - Computing the $\text{To\_}v^*$ vector requires filling out $|V|-1$ entries, where each entry takes $O(|V|)$ time to scan neighbors $\implies O(|V|^2)$.
        
    - Computing the $\text{From\_}v^*$ vector similarly takes $\implies O(|V|^2)$ time.
        
    - Updating the $|V| \times |V|$ distance matrix entries takes exactly $O(1)$ time per cell $\implies O(|V|^2)$.
        
    - Total Runtime: **$\mathbf{O(\|V\|^2)}$**, bypassing the standard $O(|V|^3)$ All-Pairs Shortest Paths processing bottleneck.


## Module 3.5: Advanced Shortest Path Engineering (Seidel's Matrix APSP, Transitive Closure, and A* Search)

This module compiles the advanced theoretical structures, mathematical limits, and algebraic graph transformations covering **Syllabus Week 5**. It details unweighted all-pairs optimizations, matrix reachability expansions, and heuristic-driven single-source traversals.

### 1. Foundational Vocabulary & Algebraic Path Matrices

In past exams, you are frequently required to translate standard graph path exploration into raw matrix operations or verify triangle invariants.

#### 1.1 The Transitive Closure & Boolean Reductions

The **Transitive Closure** (סגור טרנזיטיבי) of a directed graph $G = (V, E)$ is a graph $G^* = (V, E^*)$ where $(u, v) \in E^* \iff$ there exists a valid directed path $u \leadsto v$ of any length in $G$.

Instead of running an expensive $\Theta(|V|^3)$ Floyd-Warshall loop, we can compute reachability algebraically using the graph's Boolean **Adjacency Matrix** $A$. Let $I$ be the $|V| \times |V|$ Identity Matrix, and define $A' = A \lor I$ (setting all diagonal entries to 1, representing that every node can reach itself). The Transitive Closure matrix is equal to the Boolean matrix power:

$$(A')^{|V|-1} = \underbrace{A' \cdot A' \dots A'}_{|V|-1 \text{ times}}$$

Using **Square-and-Multiply (כפל מטריצות בריבוע)**, we can compute this matrix power by squaring the matrix exactly $\lceil \log_2 |V| \rceil$ times. If standard matrix multiplication costs $O(|V|^\omega)$ where $\omega < 2.373$ (e.g., using Strassen's or advanced Fast Matrix Multiplication), the transitive closure can be solved in **$O(|V|^\omega \log |V|)$ time**.

*   **Proof of Validity:**
    1.  *Boolean Exponentiation Invariant:* We prove by induction on $k \ge 1$ that $(A^k)_{ij} = 1 \iff$ there exists a directed walk of length exactly $k$ from $i$ to $j$.
        - *Base case ($k=1$):* $(A^1)_{ij} = A_{ij} = 1 \iff (i, j) \in E$, which is a walk of length 1.
        - *Inductive step:* By Boolean matrix multiplication definition, $(A^{k+1})_{ij} = \bigvee_{m=1}^{|V|} \left((A^k)_{im} \land A_{mj}\right)$. This is 1 iff there exists some vertex $m$ such that $(A^k)_{im} = 1$ (there is a walk of length $k$ from $i$ to $m$) and $A_{mj} = 1$ (there is an edge from $m$ to $j$). This is equivalent to a walk of length $k+1$ from $i$ to $j$.
    2.  *Self-Loops Expansion:* Let $A' = A \lor I$. The addition of the identity matrix $I$ represents a self-loop on every vertex. Any path of length $r \le |V|-1$ can be extended to a walk of length exactly $|V|-1$ by traversing the self-loop at any vertex along the path exactly $(|V|-1) - r$ times. Thus, $((A')^{|V|-1})_{ij} = 1 \iff$ there is a path of length $\le |V|-1$ from $i$ to $j$, which is the definition of reachability.

#### 1.2 Triangle Detection via Matrix Traces

To check for the existence of a **Triangle** (משולש)—a three-node simple cycle $u \to v \to w \to u$—in an undirected graph, evaluate the diagonal entries of the cube of the adjacency matrix, $A^3$.

- The entry $A^3[i, i]$ counts the exact number of paths of length 3 starting and ending at vertex $v_i$.
    
- **The Trace Formula:** The graph contains a triangle if and only if the trace (the sum of the diagonal elements) is strictly non-zero:
    
    $$\text{Trace}(A^3) = \sum_{i=1}^{|V|} A^3[i, i] > 0$$
    
    - _Operational Cost:_ Requires exactly one matrix multiplication step $\implies \mathbf{O(\|V\|^\omega)}$.
        
*   **Proof of the Trace Formula:**
    1.  *Diagonal Entry Meaning:* From the Boolean exponentiation proof, the entry $A^3[i, i]$ counts the exact number of walks of length 3 starting and ending at $v_i$.
    2.  *Walks to Simple Cycles:* Let $v_i \to u \to v \to v_i$ be a walk of length 3. Since there are no self-loops ($A[j, j]=0$ for all $j$) and the graph is undirected, $u \neq v_i$ and $v \neq v_i$. Also, if $u = v$, the walk would be $v_i \to u \to u \to v_i$, which requires a self-loop at $u$, impossible. Thus, $v_i, u, v$ must be three distinct vertices, meaning every walk of length 3 starting and ending at $v_i$ is a simple 3-cycle (a triangle).
    3.  *Symmetric Counting Factor:* Every triangle $\{u, v, w\}$ consists of three distinct vertices.
        - Starting at $u$, there are two directed cycles: $u \to v \to w \to u$ and $u \to w \to v \to u$.
        - Starting at $v$, there are two directed cycles: $v \to w \to u \to v$ and $v \to u \to w \to v$.
        - Starting at $w$, there are two directed cycles: $w \to u \to v \to w$ and $w \to v \to u \to w$.
        Thus, each triangle is counted exactly 6 times in the trace sum:
        $$\text{Number of Triangles} = \frac{\text{Trace}(A^3)}{6}$$
        Hence, $\text{Trace}(A^3) > 0$ iff the graph contains at least one triangle.

#### 1.3 The Floyd-Warshall Algorithm (אלגוריתם פלויד-וורשאל)

*   **Objective:** Computes all-pairs shortest paths (APSP) in a weighted directed graph that can contain negative edge weights (but no negative-weight cycles) in $O(|V|^3)$ time.
*   **Dynamic Programming Strategy:** 
    Let $d_{ij}^{(k)}$ be the weight of a shortest path from vertex $i$ to vertex $j$ such that all intermediate vertices on the path are from the set $\{1, 2, \dots, k\}$.
*   **Recurrence Relation:**
    $$d_{ij}^{(k)} = \min\left( d_{ij}^{(k-1)}, \ d_{ik}^{(k-1)} + d_{kj}^{(k-1)} \right)$$
    *Intuition:* The shortest path from $i$ to $j$ using intermediate vertices in $\{1, \dots, k\}$ either does not use the vertex $k$ at all (costing $d_{ij}^{(k-1)}$), or it uses $k$ exactly once, splitting the path into $i \rightsquigarrow k$ and $k \rightsquigarrow j$. Both subpaths only use intermediate vertices in $\{1, \dots, k-1\}$, costing $d_{ik}^{(k-1)} + d_{kj}^{(k-1)}$.
*   **Base Cases ($k=0$):**
    $$d_{ij}^{(0)} = \begin{cases} 0 & i == j \\ w(i, j) & i \neq j \text{ and } (i, j) \in E \\ \infty & \text{otherwise} \end{cases}$$

```plaintext
FLOYD-WARSHALL(W):
    n = rows[W]
    D(0) = W
    Initialize predecessor matrix PI(0) where PI(0)[i, j] = i if (i, j) in E, else NIL
    for k = 1 to n:
        allocate D(k) and PI(k) of size n x n
        for i = 1 to n:
            for j = 1 to n:
                if D(k-1)[i, j] <= D(k-1)[i, k] + D(k-1)[k, j]:
                    D(k)[i, j] = D(k-1)[i, j]
                    PI(k)[i, j] = PI(k-1)[i, j]
                else:
                    D(k)[i, j] = D(k-1)[i, k] + D(k-1)[k, j]
                    PI(k)[i, j] = PI(k-1)[k, j]
    return D(n), PI(n)
```
*   **Complexity:** Time: $\Theta(|V|^3)$, Space: $\Theta(|V|^2)$ (both matrices can be updated in-place by omitting the superscript $k$ since values in the $k$-th row/column do not change during step $k$).

##### 1. Generalized Algebraic Framework & Applications
The Floyd-Warshall algorithm is a specific instance of a **closed semi-ring path algorithm**. By replacing the operations $(\min, +)$ with other algebraic structures, it can solve a wide range of graph problems:

1.  **Transitive Closure (Warshall's Algorithm):**
    *   *Problem:* Determine if there exists a path $i \rightsquigarrow j$ for all pairs.
    *   *Operations:* Replace $\min$ with logical $\lor$ (OR), and $+$ with logical $\land$ (AND).
    *   *Recurrence:* $t_{ij}^{(k)} = t_{ij}^{(k-1)} \lor \left(t_{ik}^{(k-1)} \land t_{kj}^{(k-1)}\right)$.
2.  **Maximum Capacity / Widest Path (Bottleneck Path):**
    *   *Problem:* Find a path between $i$ and $j$ that maximizes the minimum capacity of any edge along the path (e.g., maximum network bandwidth or vehicle weight limits).
    *   *Operations:* Replace $\min$ with $\max$, and $+$ with $\min$.
    *   *Recurrence:* $c_{ij}^{(k)} = \max\left(c_{ij}^{(k-1)}, \ \min\left(c_{ik}^{(k-1)}, c_{kj}^{(k-1)}\right)\right)$.
3.  **Minimax Path (Minimum Bottleneck Path):**
    *   *Problem:* Find a path between $i$ and $j$ that minimizes the maximum edge weight along the path (e.g., minimizing the longest stretch between refueling stations).
    *   *Operations:* Replace $\min$ with $\min$, and $+$ with $\max$.
    *   *Recurrence:* $b_{ij}^{(k)} = \min\left(b_{ij}^{(k-1)}, \ \max\left(b_{ik}^{(k-1)}, b_{kj}^{(k-1)}\right)\right)$.
4.  **Most Reliable Path (Product Path):**
    *   *Problem:* Given edge success probabilities $p(u, v) \in [0, 1]$, find a path maximizing the product of edge probabilities.
    *   *Operations:* Replace $\min$ with $\max$, and $+$ with multiplication $\cdot$.
    *   *Recurrence:* $r_{ij}^{(k)} = \max\left(r_{ij}^{(k-1)}, \ r_{ik}^{(k-1)} \cdot r_{kj}^{(k-1)}\right)$.
5.  **Negative Cycle Detection:**
    *   If after the $n$-th iteration, there exists any vertex $i$ such that $D[i, i] < 0$, then the graph contains a negative-weight cycle containing or reachable from $i$. This is because $D[i, i]$ represents the shortest path from $i$ back to $i$; if it is negative, a cycle with total negative weight exists.
#### 1.4 Triangle Detection & Matrix Reductions (זיהוי משולשים ורדוקציות מטריציאליות)

##### 1. Definition of Boolean Matrix Multiplication (BMM)
*   **The Formulation:** For two Boolean matrices $A, B \in \{0, 1\}^{n \times n}$, the Boolean product $C = A \odot B$ is defined as:
    $$C_{ij} = \bigvee_{k=1}^n \left( A_{ik} \land B_{kj} \right)$$
    where $\lor$ is logical OR and $\land$ is logical AND.
*   **Algebraic Computation via FMM:**
    Since Boolean matrix operations lack additive inverses (there is no subtraction operation corresponding to OR), we cannot apply Fast Matrix Multiplication (like Strassen's or $O(n^\omega)$ algorithms) directly on the Boolean semiring.
    Instead, we compute the Boolean product by:
    1. Treating the Boolean matrices as standard integer matrices and computing their standard matrix product: $C'_{ij} = \sum_{k=1}^n A_{ik} \cdot B_{kj}$ in $O(n^\omega)$ time.
    2. Applying a thresholding step in $O(n^2)$ time: setting $C_{ij} = 1$ if $C'_{ij} > 0$, and $C_{ij} = 0$ if $C'_{ij} = 0$.
    Thus, Boolean Matrix Multiplication can be solved in standard **$O(n^\omega)$** time.

##### 2. Equivalence of Triangle Detection (TD) and Boolean Matrix Multiplication (BMM)
*   **The Theorem:** Finding if a graph contains a triangle is computationally equivalent to Boolean Matrix Multiplication. Specifically, if Triangle Detection can be solved in $O(n^{3-\varepsilon})$ time, then BMM can be solved in $O(n^{3-\varepsilon/3})$ time.
*   **The Tripartite Reduction Construction:**
    Given two $n \times n$ Boolean matrices $A$ and $B$, we construct a tripartite graph $G = (V, E)$ with three partitions $I, J, K$ of size $n$ each:
    - Edges between $I$ and $J$: $e = (i, j) \in E \iff A_{ij} = 1$.
    - Edges between $J$ and $K$: $e = (j, k) \in E \iff B_{jk} = 1$.
    - Edges between $K$ and $I$: Include all possible edges $(k, i)$ for $k \in K, i \in I$.
    *Observation:* Node $i \in I$ and $k \in K$ form a triangle with some node $j \in J$ if and only if $A_{ij} = 1$ and $B_{jk} = 1$, which is equivalent to the Boolean product entry $C_{ik} = (A \cdot B)_{ik} = 1$.
*   **The Sub-Partitioning Algorithm:**
    To find all entries of $C = A \cdot B$ without running a slow $O(n^4)$ loop:
    1. Divide each partition $I, J, K$ into $t$ sub-blocks of size $n/t$ each.
    2. For each of the $t^3$ triplets of blocks $(I_x, J_y, K_z)$:
       - Run a Triangle Finder algorithm on the induced subgraph of size $3n/t$.
       - Whenever a triangle $(i, j, k)$ is found, set $C_{ik} = 1$ and delete the edge $(k, i)$ from $G$ (to avoid finding the same triangle again).
    3. **Optimal Partition Choice:** Setting $t = n^{2/3}$ minimizes the total runtime. The number of calls to the Triangle Finder is at most $n^2$ (for successful deletions) plus $t^3$ (for failures), leading to a total time of:
       $$(n^2 + t^3) \cdot O\left( \left(\frac{n}{t}\right)^{3-\varepsilon} \right) \implies \mathbf{O(n^{3-\varepsilon/3})}$$

##### 3. Bounded Weight Distance Product (Min-Plus Multiplication) via BMM
*   **Problem:** Given two matrices $A, B \in ([M] \cup \{\infty\})^{n \times n}$, compute the distance product $C = A \star B$ where $C_{ij} = \min_{k} (A_{ik} + B_{kj})$ for a small integer weight bound $M$.
*   **The Reduction:**
    1. Define $A_r$ for each $r \in [1, M]$ where $(A_r)_{ij} = 1 \iff A_{ij} = r$ (and $0$ otherwise).
    2. Define $B_s$ for each $s \in [1, M]$ where $(B_s)_{ij} = 1 \iff B_{ij} = s$ (and $0$ otherwise).
    3. Compute all $M^2$ products $C_{r, s} = A_r \cdot B_s$ using standard BMM.
    4. Construct $C_{ij} = \min \{ r + s \mid (C_{r, s})_{ij} == 1 \}$.
*   **Complexity:** Runs in **$O(M^2 \cdot n^\omega)$** time.

##### 4. Sparse Triangle Detection via Heavy/Light Node Partitioning
*   **Problem:** Detect if an undirected graph $G = (V, E)$ contains at least one triangle in $O(|E|^{\frac{2\omega}{\omega+1}}) = O(|E|^{1.41})$ time.
*   **The Alon-Yuster-Zwick (AYZ) Strategy:**
    1.  Classify vertices based on their degree using a threshold $d$:
        *   **Heavy Vertices ($V_{heavy}$):** Vertices with $\text{deg}(v) \ge d$. Since the sum of degrees is $2|E|$, the number of heavy vertices is bounded by:
            $$|V_{heavy}| \le \frac{2|E|}{d}$$
        *   **Light Vertices ($V_{light}$):** Vertices with $\text{deg}(v) < d$.
    2.  **Case 1: The triangle contains at least one light vertex.**
        For each vertex $u \in V$:
        *   If $u$ has at least one neighbor that is a light vertex (say $v \in V_{light}$):
            *   Iterate over all neighbors $w$ of the light vertex $v$. Since $\text{deg}(v) < d$, there are at most $d$ neighbors.
            *   Check if $w$ is also a neighbor of $u$ (which takes $O(1)$ time using adjacency matrix representation or hash table lookups). If yes, report triangle $(u, v, w)$.
            *   The total time to check all light neighbors over all edges is:
                $$\sum_{(u, v) \in E \text{ s.t. } v \in V_{light}} \text{deg}(v) \le |E| \cdot d$$
    3.  **Case 2: The triangle contains only heavy vertices.**
        *   Extract the induced subgraph $G_{heavy}$ containing only the heavy vertices $V_{heavy}$.
        *   Since $|V_{heavy}| \le 2|E|/d$, we can check for triangles in $G_{heavy}$ using the fast matrix multiplication trace method in time:
            $$O\left( |V_{heavy}|^\omega \right) = O\left( \left(\frac{|E|}{d}\right)^\omega \right)$$
    4.  **Optimal Threshold Balancing:**
        To minimize the total time, we equate the cost of both cases:
        $$|E| \cdot d = \left(\frac{|E|}{d}\right)^\omega \implies d^{\omega+1} = |E|^{\omega-1} \implies d = |E|^{\frac{\omega-1}{\omega+1}}$$
        Substituting this optimal threshold $d$ back into the cost formula gives the total time:
        $$\text{Total Time} = O\left( |E| \cdot d \right) = O\left( |E| \cdot |E|^{\frac{\omega-1}{\omega+1}} \right) = O\left( |E|^{\frac{2\omega}{\omega+1}} \right)$$
        Using the current bounds for matrix multiplication exponent $\omega \approx 2.373$, the total running time is **$O(|E|^{1.41})$** time, which is highly optimal for sparse graphs.


### 2. Seidel’s All-Pairs Shortest Path Algorithm (APSP)

Designed strictly for **unweighted, connected, undirected graphs**, Seidel’s algorithm uses Fast Matrix Multiplication (FMM) to solve the All-Pairs Shortest Paths problem in $O(|V|^\omega \log |V|)$ time.

#### 2.1 The Algebraic Squaring Mechanics

Given an unweighted adjacency matrix $A$, Seidel's algorithm constructs a modified squared matrix $Z = A^2 \lor A$ to contract the graph's distances.



```
SEIDEL(A):
    if A is all 1s (except for the diagonal): 
        return A - I
    
    Construct B: B[i, j] = 1 if (i != j and (A[i, j] == 1 or A^2[i, j] == 1)), else 0
    D' = SEIDEL(B) // Recursive call on contracted graph
    
    Compute product matrix: M = D' · A (Standard integer matrix multiplication)
    
    Construct Final Distance Matrix D:
    for each pair (u, v):
        if M[u, v] >= D'[u, v] · deg(v):
            D[u, v] = 2 · D'[u, v]
        else:
            D[u, v] = 2 · D'[u, v] - 1
    return D
```

#### 2.2 Detailed Mathematical Proof & Parity Correction Invariant

To understand why Seidel's algorithm works, we analyze the relationship between the shortest path distance $d(u, v)$ in the original graph $G = (V, E)$ and the shortest path distance $d'(u, v)$ in the contracted graph $G' = (V, E')$.

##### 2.2.1 Distance Contraction Lemma
*   **Lemma:** For any two vertices $u, v \in V$, the distance in the contracted graph satisfies:
    $$d'(u, v) = \lceil d(u, v) / 2 \rceil$$
*   **Proof:**
    1.  **Showing $d'(u, v) \le \lceil d(u, v) / 2 \rceil$:** 
        Let $P = (u_0, u_1, \dots, u_r)$ be the shortest path between $u = u_0$ and $v = u_r$ in $G$, so $d(u, v) = r$.
        *   If $r = 2k$ is even, the sequence of vertices $(u_0, u_2, u_4, \dots, u_{2k})$ forms a path in $G'$ because the distance between $u_{2i}$ and $u_{2(i+1)}$ in $G$ is exactly 2, meaning there is an edge between them in $G'$. The length of this path in $G'$ is $k$, so $d'(u, v) \le k = r/2$.
        *   If $r = 2k-1$ is odd, the sequence $(u_0, u_2, \dots, u_{2k-2}, u_{2k-1})$ forms a path in $G'$ of length $k$, so $d'(u, v) \le k = (r+1)/2$.
        *   Combining both cases: $d'(u, v) \le \lceil d(u, v) / 2 \rceil$.
    2.  **Showing $d'(u, v) \ge \lceil d(u, v) / 2 \rceil$:**
        Let $P' = (v_0, v_1, \dots, v_m)$ be the shortest path between $u = v_0$ and $v = v_m$ in $G'$, so $d'(u, v) = m$.
        *   By definition, each edge $(v_i, v_{i+1})$ in $G'$ corresponds to a path of length at most 2 in $G$.
        *   Therefore, the path $P'$ corresponds to a walk in $G$ of length at most $2m$.
        *   Thus, the shortest path distance in $G$ is $d(u, v) \le 2m = 2 d'(u, v) \implies d'(u, v) \ge d(u, v) / 2 \implies d'(u, v) \ge \lceil d(u, v)/2 \rceil$.
    3.  From (1) and (2), we conclude $d'(u, v) = \lceil d(u, v) / 2 \rceil$. $\blacksquare$

##### 2.2.2 Parity Correction Rule Proof
Since $d'(u, v) = \lceil d(u, v) / 2 \rceil$, the true distance $d(u, v)$ is either:
*   **Even Case:** $d(u, v) = 2 d'(u, v)$
*   **Odd Case:** $d(u, v) = 2 d'(u, v) - 1$

To distinguish these cases, let $N(v) = \{ w \in V \mid (w, v) \in E \}$ be the set of neighbors of $v$ in $G$. For any neighbor $w \in N(v)$, the triangle inequality ensures $|d(u, w) - d(u, v)| \le 1$. Since $G$ is unweighted, the distance parities of neighbors must differ, meaning $d(u, w) = d(u, v) \pm 1$.

Let us compute the neighbor-sum product entry $M[u, v]$:
$$M[u, v] = (D' \cdot A)_{uv} = \sum_{w \in N(v)} d'(u, w)$$

*   **Case 1: $d(u, v) = 2 d'(u, v)$ (Even)**
    *   For any neighbor $w \in N(v)$, $d(u, w) \in \{ 2 d'(u, v) - 1, \ 2 d'(u, v) + 1 \}$.
    *   If $d(u, w) = 2 d'(u, v) - 1 \implies d'(u, w) = \lceil (2 d'(u, v) - 1)/2 \rceil = d'(u, v)$.
    *   If $d(u, w) = 2 d'(u, v) + 1 \implies d'(u, w) = \lceil (2 d'(u, v) + 1)/2 \rceil = d'(u, v) + 1$.
    *   In both cases, we have $d'(u, w) \ge d'(u, v)$.
    *   Therefore, summing over all neighbors:
        $$M[u, v] = \sum_{w \in N(v)} d'(u, w) \ge \sum_{w \in N(v)} d'(u, v) = d'(u, v) \cdot \text{deg}(v)$$
*   **Case 2: $d(u, v) = 2 d'(u, v) - 1$ (Odd)**
    *   For any neighbor $w \in N(v)$, $d(u, w) \in \{ 2 d'(u, v) - 2, \ 2 d'(u, v) \}$.
    *   If $d(u, w) = 2 d'(u, v) - 2 \implies d'(u, w) = \lceil (2 d'(u, v) - 2)/2 \rceil = d'(u, v) - 1$.
    *   If $d(u, w) = 2 d'(u, v) \implies d'(u, w) = \lceil 2 d'(u, v) / 2 \rceil = d'(u, v)$.
    *   In all cases, we have $d'(u, w) \le d'(u, v)$.
    *   Furthermore, there must exist at least one neighbor $w^* \in N(v)$ that lies on the shortest path from $u$ to $v$. For this neighbor, $d(u, w^*) = d(u, v) - 1 = 2 d'(u, v) - 2 \implies d'(u, w^*) = d'(u, v) - 1 < d'(u, v)$.
    *   Therefore, summing over all neighbors:
        $$M[u, v] = \sum_{w \in N(v)} d'(u, w) < \sum_{w \in N(v)} d'(u, v) = d'(u, v) \cdot \text{deg}(v)$$

##### Summary of Parity Decision:
$$d(u, v) = \begin{cases} 
2 d'(u, v) & \text{if } M[u, v] \ge d'(u, v) \cdot \text{deg}(v) \\
2 d'(u, v) - 1 & \text{if } M[u, v] < d'(u, v) \cdot \text{deg}(v)
\end{cases}$$

##### 2.2.3 Complexity Analysis Recurrence
*   **Recursion Depth:** Since each contraction step yields $d'(u, v) = \lceil d(u, v)/2 \rceil$, the maximum distance (diameter of the graph) is halved at each level. For a graph of size $n$, the recursion depth is at most $\lceil \log_2 n \rceil$.
*   **Operations per Level:**
    1.  Computing $A^2$ to construct $B$ takes $O(n^\omega)$ time using Fast Matrix Multiplication.
    2.  Computing the product matrix $M = D' \cdot A$ takes $O(n^\omega)$ time.
    3.  Constructing $D$ via neighbor degree comparisons takes $O(n^2)$ time.
*   **Recurrence Relation:**
    $$T(n) = T(n) + O(n^\omega) \implies T(n) = \mathbf{O(n^\omega \log n)}$$

##### 2.2.4 Python Implementation (NumPy & Standard List variants)

```python
import numpy as np

def seidel_apsp_numpy(A):
    """
    Computes All-Pairs Shortest Paths (APSP) for an unweighted, connected, 
    undirected graph represented by its adjacency matrix A using NumPy.
    """
    n = A.shape[0]
    
    # Base case: Complete graph (all off-diagonal entries are 1)
    if np.all(A + np.eye(n) == 1):
        return A
        
    # Construct B = A^2 or A (off-diagonal only)
    A2 = np.dot(A, A)
    B = ((A == 1) | (A2 >= 1)).astype(int)
    np.fill_diagonal(B, 0)
    
    # Recursive Call on contracted graph
    D_prime = seidel_apsp_numpy(B)
    
    # Compute M = D' * A
    M = np.dot(D_prime, A)
    
    # Compute degree of each vertex (column sum since undirected)
    deg = np.sum(A, axis=0)
    
    # Parity Correction and Reconstruction
    D = np.zeros((n, n), dtype=int)
    for u in range(n):
        for v in range(n):
            if u == v:
                D[u, v] = 0
            else:
                if M[u, v] >= D_prime[u, v] * deg[v]:
                    D[u, v] = 2 * D_prime[u, v]
                else:
                    D[u, v] = 2 * D_prime[u, v] - 1
    return D
```

### 3. Rigorous Constraints of A* Search

A* accelerates the search for a shortest path from a source $s$ to a target $t$ by reweighting edges using an evaluation function $f(u) = d(s, u) + h(u)$, where $h(u)$ is a heuristic function estimating the distance from $u$ to $t$.

#### 3.1 The Heuristic Hierarchy

To guarantee correctness and prevent suboptimal path convergence, the heuristic function $h(u)$ must satisfy strict mathematical properties:

- **Admissibility (קבילות):** $h(u)$ never overestimates the true remaining cost to the target $t$:
    
    $$0 \le h(u) \le \delta(u, t) \quad \forall u \in V$$
    
- **Consistency/Monotonicity (עקביות):** For every edge $(u, v) \in E$, the heuristic satisfies the triangle inequality:
    
    $$h(u) \le w(u, v) + h(v)$$
    

> [!IMPORTANT]
> 
> **The Consistency Theorem:** If a heuristic $h(u)$ is consistent, then running A* search is identical to running Dijkstra's algorithm with a non-negative modified weight function $\hat{w}(u, v) = w(u, v) + h(v) - h(u) \ge 0$. This ensures that once a node is extracted from the priority queue, its path estimate is guaranteed to be optimal, meaning no node ever needs to be re-opened or re-evaluated (`DECREASE-KEY` only occurs for white/gray nodes).

#### 3.2 Bounded Error Heuristics
Heuristics are often classified by how close they are to the true shortest path distance $\delta(u, t)$:
*   **Perfect Potential Heuristic:** If $h(u) = \delta(u, t)$ for all $u \in V$.
    *   *Property:* For any vertex $v$ on the shortest path from $s$ to $t$, the modified path estimate is $\hat{\delta}(s, v) = \delta(s, v) + \delta(v, t) - \delta(s, t) = 0$. For any vertex $u$ not on the shortest path, $\hat{\delta}(s, u) = \delta(s, u) + \delta(u, t) - \delta(s, t) > 0$.
    *   *Effect:* Dijkstra's algorithm will visit *only* the vertices on the shortest path before reaching $t$.
*   **Admissible Heuristic:** If $h(u) \le \delta(u, t)$ for all $u \in V$.
    *   *Property:* Every vertex $v$ on the shortest path will be extracted from the priority queue before any vertex $u$ satisfying $\delta(s, u) + h(u) > \delta(s, t)$.
*   **Bounded Error Heuristic:** If the heuristic error is bounded by a multiplicative factor $\epsilon \ge 1$ such that $h(u) \le \epsilon \cdot \delta(u, t)$ for all $u \in V$.
    *   *Property:* Every vertex $v$ on the shortest path will be extracted from the priority queue before any vertex $u$ satisfying $\delta(s, u) + h(u) > \epsilon \cdot \delta(s, t)$.
    *   *Effect:* This restricts queue extractions to vertices within an $\epsilon$-scaled envelope of the true shortest-path distance.

### 4. Exam-Style Applications & Problem Solving Techniques

#### 4.1 Paradigm A: Validating Potential Fields via Triangle Bounds (2026 Moed A)

> [!IMPORTANT]
> 
> **Exam Target:** In graph reweighting questions, you may be asked to prove whether a given custom function $p(u)$ forms a valid, feasible potential field (satisfying $\hat{w}(u,v) = w(u,v) + p(u) - p(v) \ge 0$). To prove this, check if the function satisfies the triangle inequality across all edge transitions.

#### 4.2 Worked Example: The Multi-Source Anchor Potential Field

Suppose you are given a directed graph $G = (V, E)$ with an arbitrary real-valued edge weight function $w: E \to \mathbb{R}$ and a selected subset of landmark anchor vertices $S \subseteq V$. You are given a precomputed function $p_1(u)$ defined as:

$$p_1(u) = \max_{v_i \in S} \big\{ \delta(u, v_i) - \delta(t, v_i) \big\}$$

Assuming all true shortest-path distances $\delta(\cdot, \cdot)$ are finite and known, prove that $p_1(u)$ is a **feasible potential function** that yields non-negative weights $\hat{w}(u,v) \ge 0$ for all edges in the graph.

##### Step-by-Step Problem Resolution:

1. **State the Feasibility Condition Requirement:**
    
    For a potential function to be feasible, it must satisfy the following inequality for every directed edge $(u, v) \in E$:
    
    $$w(u, v) + p_1(v) - p_1(u) \ge 0 \implies p_1(u) \le w(u, v) + p_1(v)$$
    
2. **Apply the Core Triangle Inequality Invariant:**
    
    By definition, the true shortest-path distance function $\delta$ satisfies the triangle inequality. Therefore, for any edge $(u, v) \in E$ and any anchor landmark vertex $v_i \in S$:
    
    $$\delta(u, v_i) \le w(u, v) + \delta(v, v_i)$$
    
3. **Manipulate the Inequalities:**
    
    Subtract the constant offset value $\delta(t, v_i)$ from both sides of the inequality:
    
    $$\delta(u, v_i) - \delta(t, v_i) \le w(u, v) + \delta(v, v_i) - \delta(t, v_i)$$
    
4. **Take the Maximum Across the Anchor Set:**
    
    Since the inequality holds true for every individual anchor vertex $v_i \in S$, it must also hold true for the maximum value over the entire set $S$:
    
    $$\max_{v_i \in S} \big\{ \delta(u, v_i) - \delta(t, v_i) \big\} \le \max_{v_i \in S} \big\{ w(u, v) + \delta(v, v_i) - \delta(t, v_i) \big\}$$
    
    Since $w(u, v)$ is a scalar value completely independent of the choice of $v_i$, it can be factored out of the maximization expression:
    
    $$\max_{v_i \in S} \big\{ \delta(u, v_i) - \delta(t, v_i) \big\} \le w(u, v) + \max_{v_i \in S} \big\{ \delta(v, v_i) - \delta(t, v_i) \big\}$$
    
5. **Substitute Back the Potential Function Definitions:**
    
    The left-hand side matches the definition of $p_1(u)$, and the right-hand side matches $w(u, v) + p_1(v)$:
    
    $$p_1(u) \le w(u, v) + p_1(v) \implies w(u, v) + p_1(v) - p_1(u) \ge 0$$
    
    This completes the proof, confirming that $p_1(u)$ is a mathematically valid, feasible potential function.
    

### 4.3 Paradigm B: Shortest Path Reductions and Layer Graphs (Homework Reductions)

#### 4.3.1 Minimum Distance from Any Source
*   **Problem:** Given a directed graph $G = (V, E)$ with real-valued edge weights, compute for every vertex $v \in V$ the minimum shortest path distance from *any* vertex in the graph, formally $\delta^*(v) = \min_{u} \delta(u, v)$, in $O(|V| \cdot |E|)$ time.
*   **The Reduction:**
    1.  Create a virtual super-source vertex $s$ (not in $V$).
    2.  Construct an extended graph $G' = (V \cup \{s\}, E \cup \{(s, u) \mid u \in V\})$, where the virtual edges have weight 0.
    3.  Run the **Bellman-Ford** algorithm from $s$ on $G'$ in $O(|V| \cdot |E|)$ time.
*   **Proof of Validity:**
    *   $\delta_{G'}(s, v) \le \delta^*(v)$: Let $u \in V$ minimize the distance to $v$, i.e. $\delta^*(v) = \delta_G(u, v)$. The path $(s, u) \circ P_{u \rightsquigarrow v}$ has weight $0 + \delta_G(u, v) = \delta^*(v)$ in $G'$, so $\delta_{G'}(s, v) \le \delta^*(v)$.
    *   $\delta^*(v) \le \delta_{G'}(s, v)$: Let $P = (s, u, \dots, v)$ be the shortest path from $s$ to $v$ in $G'$. The subpath $P' = u \rightsquigarrow v$ only contains vertices from $V$, so its weight is $\ge \delta_G(u, v) \ge \delta^*(v)$. Since $w(s, u) = 0$, $w(P') = w(P) = \delta_{G'}(s, v) \ge \delta^*(v)$.
    *   Thus, $\delta_{G'}(s, v) = \delta^*(v)$. Since $s$ has no incoming edges, no new cycles are introduced.

#### 4.3.2 Incremental APSP (Adding a Single Node)
*   **Problem:** Given an $n \times n$ All-Pairs Shortest Path distance matrix $D$ for a graph $G = (V, E)$ and a new node $v'$ to be added, along with incoming edge weights $v'_{in}[j]$ (from $j \in V$ to $v'$) and outgoing weights $v'_{out}[j]$ (from $v'$ to $j \in V$). Update the APSP matrix to include $v'$ in $O(n^2)$ time.
*   **The Algorithm:**
    1.  *Compute distances to/from the new node $v'$:*
        *   For each existing vertex $i \in V$, the shortest path from $i$ to $v'$ must end with a direct edge from some intermediate vertex $j$ to $v'$:
            $$D'[i, v'] = \min_{j \in V} \{ D[i, j] + v'_{in}[j] \}$$
        *   For each existing vertex $i \in V$, the shortest path from $v'$ to $i$ must start with a direct edge from $v'$ to some intermediate vertex $j$:
            $$D'[v', i] = \min_{j \in V} \{ v'_{out}[j] + D[j, i] \}$$
        *   Set $D'[v', v'] = 0$.
        *   *Cost:* Computing these $2n$ entries takes $n \cdot O(n) = O(n^2)$ time.
    2.  *Update distances between existing vertices $i, j \in V$:*
        *   Check if the path through the new node $v'$ is shorter:
            $$D'[i, j] = \min \{ D[i, j], \ D'[i, v'] + D'[v', j] \}$$
        *   *Cost:* Takes $O(1)$ time for each of the $n^2$ pairs $\implies O(n^2)$ time.
*   **Total Complexity:** **$O(n^2)$** time.

#### 4.3.3 Multiplicative Shortest Paths
*   **Problem:** Given a directed graph $G = (V, E)$ with edge weights $w(e) > 1$, find the path $P$ from $s$ to $t$ that minimizes the *product* of edge weights $\prod_{e \in P} w(e)$.
*   **The Reduction:**
    1.  Define a new edge weight function $w'(e) = \log_2 w(e)$ for all $e \in E$. Since $w(e) > 1$, we have $w'(e) > 0$.
    2.  Run **Dijkstra's** algorithm from $s$ on $G$ with weight function $w'$ to find the shortest path tree.
    3.  Convert the additive distances back to product distances: $\text{Product-Dist}(s, u) = 2^{\delta_{w'}(s, u)}$.
*   **Proof of Validity:**
    Since $\log_2 (x)$ and $2^x$ are monotonically increasing functions:
    $$\arg\min_P \prod_{e \in P} w(e) = \arg\min_P \log_2\left(\prod_{e \in P} w(e)\right) = \arg\min_P \sum_{e \in P} \log_2 w(e) = \arg\min_P \sum_{e \in P} w'(e)$$
*   **Complexity:** Dominated by Dijkstra's algorithm: **$O(|E| + |V| \log |V|)$** time.

#### 4.3.4 Shortest Path with Even Blue Edges (Layer Graph)
*   **Problem:** In a graph $G = (V, E)$ where edges are colored either red or blue, find the shortest path from $s$ to $t$ containing an **even number** of blue edges.
*   **The Layer Graph Construction:**
    Create a 2-layer graph $G' = (V', E')$:
    1.  *Vertices:* For each vertex $v \in V$, create two copies: $v_0$ (even blue edges) and $v_1$ (odd blue edges).
    2.  *Edges:*
        *   For each red edge $(u, v) \in E$ of weight $w$: Add edges $(u_0, v_0)$ and $(u_1, v_1)$ of weight $w$.
        *   For each blue edge $(u, v) \in E$ of weight $w$: Add edges $(u_0, v_1)$ and $(u_1, v_0)$ of weight $w$.
    3.  Run Dijkstra's algorithm from $s_0$ in $G'$. The shortest path from $s_0$ to $t_0$ is the optimal path.
*   **Complexity:** $G'$ contains $2|V|$ vertices and $2|E|$ edges, so the runtime is **$O(|E| + |V| \log |V|)$**.

#### 4.3.5 Shortest Path with at Most One Edge from $E \setminus F$ (Layer Graph)
*   **Problem:** Given a graph $G = (V, E)$ and a subgraph $H = (V, F)$ where $F \subseteq E$, find the shortest path from $s$ to $t$ that uses **at most one** edge from $E \setminus F$.
*   **The Layer Graph Construction:**
    Create a 2-layer graph $G' = (V', E')$ representing the states "0 edges used from $E \setminus F$" (Layer 1) and "1 edge used from $E \setminus F$" (Layer 2):
    1.  *Vertices:* Create two layers of vertices: $V_1 = \{v_1 \mid v \in V\}$ and $V_2 = \{v_2 \mid v \in V\}$.
    2.  *Edges:*
        *   *Within Layer 1:* Add $(u_1, v_1)$ of weight $w(u, v)$ for each edge $(u, v) \in F$.
        *   *Within Layer 2:* Add $(u_2, v_2)$ of weight $w(u, v)$ for each edge $(u, v) \in F$.
        *   *Transition Edges (from Layer 1 to Layer 2):* Add $(u_1, v_2)$ of weight $w(u, v)$ for each edge $(u, v) \in E \setminus F$.
        *   *Dummy Edge:* Add $(s_1, s_2)$ of weight 0 to allow using 0 edges from $E \setminus F$.
    3.  Run Dijkstra's algorithm from $s_1$. The shortest path to $t_2$ gives the optimal path.
*   **Complexity:** $G'$ contains $2|V|$ vertices and at most $2|E|$ edges, so the runtime is **$O(|E| + |V| \log |V|)$**.

#### 4.3.6 Last Edge Verification
*   **Problem:** Determine if a directed edge $(u, v)$ is the last edge of some shortest path from source $s$ to $v$.
*   **The Condition:**
    The edge $(u, v)$ is the last edge of a shortest path from $s$ to $v$ if and only if:
    $$d(s, u) + w(u, v) == d(s, v)$$
*   **Algorithm:**
    1.  Run Dijkstra's algorithm from $s$ to compute distances $d(s, \cdot)$ for all vertices.
    2.  Check if $d(s, u) + w(u, v) == d(s, v)$. If true, output Yes; else No.
*   **Complexity:** Runs in **$O(|E| + |V| \log |V|)$** time.

#### 4.3.7 Hamiltonian Path & Longest Path in a DAG
*   **Hamiltonian Path in a DAG:**
    1.  Run Topological Sort on the DAG $G$ to get a sequence $v_1, \dots, v_n$.
    2.  Check if $(v_i, v_{i+1}) \in E$ for all $i \in \{1, \dots, n-1\}$. If all edges exist, then $v_1 \to v_2 \to \dots \to v_n$ is a Hamiltonian path (which is unique). If any edge is missing, no Hamiltonian path exists.
    3.  *Complexity:* **$O(|V| + |E|)$** time.
*   **Longest Path (Heaviest Path) in a DAG from $s$ to $t$:**
    1.  Run Topological Sort on the DAG.
    2.  Initialize $dp[s] = 0$ and $dp[v] = -\infty$ for all other vertices.
    3.  Iterate through the vertices in topological order:
        For each vertex $u$, and for each outgoing edge $(u, v)$:
        $$dp[v] = \max(dp[v], \ dp[u] + w(u, v))$$
    4.  $dp[t]$ will contain the weight of the heaviest path.
    5.  *Complexity:* **$O(|V| + |E|)$** time.

#### 4.3.8 Counting Paths in a DAG and Shortest Paths in Weighted Graphs
*   **Counting Paths in a DAG from $s$ to $t$:**
    1.  Run Topological Sort on the DAG.
    2.  Initialize `paths[s] = 1` and `paths[v] = 0` for all other vertices.
    3.  Iterate in topological order from $s$ to $t$:
        For each vertex $u$, and for each outgoing edge $(u, v)$:
        $$\text{paths}[v] = \text{paths}[v] + \text{paths}[u]$$
    4.  Return `paths[t]`.
    5.  *Complexity:* **$O(|V| + |E|)$** time.
*   **Counting Shortest Paths from $s$ to $t$ in Positive-Weighted Graph $G = (V, E)$:**
    1.  Run Dijkstra's algorithm from $s$ to find distances $d(s, \cdot)$ for all vertices.
    2.  Filter the edges to build the Shortest Path DAG $G_{SP} = (V, E_{SP})$:
        $$E_{SP} = \{ (u, v) \in E \mid d(s, u) + w(u, v) == d(s, v) \}$$
    3.  Run the DAG path-counting algorithm from step 1 on $G_{SP}$ to count the paths from $s$ to $t$.
    4.  *Complexity:* **$O(|E| + |V| \log |V|)$** time.

### 5. Advanced Shortest Path Complexity Matrix

This matrix tracks the performance profiles of algebraic graph transformations and heuristic path-finding algorithms.

|**Algorithm Primitive**|**Input Graph Constraints**|**Core Matrix Operations**|**Overall Time Complexity**|**Space Complexity**|
|---|---|---|---|---|
|**Transitive Closure**|General directed graphs|Square-and-Multiply Boolean product sweeps|$O(\|V\|^\omega \log \|V\|)$|$\Theta(\|V\|^2)$|
|**Triangle Detection**|Undirected unweighted layouts|Computing $\text{Trace}(A^3)$ via FMM matrix cubes|$O(\|V\|^\omega)$|$\Theta(\|V\|^2)$|
|**Seidel’s Algorithm**|Connected, undirected, unweighted graphs|Recursive parity corrections with FMM products|$O(\|V\|^\omega \log \|V\|)$|$\Theta(\|V\|^2)$|
|**Consistent A* Search**|Directed graphs, $h(u) \le w(u,v) + h(v)$|Priority Queue extractions (Dijkstra equivalent)|$O(\|E\| \log \|V\|)$|$\Theta(\|V\|)$|

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]