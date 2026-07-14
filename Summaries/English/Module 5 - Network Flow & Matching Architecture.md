---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-18
---
# Module 5 - Network Flow & Matching Architecture

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]
# Algorithms 1: Ultimate Study Guide

## Module 5: Network Flow & Matching Architecture

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering Network Flow networks, residual structures, and structural reductions. Mastery of these concepts is essential for successfully solving maximum flow allocation and min-cut verification sections in past exams.

### 1. Foundational Vocabulary & Flow Invariants

In past exams, you are frequently required to prove structural capacities or verify if a specific edge subset forms a legal cut system. A flow network is a directed graph where edges act as pipes with strict physical bounds.

#### 1.1 Core Mathematical Definitions

Let $G = (V, E)$ be a directed graph with a non-negative capacity function $c: E \to \mathbb{R}^+$. We isolate a unique source vertex $s \in V$ and a unique sink/destination vertex $t \in V$. A formal **Flow** (זרימה) is a real-valued function $f: V \times V \to \mathbb{R}$ that satisfies three foundational conditions:

- **Capacity Constraint (הגבלת הקיבול):** For all $u, v \in V$, the flow tracking cannot exceed the structural threshold:
    
    $$f(u, v) \le c(u, v)$$
    
- **Skew Symmetry (סימטריה אנטי-רפלקסיבית):** For all $u, v \in V$, the directional routing complies with conservation definitions:
    
    $$f(u, v) = -f(v, u)$$
    
- **Flow Conservation (שימור הזרימה):** For all vertices $u \in V \setminus \{s, t\}$, the inbound accumulation matches outbound execution exactly:
    
    $$\sum_{v \in V} f(u, v) = 0$$
    

The total **value** of the flow is defined as $|f| = \sum_{v \in V} f(s, v)$. To construct rigorous proofs for open-ended exam questions, you must master the structural equivalence of the Max-Flow Min-Cut theorem.

*   **Definition of a Cut (חתך):** 
    A **cut** of a graph $G = (V, E)$ is a partition of the vertices $V$ into two disjoint subsets $S$ and $T$ (where $T = V \setminus S$).
*   **Definition of an $(s, t)$-Cut:**
    An **$(s, t)$-cut** is a cut $(S, T)$ where the source vertex $s \in S$ and the sink vertex $t \in T$.
*   **Crossing Edges:**
    An edge $e = (u, v) \in E$ is said to **cross** the cut if it starts in $S$ and ends in $T$ ($u \in S$ and $v \in T$).
*   **Capacity of a Cut:**
    The **capacity** of a cut $(S, T)$, denoted $c(S, T)$, is the sum of the capacities of all edges crossing from $S$ to $T$:
    $$c(S, T) = \sum_{u \in S, v \in T} c(u, v)$$
    *(Crucially, edges pointing backward from $T$ to $S$ do not contribute to the capacity of the cut.)*
*   **Definition of a Minimum $(s, t)$-Cut:**
    A **minimum $(s, t)$-cut** is an $(s, t)$-cut $(S, T)$ whose capacity is the minimum among all possible $(s, t)$-cuts in the graph:
    $$c(S, T) \le c(S', T') \quad \text{for all } (s, t)\text{-cuts } (S', T')$$
    *(Intuitively, it represents the absolute smallest capacity bottleneck that completely disconnects all directed paths from $s$ to $t$).*


*   **Lemma 1: Net Flow Across a Cut**
    *   *Statement:* For any flow $f$ and any $(s, t)$-cut $(S, T)$, the net flow out of $S$ equals the value of the flow:
        $$f(S, T) = \sum_{u \in S, v \in T} f(u, v) - \sum_{v \in T, u \in S} f(v, u) = |f|$$
    *   *Proof:* By definition, $|f| = \sum_{w \in V} f(s, w)$. For any node $u \in S \setminus \{s\}$, flow conservation implies $\sum_{w \in V} f(u, w) = 0$. Summing over all vertices in $S$:
        $$\sum_{u \in S} \sum_{w \in V} f(u, w) = \sum_{w \in V} f(s, w) + \sum_{u \in S \setminus \{s\}} 0 = |f|$$
        We can expand this sum into elements in $S$ and elements in $T$:
        $$\sum_{u \in S} \sum_{w \in V} f(u, w) = \sum_{u \in S} \left( \sum_{v \in S} f(u, v) + \sum_{v \in T} f(u, v) \right)$$
        By skew symmetry, $f(u, v) = -f(v, u)$ for all pairs, which implies $\sum_{u \in S} \sum_{v \in S} f(u, v) = 0$ (all internal flow terms cancel out). Therefore:
        $$|f| = \sum_{u \in S} \sum_{v \in T} f(u, v) = f(S, T) \quad \blacksquare$$

*   **Lemma 2: Weak Duality (Upper Bound Invariant)**
    *   *Statement:* The value of any flow $f$ is upper-bounded by the capacity of any $(s, t)$-cut $(S, T)$:
        $$|f| \le c(S, T)$$
    *   *Proof:* By Lemma 1 and the capacity constraints $f(u, v) \le c(u, v)$:
        $$|f| = \sum_{u \in S, v \in T} f(u, v) - \sum_{v \in T, u \in S} f(v, u)$$
        Since capacities are non-negative, and flow is non-negative on directed edges ($f(v, u) \ge 0$), we have $-f(v, u) \le 0$. Thus:
        $$|f| \le \sum_{u \in S, v \in T} c(u, v) - 0 = c(S, T) \quad \blacksquare$$

*   **Lemma 3: Optimal Cut Saturation (Min-Cut Properties)**
    *   *Statement:* Let $f^*$ be a maximum flow in $G$, and let $S^*$ be the set of vertices reachable from $s$ in the residual graph $G_{f^*}$. Let $T^* = V \setminus S^*$. For any edge $(u, v)$ crossing the cut:
        1. If $u \in S^*$ and $v \in T^*$, then $f^*(u, v) = c(u, v)$ (the edge is fully saturated).
        2. If $u \in T^*$ and $v \in S^*$, then $f^*(u, v) = 0$ (zero backward flow).
    *   *Proof:*
        1. If $f^*(u, v) < c(u, v)$ for some $u \in S^*, v \in T^*$, then the residual capacity is positive: $c_{f^*}(u, v) = c(u, v) - f^*(u, v) > 0$. This implies a directed edge $(u, v) \in G_{f^*}$. Since $u \in S^*$, $u$ is reachable from $s$, meaning $v$ must also be reachable from $s$, placing $v \in S^*$ (contradiction).
        2. If $f^*(v, u) > 0$ for some $v \in T^*, u \in S^*$, then the residual capacity of the reverse edge is positive: $c_{f^*}(u, v) = c(u, v) - f^*(u, v) + f^*(v, u) > 0$. This implies $(u, v) \in G_{f^*}$, making $v$ reachable from $s$ and placing $v \in S^*$ (contradiction).
        3. Therefore, all forward edges are fully saturated, and all backward edges have zero flow:
           $$|f^*| = f^*(S^*, T^*) = \sum_{u \in S^*, v \in T^*} f^*(u, v) - \sum_{v \in T^*, u \in S^*} f^*(v, u) = \sum_{u \in S^*, v \in T^*} c(u, v) - 0 = c(S^*, T^*) \quad \blacksquare$$

> [!IMPORTANT]
> 
> **The Three Structural Equivalences:** For any flow function $f$, the following three statements are completely identical:
> 
> 1. $f$ is a maximum flow in the network $G$.
>     
> 2. The residual network $G_f$ contains absolutely **no augmenting paths** from $s$ to $t$.
>     
> 3. The total scalar value of the flow matches the capacity of some cut boundary: $|f| = c(S, T)$. Thus, the maximum flow value exactly equals the minimum cut capacity.
>     

### 2. Core Flow Algorithms

#### 2.1 The Ford-Fulkerson Method & Residual Networks

- **The Residual Primitive:** For a given flow $f$, the residual capacity $c_f(u, v)$ tracks how much additional net flow can be routed from $u$ to $v$:
    $$c_f(u, v) = c(u, v) - f(u, v)$$
- **Operational Strategy:** An iterative improvement scheme. As long as a simple directed path $p: s \leadsto t$ exists in the residual graph $G_f$ where $c_f(e) > 0$ for all $e \in p$, we push the bottleneck capacity $c_f(p) = \min_{e \in p} c_f(e)$ along the path, updating the flow values.

```plaintext
FORD-FULKERSON(G, s, t, c):
    for each edge (u, v) in E:
        f(u, v) = 0
        f(v, u) = 0
    while there exists a path p from s to t in G_f:
        c_f(p) = min({c_f(u, v) : (u, v) in p})
        for each edge (u, v) in p:
            f(u, v) = f(u, v) + c_f(p)
            f(v, u) = -f(u, v)
```

#### 2.2 The Edmonds-Karp Algorithm (אלגוריתם אדמונדס-קארפ)

- **Operational Strategy:** A deterministic implementation of Ford-Fulkerson. It explicitly mandates that the augmenting path chosen in $G_f$ must be a **shortest path** in terms of the number of edges (unweighted distance), discovered by running a standard **Breadth-First Search (BFS)** on $G_f$.

```plaintext
EDMONDS-KARP(G, s, t, c):
    for each edge (u, v) in E:
        f(u, v) = 0
        f(v, u) = 0
    while True:
        p = BFS-FIND-SHORTEST-PATH(G_f, s, t)
        if p == NIL:
            break
        c_f(p) = min({c_f(u, v) : (u, v) in p})
        for each edge (u, v) in p:
            f(u, v) = f(u, v) + c_f(p)
            f(v, u) = -f(u, v)
```

##### Algorithmic Complexity Matrix

|**Algorithm Primitive**|**Edge Capacity Boundary Constraints**|**Time Complexity Limit**|**Space Complexity Baseline**|
|---|---|---|---|
|**Standard Ford-Fulkerson**|Requires integer capacities $c(e) \in \mathbb{Z}^+$.|$O(\|E\| \cdot \|f^*\|)$|$\Theta(\|V\| + \|E\|)$|
|**Edmonds-Karp Algorithm**|Works on arbitrary real-valued capacities.|$\mathbf{O(\|V\| \cdot \|E\|^2)}$|$\Theta(\|V\| + \|E\|)$|
|**Dinic's Algorithm**|Works on arbitrary real-valued capacities.|$\mathbf{O(\|V\|^2 \cdot \|E\|)}$|$\Theta(\|V\| + \|E\|)$|

#### 2.3 Dinic's Algorithm (אלגוריתם דיניץ)
    
- **Operational Strategy:** Instead of finding a single augmenting path per BFS, Dinic's algorithm finds multiple augmenting paths simultaneously in each phase by routing a **blocking flow** in a structured **level graph**.
- **Level Graph ($L_f$):** A subgraph of the residual network $G_f$ where vertices are grouped into layers based on their unweighted shortest distance from $s$. An edge $(u, v)$ is included in $L_f$ if and only if $\text{Level}(v) == \text{Level}(u) + 1$.

```plaintext
DINIC(G, s, t, c):
    for each edge (u, v) in E:
        f(u, v) = 0
        f(v, u) = 0
    while BFS-CONSTRUCT-LEVEL-GRAPH(G_f, s, t) is True:
        while True:
            pushed = DFS-FIND-BLOCKING-PATH(s, t, inf)
            if pushed == 0:
                break
    return f

DFS-FIND-BLOCKING-PATH(u, t, flow):
    if u == t or flow == 0:
        return flow
    for each v in Adj[u]:
        if Level[v] == Level[u] + 1 and c_f(u, v) > 0:
            pushed = DFS-FIND-BLOCKING-PATH(v, t, min(flow, c_f(u, v)))
            if pushed > 0:
                f(u, v) = f(u, v) + pushed
                f(v, u) = -f(u, v)
                return pushed
    return 0
```
*   **Structural Invariant:** There are at most $|V| - 1$ total phases. Finding a blocking flow using DFS takes $O(|V| \cdot |E|)$ time per phase, resulting in a total complexity of **$O(|V|^2 |E|)$** time.

#### 2.4 Capacity Scaling Method

- **Operational Strategy:** A variation of Ford-Fulkerson that prioritizes paths with large residual capacities by scaling a threshold parameter $\Delta$.

```plaintext
CAPACITY-SCALING(G, s, t, c):
    for each edge (u, v) in E:
        f(u, v) = 0
        f(v, u) = 0
    C = max({c(u, v) : (u, v) in E})
    Delta = 2^(floor(log2(C)))
    while Delta >= 1:
        while there exists an augmenting path p in G_f where c_f(e) >= Delta for all e in p:
            c_f(p) = min({c_f(u, v) : (u, v) in p})
            for each edge (u, v) in p:
                f(u, v) = f(u, v) + c_f(p)
                f(v, u) = -f(u, v)
        Delta = Delta / 2
```


### 3. Exam-Style Applications & Problem Solving Techniques

#### 3.1 Paradigm A: Vertex-Induced Capacity Enforcement (2025 Moed B / Moed C)

> [!IMPORTANT]
> 
> **Exam Target:** In past papers, standard networks limit the flow crossing over edge bounds. If a question introduces a bottleneck limit on the **vertices themselves**—stating that a node $v$ cannot process more than $l(v)$ total incoming flow units—solve this by **splitting each vertex into an operational pair**.

#### 3.2 Worked Example: Vertex-Constrained Network Allocation

Suppose you are given a directed network $G = (V, E)$ with non-negative edge capacities $c(e)$. Additionally, each vertex $v \in V$ has an interior capacity limit $l(v)$ that restricts the total amount of flow passing through it. Design an algorithm running in $O(|V| \cdot |E|^2)$ time that computes the maximum valid flow from $s$ to $t$ under both constraints.

##### Step-by-Step Problem Resolution:

1. **Construct a Node-Transformed Split Graph ($G^*$):**
    
    To enforce vertex capacity constraints using standard edge-based algorithms, we can split each original vertex $v \in V \setminus \{s, t\}$ into two distinct companion nodes linked by a directed capacity-enforcing edge:
    
    $$v \implies (v_{\text{in}}, v_{\text{out}})$$
    
    For the source and sink nodes, we maintain them without interior constraints by setting $s_{\text{in}} = s_{\text{out}}$ and $t_{\text{in}} = t_{\text{out}}$.
    
2. **Map the Transformed Edge Set ($E^*$):**
    
    - **Interior Vertex Edges:** For each split node, add an internal matching edge from the input terminal to the output terminal with a capacity equal to the node's constraint:
        
        $$e_{\text{internal}} = (v_{\text{in}}, v_{\text{out}}) \quad \text{with } c^*(v_{\text{in}}, v_{\text{out}}) = l(v)$$
        
    - **Exterior Routing Edges:** For every original directed edge $e = (u, v) \in E$, redirect its endpoints to span across the split vertex boundaries. The edge must leave the outbound terminal of $u$ and enter the inbound terminal of $v$:
        
        $$e^* = (u_{\text{out}}, v_{\text{in}}) \quad \text{with } c^*(u_{\text{out}}, v_{\text{in}}) = c(u, v)$$
        
3. **Execute the Edmonds-Karp Primitives:**
    
    Run the standard Edmonds-Karp maximum flow algorithm on the newly constructed edge graph $G^* = (V^*, E^*)$, using $s_{\text{out}}$ as the source and $t_{\text{in}}$ as the sink.
    
4. **Prove Correctness via Structural Cuts:**
    
    Any path from $s$ to $t$ must cross the internal structural edge $(v_{\text{in}}, v_{\text{out}})$. Since $c^*(v_{\text{in}}, v_{\text{out}}) = l(v)$, the maximum flow through node $v$ is bounded by $l(v)$, satisfying the constraint.
    
5. **Complexity Analysis Verification:**
    
    - $|V^*| = 2|V| = O(|V|)$ vertices.
        
    - $|E^*| = |E| + |V| = O(|E|)$ edges (since $|E| \ge |V| - 1$ in a connected network).
        
    - Running Edmonds-Karp takes $O(|V^*| \cdot |E^*|^2) = \mathbf{O(\|V\| \cdot \|E\|^2)}$ time, matching the exam target runtime bound.
        

#### 3.3 Paradigm B: Uniqueness Verification & Cut Crossing Classification (2024 Moed B)

> [!NOTE]
> 
> **Exam Target:** If asked to verify whether a specific subset of critical structural edges $F \subseteq E$ crosses **every single minimum cut** of a network, do not enumerate cuts. Instead, leverage the property of the residual network $G_f$ after computing the max flow to see if those edges act as absolute bottlenecks.

#### 3.4 Worked Example: Verifying Global Cut Intersections

Given a directed network $G = (V, E)$ with integer capacities, a source $s$, a sink $t$, and a target edge subset $F \subseteq E$. Design an algorithm that determines whether **every single minimum $(s,t)$ cut** $(S, T)$ in $G$ satisfies the property that $F \subseteq (S \times T)$ (meaning all edges in $F$ cross from $S$ to $T$). The algorithm must run in the time required for a single maximum flow execution.

##### Step-by-Step Problem Resolution:

1. **Compute Max Flow to Isolate Bottlenecks:**
    
    Run the Edmonds-Karp algorithm to find a maximum flow $f^*$ on $G$, and construct the final residual network $G_{f^*}$.
    
2. **Isolate Reachability Bound Sets:**
    
    - Find the set $S^*$ of all vertices reachable from $s$ in the residual graph $G_{f^*}$ using a search traversal (such as BFS or DFS).
        
    - Find the set $T^*$ of all vertices that can reach $t$ in the residual graph $G_{f^*}$ by running a search backward from $t$ along transposed residual edges.
        
3. **Formulate the Decision Rule:**
    
    An edge $e = (u, v)$ belongs to _every_ minimum cut if and only if it is completely saturated, its capacity is fully utilized, and it spans from the maximum possible reachability boundary of $s$ to the minimum possible reachability boundary of $t$.
    
    - Check each edge $e = (u, v) \in F$:
        
        - Verify if $u \in S^*$ and $v \in T^*$.
            
    - If _every_ edge in $F$ satisfies this condition, output `TRUE` (they cross every minimum cut). If even a single edge in $F$ fails this tracking test, output `FALSE`.
        
4. **Complexity Analysis Verification:**
    
    - Computing the maximum flow takes $O(|V| \cdot |E|^2)$ time.
        
    - Isolating the reachability sets $S^*$ and $T^*$ takes $O(|V| + |E|)$ time.
        
    - Checking the containment coordinates of the edges in $F$ takes $O(|F|) = O(|E|)$ time.
        
    - Total runtime: $\mathbf{O(\|V\| \cdot \|E\|^2)}$, matching the performance constraint of a single max-flow execution.

##### 3.4.1 Python Implementation

```python
from collections import defaultdict, deque

class FlowNetwork:
    def __init__(self, num_vertices):
        self.n = num_vertices
        self.adj = defaultdict(list)
        self.capacity = {}
        self.flow = {}

    def add_edge(self, u, v, cap):
        """Adds a directed edge with a given capacity."""
        self.adj[u].append(v)
        self.adj[v].append(u)  # Add residual edge representation
        self.capacity[(u, v)] = cap
        self.capacity[(v, u)] = self.capacity.get((v, u), 0)
        self.flow[(u, v)] = 0
        self.flow[(v, u)] = 0

    def edmonds_karp(self, s, t):
        """Computes the maximum flow using the Edmonds-Karp algorithm."""
        max_flow = 0
        while True:
            # BFS to find the shortest augmenting path in the residual graph
            parent = {s: None}
            queue = deque([s])
            while queue:
                curr = queue.popleft()
                if curr == t:
                    break
                for neighbor in self.adj[curr]:
                    residual_cap = self.capacity[(curr, neighbor)] - self.flow[(curr, neighbor)]
                    if residual_cap > 0 and neighbor not in parent:
                        parent[neighbor] = curr
                        queue.append(neighbor)
            
            if t not in parent:
                break  # No more augmenting paths
            
            # Determine the bottleneck capacity along the path
            bottleneck = float('inf')
            curr = t
            while curr != s:
                p = parent[curr]
                res_cap = self.capacity[(p, curr)] - self.flow[(p, curr)]
                bottleneck = min(bottleneck, res_cap)
                curr = p
            
            # Augment the flow along the path
            curr = t
            while curr != s:
                p = parent[curr]
                self.flow[(p, curr)] += bottleneck
                self.flow[(curr, p)] -= bottleneck
                curr = p
            
            max_flow += bottleneck
        return max_flow

    def get_residual_graph(self):
        """Constructs the adjacency list of the residual network G_f with positive capacities."""
        res_adj = defaultdict(list)
        for (u, v), cap in self.capacity.items():
            res_cap = cap - self.flow[(u, v)]
            if res_cap > 0:
                res_adj[u].append(v)
        return res_adj


def verify_global_cut_intersections(network, s, t, F):
    """
    Verifies whether every edge in F crosses every single minimum (s, t)-cut.
    Runs in the time required for a single maximum flow execution (O(|V| * |E|^2)).
    """
    # 1. Compute Max Flow
    network.edmonds_karp(s, t)
    
    # 2. Get residual graph G_f
    res_adj = network.get_residual_graph()
    
    # 3. Find S* (all vertices reachable from s in G_f)
    S_star = set()
    queue = deque([s])
    S_star.add(s)
    while queue:
        u = queue.popleft()
        for v in res_adj[u]:
            if v not in S_star:
                S_star.add(v)
                queue.append(v)
                
    # 4. Find T* (all vertices that can reach t in G_f)
    # We run a backward search from t along incoming residual edges
    rev_res_adj = defaultdict(list)
    for u in res_adj:
        for v in res_adj[u]:
            rev_res_adj[v].append(u)
            
    T_star = set()
    queue = deque([t])
    T_star.add(t)
    while queue:
        u = queue.popleft()
        for v in rev_res_adj[u]:
            if v not in T_star:
                T_star.add(v)
                queue.append(v)
                
    # 5. Apply the Decision Rule:
    # For every edge (u, v) in F, verify if u in S* and v in T*
    for u, v in F:
        if u not in S_star or v not in T_star:
            return False
    return True
```


### 4. Comprehensive Structural Reductions (Bipartite Matching)

Network Flow is a powerful tool for solving complex combinatorial optimization problems on unweighted graphs through structural reductions.

#### 4.1 Maximum Bipartite Matching Reduction

Given an undirected bipartite graph $G = (L \cup R, E)$, where edges only connect a vertex in $L$ to a vertex in $R$. We can find a maximum matching (a subset of edges with no shared vertices) by converting it into a maximum flow problem:

```
                      Bipartite Matching Network Reduction
                                      
                         ┌───▶ l1 ───► r1 ───┐
                         │    │       ▲      │
                         │    └───┐   │      │
                (s) ─────┼───▶ l2 ┼───┘      ┼────▶ (t)
                         │        ▼          │
                         └───▶ l3 ───► r2 ───┘
                         
     [All edges: Capacity = 1. Left to Right Directed Graph Architecture]
```

##### Reduction Steps:

1. Create a directed flow network $G' = (V \cup \{s, t\}, E')$.
    
2. Add a directed edge $(s, l)$ for every vertex $l \in L$, with a capacity $c(s, l) = 1$.
    
3. Add a directed edge $(r, t)$ for every vertex $r \in R$, with a capacity $c(r, t) = 1$.
    
4. Direct all original undirected edges $e \in E$ from left to right: $(l, r)$, with a capacity $c(l, r) = 1$.
    
5. **The Integrity Theorem:** Run a max-flow algorithm with integer capacities. The resulting max-flow value $|f^*|$ exactly equals the size of the maximum bipartite matching, and the edges between $L$ and $R$ carrying an integer flow of $1$ form the matching.



### 5. Advanced Flow Engineering (Layered Residual Networks & Dinic's)

Standard Edmonds-Karp scans a single augmenting path per BFS traversal. Advanced flow engineering uses structural stratification to push multiple paths simultaneously through the residual network.

#### 5.1 Layered Networks & Blocking Flows

Given a directed flow network $G = (V, E)$ and a valid flow $f$, we construct a specialized **Layered Network** (רשת שכבות) $L = (V_L, E_L)$ within the residual graph $G_f$:

- Vertices are organized into strict horizontal layers based on their unweighted shortest-path distance from the source $s$ in $G_f$:
    
    $$\text{Level}(u) = \delta_{G_f}(s, u)$$
    
- An edge $e = (u, v) \in E_f$ is kept in $E_L$ if and only if it leads directly to the next consecutive layer:
    
    $$\text{Level}(v) == \text{Level}(u) + 1$$
    

A **Blocking Flow** (זרימה חוסמת) is a flow configuration within the Layered Network $L$ such that _every_ directed path from $s$ to $t$ contains at least one completely saturated edge.

> [!CAUTION]
> 
> **Exam Pitfall:** Do not confuse a _Blocking Flow_ with a _Maximum Flow_. A blocking flow guarantees that you cannot push more flow along the currently established shortest paths without shifting levels. However, it does _not_ mean the overall flow value is maximized globally, because longer, winding augmenting paths might still exist in parts of the residual network that bypass the current layered restrictions.

#### 5.2 Dinic's Algorithm (אלגוריתם דיניץ)

- **Operational Strategy:** Divides execution into discrete _Phases_. Each phase builds a Layered Network $L$ on top of $G_f$ via BFS, finds a Blocking Flow within $L$ using Depth-First Search (DFS) with backtracking, updates the master flow field, and repeats.
    
- **The Structural Phase Invariant:** Every consecutive phase strictly increases the shortest-path distance from the source to the sink in the residual network:
    
    $$\delta_{G_{f_{\text{new}}}}(s, t) > \delta_{G_{f_{\text{old}}}}(s, t)$$
    
    This guarantees that Dinic's algorithm terminates after at most $|V| - 1$ total phases.
    

##### Complexity Matrix
- Finding a single blocking flow via DFS takes $O(|V| \cdot |E|)$ time.
    
- There are at most $|V| - 1$ phases $\implies \mathbf{O(\|V\|^2 \cdot \|E\|)}$ total time.
    
### 6. The Hopcroft-Karp Bipartite Matching Algorithm

The **Hopcroft-Karp algorithm** is an optimized bipartite matching algorithm that implements Dinic’s layered network optimization primitives directly on unweighted bipartite graphs, bypassing the overhead of constructing a full flow network.

#### 6.1 Foundational Matching Definitions
Let $G = (L \cup R, E)$ be a bipartite graph where $L$ and $R$ are disjoint vertex sets, and let $M \subseteq E$ be a valid matching (no two edges in $M$ share an endpoint).
*   **Alternating Path (מסלול מתחלף):** A simple path in $G$ whose edges alternate between being inside $M$ and outside $M$ ($E \setminus M$).
*   **Augmenting Path (מסלול משפר):** An alternating path whose start and end vertices are both **unmatched (free)** in $M$.
*   **Berge's Lemma:** A matching $M$ in $G$ is maximum if and only if there are no augmenting paths in $G$ relative to $M$.
    *   *Proof:* If an augmenting path $P$ exists, we can toggle the status of all edges along $P$ (augmenting the matching: $M' = M \oplus P$). Since $P$ starts and ends with edges outside $M$, it has exactly one more edge outside $M$ than inside $M$. Toggling increases the matching size by 1.

#### 6.2 The Hopcroft-Karp Phase Strategy
Hopcroft-Karp operates in discrete **Phases**. In each phase, it finds a **maximal set of vertex-disjoint shortest augmenting paths** and augments them simultaneously. Each phase consists of two stages:
1.  **BFS Layer Construction (`BFS-HK`):** 
    *   Start a multi-source BFS from all currently unmatched vertices in $L$ (setting their distance to 0).
    *   Traverse edges outside $M$ from $L$ to $R$, and edges inside $M$ from $R$ to $L$.
    *   The BFS terminates at the first layer in $R$ containing at least one unmatched vertex. We define this shortest augmenting path length as $D[\text{NIL}]$.
2.  **DFS Augmentation (`DFS-HK`):** 
    *   Run a DFS from all unmatched vertices in $L$.
    *   Travel strictly along the DAG levels established by the BFS ($D[v] = D[u] + 1$).
    *   When an augmenting path is found, toggle the matching edges. To enforce that augmenting paths are vertex-disjoint, vertices visited by the DFS are blocked (their distance is set to $\infty$) so they are not reused in the current phase.

```plaintext
HOPCROFT-KARP(G):
    for each vertex u in L:
        Pair_L[u] = NIL
    for each vertex v in R:
        Pair_R[v] = NIL
    matching = 0
    while BFS-HK() is True:
        for each vertex u in L:
            if Pair_L[u] == NIL:
                if DFS-HK(u) is True:
                    matching = matching + 1
    return matching

BFS-HK():
    Q = empty_queue()
    for each u in L:
        if Pair_L[u] == NIL:
            Dist[u] = 0
            ENQUEUE(Q, u)
        else:
            Dist[u] = inf
    Dist[NIL] = inf
    while Q != ∅:
        u = DEQUEUE(Q)
        if Dist[u] < Dist[NIL]:
            for each v in Adj[u]:
                if Dist[Pair_R[v]] == inf:
                    Dist[Pair_R[v]] = Dist[u] + 1
                    ENQUEUE(Q, Pair_R[v])
    return Dist[NIL] != inf

DFS-HK(u):
    if u != NIL:
        for each v in Adj[u]:
            if Dist[Pair_R[v]] == Dist[u] + 1:
                if DFS-HK(Pair_R[v]) is True:
                    Pair_R[v] = u
                    Pair_L[u] = v
                    return True
        Dist[u] = inf
        return False
    return True
```

#### 6.3 Proof of the $2\sqrt{|V|}$ Phase Bound
*   **Theorem:** The Hopcroft-Karp algorithm terminates after at most $2\sqrt{|V|}$ phases.
*   **Proof:**
    1.  *Strict Increase Invariant:* The length of the shortest augmenting path strictly increases after each phase. Let $k$ be the number of completed phases. After $k$ phases, the shortest augmenting path must have length at least $k$.
    2.  Let $M$ be the matching obtained after $k = \lceil \sqrt{|V|} \rceil$ phases. Let $M^*$ be a maximum matching.
    3.  Consider the symmetric difference $M \oplus M^*$. It consists of vertex-disjoint paths and cycles. The cycles must have even length and alternate between $M$ and $M^*$. The paths must alternate between $M$ and $M^*$.
    4.  Specifically, there are exactly $|M^*| - |M|$ augmenting paths in $M \oplus M^*$ relative to $M$.
    5.  Since $M$ was obtained after $k = \lceil \sqrt{|V|} \rceil$ phases, every augmenting path relative to $M$ must have length at least $\sqrt{|V|}$.
    6.  Since these $|M^*| - |M|$ paths are vertex-disjoint, the total number of vertices in them cannot exceed the total vertex count $|V|$:
        $$(|M^*| - |M|) \cdot \sqrt{|V|} \le |V| \implies |M^*| - |M| \le \sqrt{|V|}$$
    7.  Since each subsequent phase increases the matching size by at least 1, we require at most $|M^*| - |M| \le \sqrt{|V|}$ additional phases to reach the maximum matching.
    8.  Total phases $\le \sqrt{|V|} + \sqrt{|V|} = 2\sqrt{|V|}$. $\blacksquare$

*   **Time Complexity:** Since BFS and DFS take $O(|V| + |E|)$ time per phase, the total running time is **$\mathbf{O(|E| \sqrt{|V|})}$**.

#### 6.4 Python Implementation

```python
from collections import defaultdict, deque

class BipartiteMatchingHopcroftKarp:
    def __init__(self, left_vertices, right_vertices):
        self.L = left_vertices
        self.R = right_vertices
        self.adj = defaultdict(list)  # Edges directed from L to R
        self.pair_L = {u: None for u in left_vertices}
        self.pair_R = {v: None for v in right_vertices}
        self.dist = {}

    def add_edge(self, u, v):
        """Adds a bipartite edge from left vertex u to right vertex v."""
        self.adj[u].append(v)

    def bfs(self):
        """Multi-source BFS to build the level graph layers."""
        queue = deque()
        for u in self.L:
            if self.pair_L[u] is None:
                self.dist[u] = 0
                queue.append(u)
            else:
                self.dist[u] = float('inf')
        
        self.dist[None] = float('inf')
        
        while queue:
            u = queue.popleft()
            if self.dist[u] < self.dist[None]:
                for v in self.adj[u]:
                    neighbor = self.pair_R[v]
                    if neighbor is None:
                        # Augmenting path found (reaches unmatched NIL)
                        if self.dist[None] == float('inf'):
                            self.dist[None] = self.dist[u] + 1
                    elif self.dist[neighbor] == float('inf'):
                        self.dist[neighbor] = self.dist[u] + 1
                        queue.append(neighbor)
                        
        return self.dist[None] != float('inf')

    def dfs(self, u):
        """DFS to find vertex-disjoint augmenting paths in the level graph."""
        if u is not None:
            for v in self.adj[u]:
                neighbor = self.pair_R[v]
                # Travel strictly along the BFS levels
                if neighbor is None or self.dist[neighbor] == self.dist[u] + 1:
                    if self.dfs(neighbor):
                        self.pair_R[v] = u
                        self.pair_L[u] = v
                        return True
            self.dist[u] = float('inf')  # Block this vertex from reuse
            return False
        return True

    def max_matching(self):
        """Computes the maximum cardinality bipartite matching in O(|E| * sqrt(|V|)) time."""
        matching_count = 0
        while self.bfs():
            for u in self.L:
                if self.pair_L[u] is None:
                    if self.dfs(u):
                        matching_count += 1
        return matching_count


# --- Verification Test Case ---
if __name__ == "__main__":
    # Bipartite graph:
    # L = {1, 2, 3}, R = {A, B, C}
    # Edges: 1-A, 1-B, 2-B, 2-C, 3-B
    matcher = BipartiteMatchingHopcroftKarp([1, 2, 3], ['A', 'B', 'C'])
    matcher.add_edge(1, 'A')
    matcher.add_edge(1, 'B')
    matcher.add_edge(2, 'B')
    matcher.add_edge(2, 'C')
    matcher.add_edge(3, 'B')

    print("Max Matching Size:", matcher.max_matching())  # Should output 3 (e.g. 1-A, 3-B, 2-C)
    print("Pairs in L:", matcher.pair_L)
    print("Pairs in R:", matcher.pair_R)
```

### 7. Matrix Factorizations: Birkhoff-von Neumann Decomposition

This framework maps continuous matrix weights directly to deterministic matching permutations.

#### 7.1 Doubly Stochastic Networks

A square matrix $M \in \mathbb{R}^{n \times n}$ with non-negative entries ($M_{ij} \ge 0$) is **Doubly Stochastic** (מטריצה דו-סטוכסטית) if and only if the sum of the elements in every row and every column equals exactly 1:

$$\sum_{j=1}^n M_{ij} = 1 \quad \forall i \quad \text{and} \quad \sum_{i=1}^n M_{ij} = 1 \quad \forall j$$

#### 7.2 The Decomposition Theorem

The **Birkhoff-von Neumann Theorem** states that any doubly stochastic matrix $M$ can be represented as a convex combination of a finite number of **Permutation Matrices** $P_1, P_2, \dots, P_k$:

$$M = \sum_{z=1}^k \theta_z P_z \quad \text{where } \theta_z \ge 0 \quad \text{and } \sum_{z=1}^k \theta_z = 1$$

_(Note: A Permutation Matrix is a binary matrix containing exactly one `1` in each row and column)._

##### Algorithmic Extraction Flow via Flow Reductions

1. Represent $M$ as a weighted bipartite graph $G = (L \cup R, E)$ where an edge $(l_i, r_j)$ exists with weight $M_{ij} > 0$.
    
2. Because row and column sums equal 1, Hall's Marriage Theorem guarantees the existence of a perfect matching. Find a perfect matching using max-flow sweeps.
    
3. Isolate the corresponding permutation matrix $P_1$ mapping those matching lines. Let $\theta_1 = \min \{ M_{ij} \mid P_{1}[i,j] == 1 \}$.
    
4. Update your remaining matrix: $M \leftarrow M - \theta_1 P_1$. Scale the matrix back to be doubly stochastic and loop until $M$ collapses to all zeros.
    

### 8. Advanced Exam-Style Practice: Smallest Edge-Count Minimum Cut (2025 Moed A)

#### 8.1 Problem Statement

Suppose you are given a directed flow network $G = (V, E)$ with strictly integer capacities $c(e) \in \mathbb{Z}^+$. There may be multiple distinct cuts that all achieve the identical maximum flow value $|f^*|$. Design an algorithm that finds a valid minimum cut $(S, T)$ that contains the **absolute minimum number of crossing edges**, running within the time complexity of a single Dinic execution.

#### 8.2 Step-by-Step Resolution

1. **Apply an Asymmetric Scaling Transformation to Capacities:**
    
    To force the algorithm to prioritize cutting fewer edges, we scale the original capacity function up by a massive factor and penalize each edge by adding a small uniform cost. Let $|E|$ be the total edge count. Define a new capacity function $c^*(e)$ for every edge $e \in E$:
    
    $$c^*(e) = |E| \cdot c(e) + 1$$
    
2. **Execute Max-Flow on the Transformed Network:**
    
    Run Dinic's algorithm on the modified network $G^* = (V, E, c^*)$ to compute the new maximum flow value $|f^*|^*$.
    
3. **Extract the Targeted Minimum Cut Partition:**
    
    Construct the final residual network $G^*_{f^*}$. Run a graph search (such as BFS or DFS) starting from the source vertex $s$, traveling only along edges with a residual capacity $c^*_{f^*}(u, v) > 0$.
    
    - Let $S$ be the set of all vertices reached during this traversal.
        
    - Let $T = V \setminus S$ be the remaining unreached vertices.
        
    - Return the cut partition $(S, T)$.
        
4. **Prove Correctness via Algebraic Expansion:**
    
    The capacity of any cut $(S, T)$ in the transformed network $G^*$ expands linearly according to the formula:
    
    $$c^*(S, T) = \sum_{e \in (S \times T)} \big(|E| \cdot c(e) + 1\big) = |E| \cdot \sum_{e \in (S \times T)} c(e) + \sum_{e \in (S \times T)} 1 = |E| \cdot c(S, T) + |(S \times T)|$$
    
    Let $|(S \times T)| = k$ be the number of crossing edges (where $0 \le k \le |E|$). We show why the massive scaling factor $|E|$ guarantees that we minimize original capacity first, and break ties by edge count:
    
    *   **Case 1: Different Original Capacities ($c(A) < c(B)$)**
        If Cut A has a strictly smaller original capacity than Cut B, then since capacities are integers:
        $$c(B) \ge c(A) + 1$$
        Let's compare the transformed capacities $c^*(A)$ and $c^*(B)$ in the absolute worst-case scenario (where $A$ crosses the maximum possible number of edges $k_A = |E|$, and $B$ crosses the minimum $k_B = 0$):
        $$c^*(A) \le |E| \cdot c(A) + |E| = |E| \cdot (c(A) + 1)$$
        $$c^*(B) \ge |E| \cdot c(B) \ge |E| \cdot (c(A) + 1)$$
        Thus, $c^*(A) \le c^*(B)$ is guaranteed. The edge-count penalty can never dominate or corrupt the primary capacity comparison, ensuring the algorithm only selects from valid original minimum cuts.
        
    *   **Case 2: Equal Original Capacities ($c(A) == c(B) = C$)**
        If two cuts tie on their original capacity (i.e. both are minimum cuts), the scaled capacity terms are identical:
        $$c^*(A) < c^*(B) \iff |E| \cdot C + k_A < |E| \cdot C + k_B \iff k_A < k_B$$
        Therefore, among all cuts that achieve the minimum original capacity, the algorithm breaks the tie by selecting the cut that strictly minimizes the number of crossing edges $k$.

        
5. **Complexity Analysis Verification:**
    
    - Modifying the edge capacity array takes $O(|E|)$ time.
        
    - Since the original capacities are integers, the scaled capacities remain integers. Running Dinic's algorithm takes $O(|V|^2 \cdot |E|)$ time.
        
    - Extracting the reachable set $S$ via BFS takes $O(|V| + |E|)$ time.
        
    - Total runtime: $\mathbf{O(\|V\|^2 \cdot \|E\|)}$, meeting the performance constraint perfectly.
        

#### 8.3 Unique Minimum Cut Verification
*   **Problem:** Given a flow network $G = (V, E)$ with capacities $c(e)$ and source $s$, sink $t$, determine if the minimum $(s, t)$-cut is **unique**.
*   **The Algorithm:**
    1. Run a max-flow algorithm to find a maximum flow $f$.
    2. Construct the residual network $G_f$.
    3. Compute $S_f = \{ u \in V \mid s \rightsquigarrow u \text{ in } G_f \}$, the set of all vertices reachable from $s$ in $G_f$ (this represents the *minimum* $S$-cut containing the fewest vertices).
    4. Compute $T_f = \{ u \in V \mid u \rightsquigarrow t \text{ in } G_f \}$, the set of all vertices that can reach $t$ in $G_f$ (this represents the *minimum* $T$-cut containing the fewest vertices on the sink side, which is equivalent to the maximum $S$-cut $V \setminus T_f$).
    5. The minimum cut is unique if and only if $S_f \cup T_f = V$.
*   **Proof of Correctness:**
    *   Let $(S, T)$ be any minimum $(s, t)$-cut. For any $u \in S$ and $v \in T$, we must have $f(u, v) = c(u, v)$ and $f(v, u) = 0$, meaning $c_f(u, v) = 0$ in the residual network $G_f$.
    *   This implies that in the residual network $G_f$, there are no edges leaving $S$ to $T$. Thus:
        1. No node in $T$ can be reachable from $s$ in $G_f \implies S_f \subseteq S$.
        2. No node in $S$ can reach $t$ in $G_f \implies T_f \subseteq T = V \setminus S \implies S \subseteq V \setminus T_f$.
    *   Therefore, any minimum cut $S$ must satisfy:
        $$S_f \subseteq S \subseteq V \setminus T_f$$
    *   If $S_f \cup T_f = V$, then $S_f = V \setminus T_f$, so we must have $S = S_f$, confirming the minimum cut is unique.
    *   If $S_f \cup T_f \neq V$, there exists some vertex $v \notin S_f \cup T_f$. We can construct at least two distinct minimum cuts: $(S_f, V \setminus S_f)$ and $(V \setminus T_f, T_f)$. Since $v \notin S_f$ and $v \in V \setminus T_f$, these cuts are distinct, confirming the min-cut is not unique.
*   **Complexity:** Dominated by the initial max flow computation: **$O(|V|^2 |E|)$** time (using Dinic's), plus $O(|V| + |E|)$ for the reachability scans.

### 9. Flow and Matching Complexity Matrix

|**Algorithm Primitive**|**Input Parameter Conditions**|**Time Complexity**|**Space Complexity**|
|---|---|---|---|
|**Edmonds-Karp**|General directed network graphs|$O(\|V\| \cdot \|E\|^2)$|$\Theta(\|V\| + \|E\|)$|
|**Dinic's Algorithm**|General directed network graphs|$O(\|V\|^2 \cdot \|E\|)$|$\Theta(\|V\| + \|E\|)$|
|**Hopcroft-Karp**|Bipartite matching unweighted networks|$\mathbf{O(\|E\| \sqrt{\|V\|})}$|$\Theta(\|V\| + \|E\|)$|
|**Cut Intersection Pass**|Requires precalculated flow fields|$O(\|V\| + \|E\|)$|$\Theta(\|V\| + \|E\|)$|

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]