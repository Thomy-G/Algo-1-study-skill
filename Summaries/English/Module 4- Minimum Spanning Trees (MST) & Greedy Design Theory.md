---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-17
---
# Module 4- Minimum Spanning Trees (MST) & Greedy Design Theory

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 4: Minimum Spanning Trees (MST) & Greedy Design Theory

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering Greedy design theory and Minimum Spanning Trees. Mastery of these concepts is essential for successfully solving network optimization, cycle intersections, and dynamic edge-weight modifications in past exams.

### 1. Foundational Vocabulary & Greedy Choice Theory

In past exams, you are frequently required to prove the optimal convergence of a local choice or leverage structural modifications. Greedy algorithms commit to a single subproblem by making a locally optimal decision and never reversing it.

#### 1.1 Core Mathematical Characterization

Let $G = (V, E)$ be a connected, undirected graph with a real-valued weight function $w: E \to \mathbb{R}$. A **Minimum Spanning Tree (MST)** is an acyclic subgraph $T = (V, E_T)$ where $E_T \subseteq E$ that connects all vertices in $V$ and minimizes the total cumulative weight metric:

$$w(T) = \sum_{e \in E_T} w(e)$$

#### 1.2 The Cut and Cycle Properties

To construct rigorous proofs for open-ended exam questions, you must memorize these two dual structural pillars:

- **The Cut Property (תכונת החתך):** For any structural cut $(S, V \setminus S)$ of a graph $G$, if an edge $e = (u, v)$ is the strictly unique lightest edge crossing the cut boundary ($u \in S, v \in V \setminus S$), then this edge **must belong to every MST** of $G$.
    
- **The Cycle Property (תכונת המעגל):** For any simple cycle $C \subseteq E$ in a graph $G$, if an edge $e' \in C$ is the strictly heaviest edge on the cycle path, then this edge **cannot belong to any MST** of $G$.
    

> [!TIP]
> 
> **Exam Invariant (Unique Weights):** If all edge weights in $G$ are distinct, the graph is guaranteed to have exactly **one unique MST**. If weights are repeated, multiple distinct valid spanning trees can exist, but their total minimum cost remains identical.

### 2. Core Spanning Tree Algorithms

#### 2.1 Kruskal's Algorithm (אלגוריתם קרוסקל)

- **Operational Strategy:** A greedy component-merging approach. It sorts all edges in $E$ in strictly increasing order of weight, then processes them sequentially. An edge is added if and only if it connects two previously disconnected components.
    
- **Data Structure Engine:** Managed via a **Disjoint-Set (Union-Find)** structure utilizing path compression and union by rank.
    

Plaintext

```
MST-KRUSKAL(G, w):
    E_T = ∅
    for each vertex u in V:
        MAKE-SET(u)
    sort the edges of E in non-decreasing order by weight w
    for each edge (u, v) in E by sorted order:
        if FIND-SET(u) != FIND-SET(v):
            E_T = E_T ∪ {(u, v)}
            UNION(u, v)
    return E_T
```

- **Complexity Matrix:** Sorting takes $O(|E| \log |E|) = O(|E| \log |V|)$. The Union-Find operations cost $O(|E| \cdot \alpha(|V|))$, where $\alpha$ is the inverse Ackermann function. Total Time: $\mathbf{O(\|E\| \log \|V\|)}$, Space: $\Theta(|V|)$.
    

#### 2.2 Prim's Algorithm (אלגוריתם פרים)

- **Operational Strategy:** A greedy neighborhood-expansion approach. It grows a single tree from an arbitrary root vertex $r$. At each step, it identifies the lightest edge crossing the cut boundary between the visited tree vertices $S$ and the unvisited vertices $V \setminus S$, absorbing the corresponding neighbor.
    
- **Data Structure Engine:** Managed via a **Min-Priority Queue** tracking the minimum distance crossing into the growing tree.
    

Plaintext

```
MST-PRIM(G, w, r):
    for each u in V:
        key[u] = inf
        pi[u] = NIL
    key[r] = 0
    Q = BUILD-PRIORITY-QUEUE(V) // Keyed by key[u]
    while Q != ∅:
        u = EXTRACT-MIN(Q)
        for each v in Adj[u]:
            if v ∈ Q and w(u, v) < key[v]:
                pi[v] = u
                key[v] = w(u, v)
                DECREASE-KEY(Q, v, key[v])
```

##### Priority Queue Complexity Tradeoffs

|**Queue Implementation Choice**|**EXTRACT-MIN Cost**|**DECREASE-KEY Cost**|**Total Prim Time Complexity**|
|---|---|---|---|
|**Binary Heap (Min-Heap)**|$O(\log\|V\|)$|$O(\log\|V\|)$|$O(\|E\| \log \|V\|)$|
|**Fibonacci Heap**|$O(\log\|V\|)$ amortized|$O(1)$ amortized|$O(\|E\| + \|V\| \log \|V\|)$|

#### 2.3 Huffman Coding (קוד הופמן)

*   **Objective:** Construct an optimal prefix-free binary code for a set of characters with given frequencies, minimizing the total length of the encoded message.
*   **Operational Strategy:** A greedy approach that builds a binary tree from the bottom up. In each step, it merges the two nodes with the lowest frequencies into a new parent node whose frequency is the sum of the children's frequencies.
*   **Data Structure Engine:** Managed via a **Min-Priority Queue** keyed by node frequency.

```plaintext
HUFFMAN(C):
    n = |C|
    Q = BUILD-PRIORITY-QUEUE(C) // Keyed by frequency f[c]
    for i = 1 to n - 1:
        allocate a new node z
        z.left = x = EXTRACT-MIN(Q)
        z.right = y = EXTRACT-MIN(Q)
        f[z] = f[x] + f[y]
        INSERT(Q, z)
    return EXTRACT-MIN(Q) // Return the root of the tree
```
*   **Complexity:** Time: $O(n \log n)$ (since each priority queue operation takes $O(\log n)$ time for $n$ characters), Space: $O(n)$ to store the tree.

#### 2.4 Advanced Spanning Tree Primitives (אלגוריתמי עץ פורש מינימלי מתקדמים)

##### 1. Boruvka / Sollen Algorithm (אלגוריתם בורובקה)
*   **Operational Strategy:** Builds the MST by growing multiple trees in a forest simultaneously. Initially, every vertex is its own tree. In each phase, every tree independently finds its cheapest incident edge (a light crossing edge) and adds it to the forest. The connected components (trees) are contracted into super-nodes, and the process repeats.
*   **Complexity:** Since each phase contracts the number of trees by at least half, there are at most $\log |V|$ phases. Each phase scans the edges in $O(|E|)$ time, resulting in **$O(|E| \log |V|)$** total time.

```plaintext
BORUVKA(G, w):
    T = ∅
    Initialize forest F where each v in V is a separate tree component
    while |F| > 1:
        let cheapest[1...|F|] be an array of NIL edges
        for each edge (u, v) in E:
            Comp_u = FIND-COMPONENT(F, u)
            Comp_v = FIND-COMPONENT(F, v)
            if Comp_u != Comp_v:
                if cheapest[Comp_u] == NIL or w(u, v) < w(cheapest[Comp_u]):
                    cheapest[Comp_u] = (u, v)
                if cheapest[Comp_v] == NIL or w(u, v) < w(cheapest[Comp_v]):
                    cheapest[Comp_v] = (u, v)
        for i = 1 to |F|:
            if cheapest[i] != NIL:
                if cheapest[i] not in T:
                    T = T ∪ {cheapest[i]}
                    UNION-COMPONENTS(F, cheapest[i])
    return T
```

##### 2. Yao's MST Algorithm (אלגוריתם יאו)
*   **Operational Strategy:** An optimization of Boruvka's algorithm. Instead of sorting all edges, Yao partitions the incident edges of each vertex $v$ into $k$ sorted blocks $E_v^{(1)}, E_v^{(2)}, \dots, E_v^{(k)}$ using Median Selection (QuickSelect). In each Boruvka phase, the algorithm only inspects the first active block of edges for $v$, discarding it when all its edges are internal, and moving to the next block.
*   **Complexity:** Setting $k = \log |V|$ and running $\log \log |V|$ initial Boruvka phases yields a total complexity of:
    $$\mathbf{O(|E| \log \log |V| + |V| \log |V|)}$$

##### 3. KKT (Karger-Klein-Tarjan) Randomized Linear-Time Algorithm
*   **Objective:** Finds the MST of an undirected connected weighted graph in **expected $O(|V| + |E|)$** time.
*   **Key Primitives:**
    *   *A-light edges:* An edge $e$ is $A$-light if it doesn't create a cycle in $A \subseteq E$, or if it is the cheapest edge on the unique cycle in $A \cup \{e\}$. Deterministic verification of $A$-lightness takes $O(|V| + |E|)$ time.
*   **Algorithm Steps:**
    1.  Perform 3 iterations of Boruvka contraction to shrink the number of vertices: $V_1 = V/8$. Let $C$ be the contracted edges.
    2.  Construct a subgraph $G_2$ by sampling at most $2|V_1|$ edges from $G_1$ randomly and uniformly.
    3.  Find the MST of $G_2$ recursively: $F_2 = \text{MST-KKT}(G_2)$.
    4.  Verify which edges of $G_1$ are $F_2$-light, removing all $F_2$-heavy edges to obtain $G_3(V_1, E_3)$. The expected number of $F_2$-light edges is at most $|E_1|/2$.
    5.  Find the MST of $G_3$ recursively: $F = \text{MST-KKT}(G_3)$.
    6.  Return $C \cup F$.
*   **Expected Runtime Recurrence:**
    $$T(V, E) = T\left(\frac{|V|}{8}, \frac{|V|}{4}\right) + T\left(\frac{|V|}{8}, \frac{|E|}{2}\right) + O(|V| + |E|) \implies \mathbf{O(|V| + |E|)}$$

### 3. Exam-Style Applications & Problem Solving Techniques

#### 3.1 Paradigm A: Bottleneck Spanning Trees & Threshold Decisions (2025 Moed A)

> [!IMPORTANT]
> 
> **Exam Target:** A _Bottleneck Spanning Tree_ (BST) minimizes the weight of its _single heaviest edge_, rather than minimizing the sum of all its edges. You must know how to prove that **every MST is automatically a valid Bottleneck Spanning Tree**, and how to check if a specific bottleneck constraint $k$ is feasible in linear time without fully building the tree.

#### 3.2 Worked Example: Linear-Time Bottleneck Feasibility Validation

Given an undirected graph $G = (V, E)$ with real weights $w: E \to \mathbb{R}$ and a target constraint threshold $k$. Design an algorithm running in **$O(|V| + |E|)$ time** that determines whether there exists a spanning tree on $G$ whose maximum edge weight is at most $k$.

##### Step-by-Step Problem Resolution:

1. **Apply a Threshold Filter on the Edge Set:**
    
    Instead of running Kruskal's sorting sweep, apply a strict structural filter. Define a filtered subgraph $G_k = (V, E_k)$ where an edge is retained if and only if its weight satisfies the threshold constraint:
    
    $$E_k = \{ e \in E \mid w(e) \le k \}$$
    
2. **Evaluate Connectivity Components:**
    
    Run a linear-time connectivity scan (such as standard Breadth-First Search or Depth-First Search) on the filtered graph $G_k$, starting from an arbitrary vertex $r \in V$.
    
3. **Check Global Reachability Invariants:**
    
    - Track the total number of unique vertices reached during the traversal.
        
    - If the search successfully reaches all $|V|$ vertices, the subgraph $G_k$ is fully connected. This guarantees that a spanning tree can be extracted using only edges from $E_k$, meaning a bottleneck value $\le k$ is achievable. Output `TRUE`.
        
    - If the search fails to visit all $|V|$ vertices, no legal spanning tree can be formed without using at least one edge heavier than $k$. Output `FALSE`.
        
4. **Complexity Analysis Verification:**
    
    - Filtering the edge array into $E_k$ takes $O(|E|)$ time.
        
    - Running BFS/DFS on $G_k$ takes $O(|V| + |E_k|) = O(|V| + |E|)$ time.
        
    - Total Runtime: **$\mathbf{O(\|V\| + \|E\|)}$**, achieving linear execution and bypassing the $O(|E| \log |V|)$ sorting bottleneck.
        

#### 3.3 Paradigm B: Dynamic Tree Adjustments & Edge Modification Recovery (2025 Moed B / Moed C)

> [!NOTE]
> 
> **Exam Target:** If the weight of an edge changes after you have already computed an MST, do not recompute the tree from scratch using Kruskal's or Prim's algorithm. Instead, utilize the properties of the precomputed tree to find the updated MST in $O(|V|)$ time.

#### 3.4 Worked Example: Recovering from non-MST Edge Weight Reductions

You are given a graph $G = (V, E)$, its unique minimum spanning tree $T$, and a specific edge $e^* = (u, v)$ that does **not** belong to $T$ ($e^* \notin T$). The weight of $e^*$ is suddenly reduced to a new value $w_{\text{new}}(e^*)$. Design an algorithm running in **$O(|V|)$ time** that reconstructs the valid updated MST after this modification.

##### Step-by-Step Problem Resolution:

1. **Locate the Fundamental Cycle:**
    
    Adding the non-tree edge $e^* = (u, v)$ to the spanning tree $T$ forms a single unique simple cycle, denoted as $C$.
    
    - Run a traversal (such as BFS or DFS) restricted entirely to the edges of the tree $T$, searching for a path from $u$ to $v$.
        
    - Gathering the edges of this tree-path along with $e^*$ isolates the simple cycle $C$ in $O(|V|)$ time.
        
2. **Apply the Cycle Property Rule:**
    
    Find the heaviest edge on the isolated cycle $C$:
    
    $$e_{\max} = \arg\max_{e \in C} \{ w(e) \}$$
    
3. **Construct the Updated Spanning Tree:**
    
    - **Case 1:** If $w_{\text{new}}(e^*)$ is still greater than or equal to $w(e_{\max})$, then $e^*$ remains the heaviest edge on the cycle $C$. By the Cycle Property, it cannot belong to the MST. The tree remains unchanged: $T_{\text{new}} = T$.
        
    - **Case 2:** If $w_{\text{new}}(e^*)$ drops below $w(e_{\max})$, then $e_{\max}$ becomes the heaviest edge on the cycle. Remove $e_{\max}$ and insert $e^*$ to break the cycle and minimize the total weight:
        
        $$T_{\text{new}} = T \cup \{e^*\} \setminus \{e_{\max}\}$$
        
4. **Complexity Analysis Verification:**
    
    - The tree $T$ contains exactly $|V| - 1$ edges. Running a traversal on $T$ to isolate the path between $u$ and $v$ takes $O(|V|)$ time.
        
    - The cycle $C$ contains at most $|V|$ edges. Finding the maximum weight edge on this cycle takes $O(|V|)$ time.
        
    - Modifying the tree structure takes $O(1)$ pointer adjustments.
        
    - Total Runtime: **$\mathbf{O(\|V\|)}$**, completely bypassing the standard $O(|E| \log |V|)$ reconstruction penalty.
        

### 3.5 Paradigm C: Advanced Spanning Tree Applications (Homework Reductions)

#### 3.5.1 Feedback Edge Set (FES) Optimization
*   **Definition:** Given an undirected graph $G = (V, E)$, a *Feedback Edge Set* (FES) is a set of edges $F \subseteq E$ such that removing $F$ breaks all cycles in $G$ (making $G' = (V, E \setminus F)$ a forest).
*   **Reductions:**
    1.  **FES of Minimum Size:**
        *   *Goal:* Minimize $|F|$, which is equivalent to maximizing the number of edges retained in the cycle-free forest $E \setminus F$.
        *   *Algorithm:* Run DFS or BFS to find any spanning forest $T$ of $G$. Output $F = E \setminus T$.
        *   *Complexity:* **$O(|V| + |E|)$** time.
    2.  **FES of Maximum Weight:**
        *   *Goal:* Given a weight function $w: E \to \mathbb{R}$, find $F$ maximizing $\sum_{e \in F} w(e)$.
        *   *Algorithm:*
            1. Identify all positive-weight edges: $P = \{ e \in E \mid w(e) \ge 0 \}$. These should always be included in $F$.
            2. For the negative-weight edges $E \setminus P$, we want to minimize the weight of the edges not in $F$ (which must form a cycle-free forest). Thus, we find a Minimum Spanning Forest (MSF) $T$ on the negative edges $E \setminus P$.
            3. Output $F = P \cup ((E \setminus P) \setminus T)$.
        *   *Complexity:* **$O(|E| + |V| \log |V|)$** time.
    3.  **FES of Minimum Weight:**
        *   *Goal:* Find $F$ minimizing $\sum_{e \in F} w(e)$.
        *   *Algorithm:* Negate the weights: $w'(e) = -w(e)$ for all $e \in E$, and run the FES of Maximum Weight algorithm under $w'$.
        *   *Complexity:* **$O(|E| + |V| \log |V|)$** time.

*   **Computing a Minimum Spanning Forest (MSF):**
    To find a Minimum Spanning Forest (MSF) on a general, possibly disconnected graph $H = (V, E_H)$:
    1.  *Global Kruskal's:* Run Kruskal's algorithm on the entire edge set $E_H$ without modification. Since Kruskal's only adds edges that connect separate trees and never forms cycles, it naturally handles disconnected graphs, terminating with an MST for each connected component.
    2.  *Component-wise MST:* Alternatively, run a DFS or BFS to find the connected components of $H$ in $O(|V| + |E_H|)$ time, and then run a standard MST algorithm (e.g., Prim's or Kruskal's) on each component. The union of these MSTs is the MSF.

#### 3.5.2 Spanning Tree of Vertex Weights (Minimum Graded Weight Spanning Tree)
*   **Problem:** Given a connected undirected graph $G = (V, E)$ and a vertex weight function $w: V \to \mathbb{R}$. The *graded weight* of a spanning tree $T$ is defined as $w_d(T) = \sum_{v \in V} w(v) \cdot \text{deg}_T(v)$. Find a spanning tree with the minimum graded weight.
*   **The Reduction:**
    1.  Notice that the degree of a vertex $v$ in $T$ counts the number of tree edges incident to $v$. We can rewrite the sum over vertices as a sum over edges:
        $$w_d(T) = \sum_{v \in V} w(v) \cdot \text{deg}_T(v) = \sum_{(u, v) \in T} \big( w(u) + w(v) \big)$$
    2.  Define a new edge weight function $w': E \to \mathbb{R}$ where:
        $$w'(u, v) = w(u) + w(v) \quad \forall (u, v) \in E$$
    3.  Run a standard MST algorithm (e.g. Prim's or Kruskal's) on $G$ with the edge weights $w'$. The resulting MST has the minimum graded weight.
*   **Complexity:** Dominated by the MST algorithm: **$O(|E| + |V| \log |V|)$** time.

#### 3.5.3 MST with Edge Weights $\{1, 2\}$
*   **Problem:** Compute the MST of a graph $G = (V, E)$ where edge weights are restricted to $w(e) \in \{1, 2\}$, in **$O(|V| + |E|)$ time**.
*   **The Algorithm (Prim's with 1-2 Queue):**
    Instead of using a binary heap ($O(\log |V|)$ operations), maintain three doubly-linked lists representing the priority queue: $V_1$ (vertices with key 1), $V_2$ (vertices with key 2), and $V_{\infty}$ (vertices with key $\infty$).
    1.  *Initialization:* Set key $[u] = \infty$ and insert all vertices into $V_{\infty}$. Set key $[s] = 1$ for a starting vertex $s$ and move it to $V_1$.
    2.  *Extract-Min:*
        *   If $V_1$ is not empty, pop a vertex $u$ from $V_1$ and set key $[u] = 0$.
        *   Else, if $V_2$ is not empty, pop $u$ from $V_2$ and set key $[u] = 0$.
        *   Else, terminate.
    3.  *Decrease-Key:* For each neighbor $v$ of the extracted vertex $u$:
        *   If key $[v] > w(u, v)$ and key $[v] \neq 0$:
            *   Remove $v$ from its current list using its doubly-linked pointers in $O(1)$ time.
            *   Set key $[v] = w(u, v) \in \{1, 2\}$ and insert $v$ into list $V_{\text{key}[v]}$.
*   **Complexity:**
    *   Initialization takes $O(|V|)$ time.
    *   There are $|V|$ Extract-Min operations, each taking $O(1)$ time.
    *   There are at most $|E|$ Decrease-Key operations, each taking $O(1)$ time.
    *   Total time complexity: **$O(|V| + |E|)$**.

*   **Can we use Bucket Sort + Kruskal's? (Exam Nuance):**
    *   *Sorting:* Since edge weights are only $\{1, 2\}$, we can sort them in $O(|E|)$ time using Bucket Sort or Counting Sort.
    *   *Kruskal's Execution:* After sorting, running Kruskal's standard loop requires performing $O(|E|)$ Disjoint-Set union/find operations. Using path compression and union-by-rank, this takes $O(|V| + |E| \cdot \alpha(|V|))$ time, where $\alpha$ is the inverse Ackermann function. While practically linear (since $\alpha(|V|) \le 4$), it is theoretically not strictly $O(|V| + |E|)$.
    *   *Strict $O(|V| + |E|)$ alternative (Component Contraction):* 
        1. Find the connected components of the subgraph $G_1 = (V, E_1)$ containing only edges of weight 1 using BFS/DFS. Within each component, select a spanning forest $F_1$. This takes $O(|V| + |E|)$ time.
        2. Contract each connected component of $G_1$ into a single super-vertex. 
        3. Build a graph of super-vertices connected by edges of weight 2. Run a BFS/DFS to find a spanning forest $F_2$ of these super-vertices. This takes $O(|V| + |E|)$ time.
        4. The final MST is $T = F_1 \cup F_2$, constructed in strictly $O(|V| + |E|)$ time.

#### 3.5.4 Reverse-Delete MST Algorithm
*   **Operational Strategy:** A greedy MST algorithm that starts with the full edge set and removes edges one-by-one in a way that preserves connectivity.
*   **Algorithm:**
    1. Sort the edge set $E$ in descending order of weight.
    2. For each edge $e \in E$:
        *   Remove $e$ from the graph.
        *   If the graph becomes disconnected, add $e$ back.
*   **Correctness Proof:**
    *   An edge $e$ is deleted if and only if it is part of a cycle (since removing an edge from a cycle cannot disconnect the graph).
    *   Because we check edges in descending order of weight, the deleted edge must be the heaviest edge on that cycle.
    *   By the **Cycle Property**, the heaviest edge on any cycle cannot belong to the MST. Thus, all deletions are correct, and the remaining edges form a minimum spanning tree.
*   **Complexity:**
    *   Sorting takes $O(|E| \log |V|)$ time.
    *   There are $|E|$ connectivity checks. Using a dynamic connectivity data structure, each check takes $O(\log |V| \log^* |V|)$ time.
    *   Total time complexity: **$O(|E| \log |V| \log^* |V|)$** (or $O(|E| \log |V|)$ on simple implementations).

*   **Dynamic Connectivity Deep-Dive:**
    *   *Definition:* A **dynamic connectivity data structure** maintains components of a graph under edge insertions, edge deletions, and connectivity queries (`Connected(u, v)`). 
    *   *Why Disjoint-Sets (Union-Find) Fail:* Standard Union-Find is **incremental-only** (supports unions but not deletions). Deleting an edge requires re-evaluating connectivity because path compression flattens tree structures, losing the actual graph topology. We cannot simply "undo" a union arbitrarily.
    *   *Implementation Options:*
        1.  *Link-Cut Trees (Sleator & Tarjan):* If the graph is a forest (e.g. maintaining a spanning forest), Link-Cut Trees support insertions (`Link`), deletions (`Cut`), and connectivity queries in $O(\log |V|)$ amortized time.
        2.  *Holm–de Lichtenberg–Thorup (HDT):* For general graphs under fully dynamic edge updates, HDT achieves $O(\log^2 |V|)$ amortized update time and $O(\log |V| / \log \log |V|)$ query time.
        3.  *Thorup's Decremental Connectivity:* In decremental settings (where we only delete edges, as in Reverse-Delete), Thorup's structure achieves $O(\log |V| \log^* |V|)$ amortized update time, which is used to achieve the optimal $O(|E| \log |V| \log^* |V|)$ time.

#### 3.5.5 Kruskal Edge Sorting Theorem
*   **Theorem Statement:** For any valid minimum spanning tree $T$ of a graph $G = (V, E)$, there exists a sorting order $\pi$ of the edges of $G$ such that Kruskal's algorithm on $\pi$ returns exactly $T$.
*   **Proof:**
    1. Let $T = (V, E_T)$ be a minimum spanning tree of $G$.
    2. Sort the edges of $G$ primarily by weight in ascending order.
    3. To break ties among edges of the same weight, place edges that belong to $E_T$ *before* edges that do not belong to $E_T$.
    4. Run Kruskal's algorithm on this sorted sequence.
    5. During Kruskal's execution, when considering edges of weight $w$, all tree edges of weight $w$ are examined before any non-tree edges of weight $w$. Since $T$ is acyclic, Kruskal's will successfully add every tree edge of weight $w$ to the forest. Once all tree edges of weight $w$ are added, any non-tree edge of weight $w$ must form a cycle with the already-added tree edges of weight $w$ and smaller weight edges, and will thus be rejected.
    6. Therefore, the algorithm returns exactly the tree $T$. $\blacksquare$

### 4. Fibonacci Heap Data Structure Architecture

A **Fibonacci Heap** is a collection of min-heap-ordered trees. It provides better amortized running time bounds than binary heaps for many operations, making it highly optimal for Dijkstra's SSSP and Prim's MST algorithms.

#### 4.1 Structural Representation
*   **Root List:** Trees in a Fibonacci Heap are stored in a circular doubly linked list. The root list has a pointer `min` pointing to the root containing the minimum key.
*   **Node Attributes:** For each node $x$:
    - `degree[x]`: The number of children of $x$.
    - `child[x]`: A pointer to one of $x$'s children (the children themselves form a circular doubly linked list).
    - `parent[x]`: A pointer to $x$'s parent.
    - `mark[x]`: A Boolean value indicating whether $x$ has lost a child since the last time it was made a child of another node. (Roots are always unmarked).
*   **Rank Constraint:** The maximum degree $D(n)$ of any node in a Fibonacci heap of size $n$ is bounded by $O(\log n)$.

#### 4.2 Structural Operations & Amortized Cost
Let the potential function be defined as:
$$\Phi(H) = t(H) + 2m(H)$$
where $t(H)$ is the number of trees in the root list of $H$, and $m(H)$ is the number of marked nodes in $H$.

1.  **Insert (הכנסה) — Amortized Cost: $O(1)$**
    *   Create a new node with key $x$, unmarked, and insert it into the root list.
    *   If $key[x] < key[min]$, update `min` to point to $x$.
    *   *Potential Change:* $t(H)$ increases by 1, $m(H)$ is unchanged.
        $$\Delta \Phi = 1 \implies C_{amortized} = C_{actual} + \Delta \Phi = O(1) + 1 = O(1)$$

2.  **Decrease-Key (הפחתת מפתח) — Amortized Cost: $O(1)$**
    *   If the new key violates the min-heap property (new key is smaller than parent's key), we cut the node $x$ from its parent $y$ and move it to the root list.
    *   **Cascading Cut (חיתוך משורשר):**
        *   If the parent $y$ is unmarked, mark it.
        *   If $y$ is already marked, cut $y$ from its parent, move it to the root list, unmark it, and recursively repeat for $y$'s parent.
    *   This ensures that the sizes of the trees remain exponentially large in the degree of their roots (related to Fibonacci numbers).

3.  **Extract-Min (מחיקת מינימום) — Amortized Cost: $O(\log n)$**
    *   Remove the minimum node `min` from the root list.
    *   Insert all of `min`'s children into the root list.
    *   **Consolidate (מיזוג):** Reduce the number of trees in the root list by repeatedly merging trees of the same degree.
        *   Maintain an array $A[0 \dots D(n)]$ where $A[d]$ points to a tree of degree $d$.
        *   For each root $x$ in the root list:
            *   If another tree $y$ has the same degree $d = \text{degree}[x]$, link them (make the root with the larger key a child of the root with the smaller key). This increases the degree of the winner to $d+1$.
            *   Repeat until no two trees share the same degree.
        *   Rebuild the circular doubly linked list from the consolidated roots and update `min`.

### 5. Comprehensive Algorithmic Complexity Matrix

This guide tracks the absolute time and space complexity baselines for Minimum Spanning Tree algorithms under both standard representations.

| **Algorithm Primitive**        | **Adjacency List + Binary Heap** | **Adjacency List + Fibonacci Heap** | **Adjacency Matrix Representation** | **Space Complexity** |
| ------------------------------ | -------------------------------- | ----------------------------------- | ----------------------------------- | -------------------- |
| **Kruskal's Algorithm**        | $O(\|E\| \log \|V\|)$            | $O(\|E\| \log \|V\|)$               | $O(\|V\|^2 + \|E\| \log \|V\|)$      | $\Theta(\|V\| + \|E\|)$ |
| **Prim's Algorithm**           | $O(\|E\| \log \|V\|)$            | $O(\|E\| + \|V\| \log \|V\|)$       | $O(\|V\|^2)$                        | $\Theta(\|V\|)$      |
| **BST Validation Feasibility** | $O(\|V\| + \|E\|)$               | $O(\|V\| + \|E\|)$                  | $O(\|V\|^2)$                        | $\Theta(\|V\|)$      |

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]