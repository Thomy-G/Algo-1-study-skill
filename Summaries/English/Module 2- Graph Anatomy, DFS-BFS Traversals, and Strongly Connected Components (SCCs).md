---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-17
---
# Module 2- Graph Anatomy, DFS-BFS Traversals, and Strongly Connected Components (SCCs)

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 2: Graph Anatomy, DFS/BFS Traversals, and Strongly Connected Components (SCCs)

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering the structural anatomy of graphs, traversal invariants, and connectivity decomposition. Mastery of these concepts is essential for successfully solving the graph exploration and structural network sections in past exams.

### 1. Foundational Vocabulary & Structural Metrics

In past exams, you are frequently required to analyze properties of graph representations or exploit structural boundaries relating vertices ($V$) and edges ($E$). The three key conceptual building blocks you must master are:

#### 1.1 Structural Graph Metrics & Bounds

A graph $G = (V, E)$ is maintained using fundamental operational primitives whose sizes govern all linear-time execution bounds:

|**Graph Metric / Concept**|**Formal Mathematical Definition**|**Core Structural Bound / Property**|
|---|---|---|
|**Sum of Undirected Degrees**|$\sum_{u \in V} \text{deg}(u)$|$= 2\vert E\vert$|
|**Sum of Directed Degrees**|$\sum_{u \in V} \text{in-deg}(u) = \sum_{u \in V} \text{out-deg}(u)$|$= \vert E\vert$|
|**Dense Graph Density**|Bounded maximally by complete layout.|Max edges: $\vert E\vert = \Theta(\vert V\vert^2)$|
|**Sparse Graph Density**|Edges are linearly bounded by vertex count.|$\vert E\vert = O(\vert V\vert)$|

#### 1.2 Graph Memory Representations

Choosing a data structure controls the efficiency of scanning local neighborhoods vs. evaluating absolute connectivity boundaries:

- **Adjacency Matrix** (מטריצת שכנויות): A Boolean matrix $M$ of size $|V| \times |V|$ where $M[i,j] = 1 \iff (v_i, v_j) \in E$.
    
    - _Space Complexity:_ $\Theta(|V|^2)$.
        
    - _Edge Lookup ($(u,v) \in E$):_ $O(1)$.
        
    - _Neighbor Scan ($Adj[u]$):_ $\Theta(|V|)$.
        
- **Adjacency Lists** (רשימות שכנויות): An array of $|V|$ linked lists, where list $i$ stores all vertices $v_j$ reachable via an outgoing edge from $v_i$.
    
    - _Space Complexity:_ $\Theta(|V| + |E|)$.
        
    - _Edge Lookup ($(u,v) \in E$):_ $O(\log(\min(\text{deg}(u), \text{deg}(v))))$ if balanced search trees (like AVL trees) are implemented within lists.
        
    - _Neighbor Scan ($Adj[u]$):_ $\Theta(\text{out-deg}(u))$.
        

### 2. Traversal Classification & Invariants

Graph exploration algorithms classify edges and discover connectivity pathways based on search timing constraints and path structures.

#### 2.1 Breadth-First Search (BFS) (אלגוריתם חיפוש לרוחב)

*   **Core Concept:** Explores the vertices of a graph level-by-level, starting from source $s$. It computes the shortest path distance (in terms of minimum number of edges) to all reachable vertices.
*   **Data Structure Engine:** Managed via a **First-In, First-Out (FIFO) Queue** $Q$.

```plaintext
BFS(G, s):
    for each vertex u in V \ {s}:
        color[u] = WHITE
        d[u] = inf
        pi[u] = NIL
    color[s] = GRAY
    d[s] = 0
    pi[s] = NIL
    Q = ∅
    ENQUEUE(Q, s)
    while Q != ∅:
        u = DEQUEUE(Q)
        for each v in Adj[u]:
            if color[v] == WHITE:
                color[v] = GRAY
                d[v] = d[u] + 1
                pi[v] = u
                ENQUEUE(Q, v)
        color[u] = BLACK
```
*   **Complexity:** Time: $\Theta(|V| + |E|)$ under Adjacency Lists, Space: $\Theta(|V|)$.

#### 2.2 Depth-First Search (DFS) (אלגוריתם חיפוש לעומק)

*   **Core Concept:** Explores the graph by searching deeper along each branch before backtracking. It maintains discovery/finishing times and generates a depth-first forest.
*   **Data Structure Engine:** Recursion stack (or LIFO stack).

```plaintext
DFS(G):
    for each vertex u in V:
        color[u] = WHITE
        pi[u] = NIL
    time = 0
    for each vertex u in V:
        if color[u] == WHITE:
            DFS-VISIT(G, u)

DFS-VISIT(G, u):
    time = time + 1
    d[u] = time
    color[u] = GRAY
    for each v in Adj[u]:
        if color[v] == WHITE:
            pi[v] = u
            DFS-VISIT(G, v)
    color[u] = BLACK
    time = time + 1
    f[u] = time
```
*   **Complexity:** Time: $\Theta(|V| + |E|)$ under Adjacency Lists, Space: $\Theta(|V|)$ (recursion stack height).

#### 2.3 DFS Timestamps: d[] and f[]

During a Depth-First Search (DFS), we maintain a global clock counter (starting at 1) that increments with each state transition. Every vertex $u$ is assigned two distinct timestamps during the traversal:

| Timestamp Array | Mathematical Definition / Meaning | Graph Exploration State | Vertex Color | Stack Operation |
| :--- | :--- | :--- | :--- | :--- |
| **$d[u]$** | **Discovery Time** (זמן גילוי): The clock time when vertex $u$ is first visited and enters the search. | Vertex $u$ is first encountered. | Turns from **White** (`WHITE`) to **Gray** (`GRAY`). | Pushed onto the recursion stack. |
| **$f[u]$** | **Finishing Time** (זמן סיום): The clock time when the search finishes examining all outgoing edges of $u$. | All descendants of $u$ are fully explored. | Turns from **Gray** (`GRAY`) to **Black** (`BLACK`). | Popped off the recursion stack. |

##### Conceptual Comparison of Start ($d[u]$) vs. Finish ($f[u]$):

| Aspect | Start / Discovery ($d[u]$) | Finish ($f[u]$) |
| :--- | :--- | :--- |
| **Trigger Event** | First time the DFS traversal encounters vertex $u$. | DFS has completely traversed all reachable paths from $u$. |
| **Vertex Color** | Turns from **White** (`WHITE` - unvisited) to **Gray** (`GRAY` - active). | Turns from **Gray** (`GRAY` - active) to **Black** (`BLACK` - finished). |
| **Recursion Stack** | Vertex $u$ is pushed onto the stack (function call starts). | Vertex $u$ is popped off the stack (function call returns). |
| **Physical Analogy** | Entering a room for the first time. | Leaving the room after checking all doors and hallways. |
| **Parenthesis Analogy** | An opening parenthesis `(` | A closing parenthesis `)` |
| **Active Duration** | Marks the beginning of the vertex's active lifetime. | Marks the end of the vertex's active lifetime. |

##### Core Properties:
1.  **Strict Bounds:** For every vertex $u \in V$, the timestamps satisfy: $1 \le d[u] < f[u] \le 2|V|$.
2.  **No Time Collisions:** All $2|V|$ timestamps (discovery and finishing times for all vertices) are distinct integers.
3.  **Active Window:** The interval $[d[u], f[u]]$ represents the entire active lifetime of vertex $u$ in the recursion stack.

---

#### 2.4 DFS Edge Classification Hierarchy

During a Depth-First Search (DFS) execution, every edge $e = (u, v)$ is classified uniquely at the moment it is evaluated from node $u$:

```
                             DFS Edge Classes
                                    │
         ┌──────────────────────────┴──────────────────────────┐
   Structural Trees                                      Non-Tree Crossings
         │                                                     │
   ┌─────┴─────┐                                   ┌───────────┼───────────┐
Tree Edge   Forward Edge                      Back Edge             Cross Edge
 (קשת עץ)     (קשת קדימה)                     (קשת אחורה)            (קשת חוצה)
```

| **Edge Type**                | **Discovery Condition (Color of v upon inspection)** | **Vertex Timestamp Invariant**                                         |
| ---------------------------- | ---------------------------------------------------- | ---------------------------------------------------------------------- |
| **Tree Edge** (קשת עץ)       | $v$ is **White** (`WHITE`).                          | $d[u] < d[v] < f[v] < f[u]$ ($v$ becomes a direct child in forest).    |
| **Forward Edge** (קשת קדימה) | $v$ is **Black** (`BLACK`) and $d[u] < d[v]$.        | $d[u] < d[v] < f[v] < f[u]$ ($v$ is a non-child descendant).           |
| **Back Edge** (קשת אחורה)    | $v$ is **Gray** (`GRAY`).                            | $d[v] \le d[u] < f[u] \le f[v]$ ($v$ is an ancestor; implies a cycle). |
| **Cross Edge** (קשת חוצה)    | $v$ is **Black** (`BLACK`) and $d[v] < d[u]$.        | $d[v] < f[v] < d[u] < f[u]$ (No ancestor-descendant relation).         |

#### Conceptual Explanation of DFS Edge Classifications

To understand these edges, it helps to think of the DFS search as a tree structure (a forest) where we are currently standing at vertex $u$ and looking at an edge to vertex $v$:

*   **Tree Edge (קשת עץ) - *The "Discovery" Edge*:**
    *   **Concept:** You are standing at $u$ and look at $v$. $v$ is untouched (**White**). You move to $v$ and discover it. This edge becomes part of the actual tree skeleton.
    *   **Timestamp Reason:** Since $v$ is discovered *after* $u$ but must be finished *before* we can finish $u$ (recursion stack behavior), its entire discovery-finish interval is nested inside $u$'s interval ($d[u] < d[v] < f[v] < f[u]$).
*   **Forward Edge (קשת קדימה) - *The "Shortcut to Descendant" Edge*:**
    *   **Concept:** You are standing at $u$ and look at $v$. $v$ is already finished (**Black**), but it was discovered *after* you ($d[u] < d[v]$). This means $v$ is a descendant of $u$ that we reached earlier through a different path of tree edges. The edge $(u,v)$ is just a "shortcut" pointing forward to an already-explored descendant.
    *   **Timestamp Reason:** Similar to a tree edge, $v$ is a descendant, so $d[u] < d[v] < f[v] < f[u]$. The only difference is that $v$ was already visited, so we don't traverse $(u,v)$ to discover it.
*   **Back Edge (קשת אחורה) - *The "Cycle-Creating" Edge*:**
    *   **Concept:** You are standing at $u$ and look at $v$. $v$ is currently active on the recursion stack (**Gray**). Since $v$ is active, $v$ is an ancestor of $u$. By pointing to $v$, you are closing a loop from a descendant back to its ancestor, forming a **cycle**.
    *   **Timestamp Reason:** Since $v$ is an ancestor of $u$, we must have started $v$ before $u$, and we cannot finish $v$ until $u$ is finished. Thus, $u$'s interval is nested inside $v$'s interval ($d[v] \le d[u] < f[u] \le f[v]$).
*   **Cross Edge (קשת חוצה) - *The "Bridge to Another Branch" Edge*:**
    *   **Concept:** You are standing at $u$ and look at $v$. $v$ is already finished (**Black**), and it was discovered *before* you ($d[v] < d[u]$). This means $v$ is neither an ancestor nor a descendant; it belongs to a completely different branch (or even a different tree in the forest) that was already fully explored and closed.
    *   **Timestamp Reason:** Because $v$ was fully processed and closed before $u$ was even discovered, their intervals are completely disjoint and $v$ is entirely in the past ($d[v] < f[v] < d[u] < f[u]$).

#### 2.5 Core Invariants & Core Lemmas

##### 2.5.1 The Parenthesis Theorem (משפט הסוגריים)

*   **Theorem Statement:** For any two distinct vertices $u$ and $v$, their active search intervals $[d[u], f[u]]$ and $[d[v], f[v]]$ are either completely disjoint or one is entirely contained within the other. That is, they can never partially overlap (e.g., $d[u] < d[v] < f[u] < f[v]$ is impossible).
*   **Intuition & Recursion Stack:** This mirrors the Last-In, First-Out (LIFO) behavior of the recursion stack. A vertex $u$ is placed on the stack at $d[u]$ and popped off at $f[u]$. If $v$ is discovered during this time, it is pushed onto the stack after $u$ and must be popped off before $u$.
*   **Proof:** 
    *   Assume without loss of generality that $d[u] < d[v]$. 
    *   **Case 1: $d[v] < f[u]$.** Since $v$ was discovered while $u$ was active, $v$ must finish and be popped before $u$. Thus, $f[v] < f[u]$, which means the interval $[d[v], f[v]]$ is entirely nested within $[d[u], f[u]]$.
    *   **Case 2: $d[v] > f[u]$.** In this case, $u$ was already finished before $v$ was discovered. Therefore, $f[u] < d[v]$, meaning the two intervals $[d[u], f[u]]$ and $[d[v], f[v]]$ are completely disjoint. $\blacksquare$

##### 2.5.2 The White-Path Theorem (משפט המסלול הלבן)

*   **Theorem Statement:** In a DFS forest, a vertex $v$ is a descendant of vertex $u$ if and only if, at the discovery time $d[u]$, there exists a path $u \rightsquigarrow v$ consisting entirely of unvisited (**White**) vertices.
*   **Proof:**
    *   **Forward Direction ($\implies$):** Assume $v$ is a descendant of $u$ in the DFS tree. Let $P$ be the path of tree edges from $u$ to $v$. For any intermediate vertex $w$ on this path, $w$ is a descendant of $u$, so $d[u] < d[w]$. This implies that at time $d[u]$, all vertices on the path to $v$ are undiscovered (White).
    *   **Backward Direction ($\impliedby$):** Assume there exists an all-white path $P = u = v_0 \to v_1 \to \dots \to v_k = v$ at time $d[u]$. Suppose for contradiction that $v$ does not become a descendant of $u$.
        1.  Let $v_i$ be the first vertex on $P$ that fails to become a descendant of $u$. Note that $v_{i-1}$ must be a descendant of $u$ (since $u=v_0$ is trivially a descendant of itself).
        2.  Since $v_{i-1}$ is a descendant of $u$, we must finish $v_{i-1}$ before $u$, so $f[v_{i-1}] \le f[u]$.
        3.  Since $v_i$ is white at $d[u]$, we have $d[v_i] > d[u]$.
        4.  Because $v_i$ is a direct neighbor of $v_{i-1}$, when the DFS processes $v_{i-1}$, it must inspect the edge $(v_{i-1}, v_i)$. Since $v_i$ is white at $d[u]$, it must be inspected before $v_{i-1}$ finishes, meaning $d[v_i] < f[v_{i-1}]$.
        5.  Putting it together: $d[u] < d[v_i] < f[v_{i-1}] \le f[u]$.
        6.  By the Parenthesis Theorem, this means the interval $[d[v_i], f[v_i]]$ is nested inside $[d[u], f[u]]$, making $v_i$ a descendant of $u$. This contradicts our assumption. $\blacksquare$

##### 2.5.3 The DAG Property & Cycle Detection (תכונת ה-DAG וגילוי מעגלים)

*   **Theorem Statement:** A directed graph $G = (V, E)$ is acyclic (a DAG) if and only if a DFS run on $G$ yields **no back edges**.
*   **Proof:**
    *   **Backward Direction ($\impliedby$):** Assume DFS yields a back edge $(u, v)$. By definition, $v$ is an ancestor of $u$ in the DFS forest, which means there is a path of tree edges $v \rightsquigarrow u$. The edge $(u, v)$ completes a cycle: $v \rightsquigarrow u \to v$. Thus, $G$ contains a cycle, so it is not a DAG.
    *   **Forward Direction ($\implies$):** Assume $G$ contains a cycle $C$. Let $v$ be the first vertex of cycle $C$ discovered by DFS.
        1.  For any other vertex $u \in C$, there is a path $v \dots u$ along the cycle edges.
        2.  Since $v$ was the first vertex of $C$ discovered, all vertices on the path $v \dots u$ are still white at time $d[v]$.
        3.  By the White-Path Theorem, $u$ must become a descendant of $v$ in the DFS tree.
        4.  Since $u$ is a descendant of $v$ and $(u, v) \in E$ (since it is an edge on the cycle $C$), the edge $(u, v)$ goes from a descendant to an ancestor, making it a back edge by definition. $\blacksquare$
    
#### 2.6 Topological Sort (מיון טופולוגי)

A **topological sort** of a directed acyclic graph (DAG) $G = (V, E)$ is a linear ordering of all its vertices such that if $G$ contains an edge $(u, v)$, then $u$ appears before $v$ in the ordering. If the graph contains a cycle, a topological sort is impossible.

*   **Operational Execution (via DFS):**
    1.  Run DFS on $G$ to compute finishing times $f[u]$ for each vertex $u$.
    2.  As each vertex is finished, insert it at the front of a linked list.
    3.  Alternatively, sort the vertices in decreasing order of their finishing times $f[u]$.

```plaintext
TOPOLOGICAL-SORT(G):
    L = empty_linked_list()
    // Run DFS on G
    for each vertex u in V:
        color[u] = WHITE
    for each vertex u in V:
        if color[u] == WHITE:
            DFS-VISIT-TOPO(u, L)
    return L

DFS-VISIT-TOPO(u, L):
    color[u] = GRAY
    for each v in Adj[u]:
        if color[v] == WHITE:
            DFS-VISIT-TOPO(v, L)
    color[u] = BLACK
    INSERT-FRONT(L, u) // Insert at front upon finishing
```
*   **Correctness Theorem:**
    Ordering the vertices of a DAG by decreasing finishing times yields a topological sort. (Proof is documented in the [[Core Lemmas and Theorems Proof Guide#4.5 Topological Sort Correctness|Core Lemmas Proof Guide]]).
*   **Complexity Metrics:**
    *   **Time Complexity:** $\Theta(|V| + |E|)$ (under Adjacency Lists representation).
    *   **Space Complexity:** $\Theta(|V|)$ (auxiliary space).

### 3. Strongly Connected Components (SCCs) & Kosaraju's Framework

A Strongly Connected Component (SCC) is a maximal subset of vertices $C \subseteq V$ such that for every pair $u, v \in C$, there exists a path $u \leadsto v$ and a path $v \leadsto u$.

#### 3.1 Kosaraju's Linear Decomposition Algorithm

Kosaraju's algorithm isolates components in $\Theta(|V| + |E|)$ time using the structural inversion properties of the transposed graph $G^T = (V, E^T)$, where $E^T = \{(v, u) \mid (u, v) \in E\}$.

##### Algorithm Pseudocode

Plaintext

```
Kosaraju-SCC(G):
    1. Run standard DFS on G to compute finishing times f[u] for all u in V.
    2. Compute the transposed graph G^T.
    3. Run a modified DFS on G^T, but in the outer loop, inspect 
       vertices in strictly decreasing order of their computed f[u] values.
    4. Each tree produced in the second DFS forest is a distinct SCC.
```

#### 3.2 Tarjan's SCC Algorithm (אלגוריתם טרזן לרכיבים קשירים היטב)

*   **Operational Strategy:** Decomposes a graph into SCCs in a single DFS pass by using a stack and tracking low-link values:
    *   `dfn[u]`: Discovery timestamp of vertex $u$.
    *   `low[u]`: The smallest discovery timestamp reachable from $u$ through at most one back-edge or cross-edge pointing to a node currently on the stack.
*   When a DFS call finishes a node $u$ and `low[u] == dfn[u]`, then $u$ is the **root** of an SCC. All nodes on the stack from the top down to $u$ are popped and form a single SCC.

```plaintext
TARJAN-SCC(G):
    time = 0
    S = empty_stack()
    in_stack = [False] * |V|
    dfn = [NIL] * |V|
    low = [NIL] * |V|
    for each u in V:
        if dfn[u] == NIL:
            TARJAN-DFS(u)

TARJAN-DFS(u):
    time = time + 1
    dfn[u] = time
    low[u] = time
    PUSH(S, u)
    in_stack[u] = True
    for each v in Adj[u]:
        if dfn[v] == NIL:
            TARJAN-DFS(v)
            low[u] = min(low[u], low[v])
        elif in_stack[v]:
            low[u] = min(low[u], dfn[v])
    if low[u] == dfn[u]:
        // Found a root of an SCC!
        current_scc = []
        repeat:
            w = POP(S)
            in_stack[w] = False
            add w to current_scc
        until w == u
        output current_scc
```
*   **Complexity:** Time: $\Theta(|V| + |E|)$, Space: $\Theta(|V|)$ (stack and bookkeeping).

#### 3.3 Component Graph ($G^{SCC}$) Property

If we contract each individual SCC into a single super-node, the resulting component graph $G^{SCC} = (V^{SCC}, E^{SCC})$ is guaranteed to be a **DAG**.

### 4. Exam-Style Applications & Problem Solving Techniques

#### 4.1 Paradigm A: Uniqueness and Variant Sensitivity in Traversal Classifications (2026 Moed B)

> [!IMPORTANT]
> 
> **Exam Warning:** Edge classifications are **not** invariants across different executions of DFS. Modifying the sorting order of the outer initialization loop or altering the neighbor list permutation dramatically re-architects the resulting depth forest.

##### Conceptual Breakdown: Why does this happen?
When running a DFS traversal, the classification of each edge depends on two arbitrary configuration choices:
1. **Starting Order (Outer Loop):** The order in which the algorithm iterates through the vertices to initiate DFS (e.g., `for each u in V`). If you start the DFS at vertex $u$ first, any visited node $v$ becomes a descendant of $u$. If you start at $v$ first, $u$ might become a descendant of $v$, completely altering their ancestral relationship.
2. **Neighbor Traversal Order (Adjacency List):** When we are at vertex $u$, the order in which we scan its outgoing edges in $Adj[u]$ determines which paths are traversed first and which are treated as shortcuts or crossovers.

##### Concrete Example:
Consider a graph with three vertices $A, B, C$ and directed edges: $(A, B)$, $(B, C)$, and $(A, C)$.
If we start the DFS at vertex $A$:
* **Scenario A (Visit B first):**
  * Neighbor list of $A$ is ordered: $B$, then $C$.
  * **DFS path:** $A \to B \to C$.
  * **Classification:** $(A, B)$ and $(B, C)$ are **Tree Edges**. When we backtrack to $A$ and inspect $(A, C)$, $C$ is already finished (`BLACK`) and $d[A] < d[C]$, making $(A, C)$ a **Forward Edge**.
* **Scenario B (Visit C first):**
  * Neighbor list of $A$ is ordered: $C$, then $B$.
  * **DFS path:** $A \to C$ (backtrack to $A$), then $A \to B \to (C \text{ is already BLACK})$.
  * **Classification:** $(A, C)$ and $(A, B)$ are **Tree Edges**. When we are at $B$ and inspect $(B, C)$, $C$ is already finished (`BLACK`) and $d[B] > d[C]$, making $(B, C)$ a **Cross Edge**.

This demonstrates that simply changing the order in which neighbors are traversed completely flips $(B, C)$ from a **Tree Edge** to a **Cross Edge**, and $(A, C)$ from a **Forward Edge** to a **Tree Edge**.

#### 4.2 Worked Example: Breaking Structural Cross-Edge Multiplicity

Suppose you are given a directed graph $G = (V, E)$. Design a linear-time algorithm running in $O(|V| + |E|)$ time that determines whether there exists _at least one_ legal DFS traversal order (via choice of starting vertices and neighbor lists) under which a specific target edge $e^* = (u, v) \in E$ is classified as a **Tree Edge**, and implement a companion pass confirming if it can simultaneously be classified as a **Cross Edge** under an alternative permutation.

##### Step-by-Step Problem Resolution:

1. **Part 1: Forcing a Tree Edge Classification ($Tree(e^*)$):**
    
    To guarantee that $e^* = (u, v)$ is registered as a tree edge, $v$ must be unvisited (`WHITE`) at the exact instant $u$ evaluates $v$'s neighbor pointer.
    
    - **The Trick Matrix Construction:** Force the outer initialization loop of DFS to select vertex $u$ first before any other vertex in the graph.
        
    - Additionally, reorder $u$'s local adjacency list such that vertex $v$ sits at the absolute head of the sequence.
        
    - **Execution Rule:** Run standard DFS with this layout. Because $u$ executes at $t=1$, and immediately targets $v$, $v$ is encountered while white, forcing $(u, v)$ to step into the tracking forest as a Tree Edge.
        
2. **Part 2: Forcing a Cross Edge Classification ($Cross(e^*)$):**
    
    To register $e^* = (u, v)$ as a cross edge, vertex $v$ must complete its full exploration cycle (`BLACK`) before vertex $u$ is ever discovered. This requires $v$ to finish completely, meaning $d[v] < f[v] < d[u] < f[u]$.
    
    - **Reachability Constraint Checking:** First check if $v$ can reach $u$ via a path in $G$. If a path $v \leadsto u$ exists, then any DFS that starts by visiting $v$ will automatically discover $u$ as a descendant, making $u$ a child/descendant of $v$. In that case, the edge $(u,v)$ would point from a descendant ($u$) back to an ancestor ($v$), forcing it to be a **Back Edge**, not a cross edge.
        
    - **The Structural Decision Matrix:** 1. Run a validation pass (such as BFS/DFS) starting from $v$ on $G$. If $u$ is reachable from $v$, output `FALSE` for the cross-edge potential.
        
        2. If $u$ is _not_ reachable from $v$, then we can isolate them. Force the outer loop of the alternative DFS traversal to place $v$ at the front of the queue, and ensure $u$ is placed later in the outer loop sequence.
        
        3. Run DFS. Node $v$ executes, finishes its component branch completely, and turns `BLACK`. Node $u$ is discovered later at a higher timestamp, and when its list scans $v$, it notes $v$ is `BLACK` with $d[v] < d[u]$, registering $(u, v)$ as a Cross Edge.
        
3. **Complexity Analysis Verification:**
    
    - Reordering lists and arrays based on index constraints takes $O(|V| + \text{deg}(u)) = O(|V| + |E|)$ time.
        
    - Running reachability validation takes $O(|V| + |E|)$ time.
        
    - Running the explicit DFS executions takes $O(|V| + |E|)$ time.
        
    - Total runtime: $\mathbf{O(|V| + |E|)}$, matching the metric standard for linear exam sweeps.
        
##### Concrete Run Example:
Let $G = (V,E)$ where $V = \{u, v, w\}$ and $E = \{(u,w), (w,v), (u,v)\}$. Let the target edge be $e^* = (u,v)$.

*   **Forcing $e^*$ to be a Tree Edge ($Tree(e^*)$):**
    *   **Configuration:** We start the DFS outer loop at $u$, and we order $u$'s neighbor list to visit $v$ first: $Adj[u] = [v, w]$.
    *   **Execution:** 
        1. DFS starts at $u$. $u$ is discovered and turns `GRAY`.
        2. We inspect $u$'s first neighbor, $v$. Since $v$ is `WHITE` (unvisited), we immediately traverse $(u,v)$.
        3. Therefore, $(u,v)$ becomes a **Tree Edge**.
    *   **Visualizing state when $e^*$ is inspected:**
        ```mermaid
        graph TD
            u((u)) -->|inspects & traverses| v((v))
            u --> w((w))
            w --> v

            class u grayNode;
            class v whiteNode;
            class w whiteNode;

            classDef whiteNode fill:#fff,stroke:#333,stroke-width:2px;
            classDef grayNode fill:#bbb,stroke:#333,stroke-width:2px;
            classDef blackNode fill:#333,stroke:#fff,stroke-width:2px,color:#fff;
        ```

*   **Forcing $e^*$ to be a Cross Edge ($Cross(e^*)$):**
    *   **Constraint Check:** First, we confirm $v \not\leadsto u$ (there is no path from $v$ to $u$). This is true, so a cross edge classification is possible.
    *   **Configuration:** We configure the outer loop starting order to be $v \to w \to u$.
    *   **Execution:**
        1. Outer loop starts at $v$. Since $v$ has no outgoing edges, it immediately finishes. $v$ turns `BLACK` ($d[v]=1, f[v]=2$).
        2. Outer loop starts at $w$. From $w$, we inspect neighbor $v$. Since $v$ is `BLACK` and $d[v] < d[w]$, $(w,v)$ is a Cross Edge. $w$ finishes and turns `BLACK` ($d[w]=3, f[w]=4$).
        3. Outer loop starts at $u$. We discover $u$ ($d[u]=5$). From $u$, we inspect neighbor $v$. Since $v$ is `BLACK` and $d[v] < d[u]$ ($1 < 5$), the edge $(u,v)$ is classified as a **Cross Edge**.
    *   **Visualizing state when $e^*$ is inspected:**
        ```mermaid
        graph TD
            u((u)) -->|inspects| v((v))
            u --> w((w))
            w --> v

            class u grayNode;
            class v blackNode;
            class w blackNode;

            classDef whiteNode fill:#fff,stroke:#333,stroke-width:2px;
            classDef grayNode fill:#bbb,stroke:#333,stroke-width:2px;
            classDef blackNode fill:#333,stroke:#fff,stroke-width:2px,color:#fff;
        ```

#### 4.3 Paradigm B: Epidemic Outbreak / Reachability Kernels (Exercise 4 / Past Moed Problems)

> [!NOTE]
> 
> **Exam Target:** When asked to isolate a minimum set of vertices capable of reaching or transmitting a properties matrix across an entire network, solve the problem by computing SCCs, collapsing the graph into its DAG component layout ($G^{SCC}$), and finding the nodes with an **in-degree of zero**.

#### 4.4 Worked Example: Minimizing Software Patch Deployments

You are given a complex network of $n$ distributed servers represented as a directed graph $G = (V, E)$, where an edge $(u, v)$ means a patch installed on server $u$ automatically propagates to server $v$. Design an algorithm running in $O(|V| + |E|)$ time that returns the **minimum size** subset of servers $V^* \subseteq V$ where you must manually install a software patch to ensure that _every_ server in the network eventually receives it.

##### Step-by-Step Problem Resolution:

1. **Decompose the Graph into Equivalent Clusters:**
    
    Run Kosaraju's or Tarjan's algorithm to compute the Strongly Connected Components of $G$. Since every node inside an SCC can reach every other node in that same SCC, patching any single node in an SCC immunizes the entire component.
    
2. **Construct the Component DAG ($G^{SCC}$):**
    
    Build the condensed component graph $G^{SCC} = (V^{SCC}, E^{SCC})$:
    
    - $V^{SCC}$ contains one super-node for each isolated SCC.
        
    - Initialize an in-degree counter tracking array `InDegC` of size $|V^{SCC}|$ to zero.
        
    - Loop through every original edge $(u, v) \in E$:
        
        - Identify component $C_u$ containing $u$ and component $C_v$ containing $v$.
            
        - If $C_u \neq C_v$, map a directed meta-edge $(C_u, C_v)$ into $E^{SCC}$ and increment the tracking count: `InDegC`$[C_v] \leftarrow$ `InDegC`$[C_v] + 1$.
            
3. **Isolate Source Source Components:**
    
    Create an empty results array $V^*$. Find all component indices $i$ where `InDegC`$[i] == 0$.
    
    - If a component has an in-degree of 0, no external node can reach it. Thus, it must receive a manual patch allocation.
        
    - For each zero-in-degree component, pick an arbitrary representative server $u \in C_i$ and append it to $V^*$.
        
4. **Prove Correctness via DAG Properties:**
    
    - **Sufficiency:** In any finite DAG, tracing any path backward must terminate at a source vertex (in-degree 0). Therefore, every node in a DAG is reachable from at least one source vertex.
        
    - **Minimality:** Since components with an in-degree of 0 cannot be reached from any node outside themselves, each one requires at least one manual patch. Selecting exactly one node per source component minimizes $|V^*|$.
        
5. **Complexity Analysis Verification:**
    
    - SCC decomposition takes $O(|V| + |E|)$ time.
        
    - Constructing $G^{SCC}$ and calculating in-degrees takes $O(|V| + |E|)$ time.
        
    - Scanning the `InDegC` array takes $O(|V^{SCC}|) = O(|V|)$ time.
        
    - Total execution time: $\mathbf{O(|V| + |E|)}$, which matches the optimal linear performance requirement.

### 4.5 Advanced SCC & Connectivity Applications (Homework & Exam Reductions)

#### 4.5.1 Linear-Time Construction of Component Graph ($G_{SCC}$)
*   **Problem:** Given a directed graph $G = (V, E)$, construct the adjacency list of the condensed component graph $G_{SCC} = (V_{SCC}, E_{SCC})$ in $O(|V| + |E|)$ time, avoiding duplicate edges.
*   **The Bottleneck:** For each original edge $(u, v) \in E$, if $u$ and $v$ belong to different components $C_u$ and $C_v$, we want to add a directed edge $(C_u, C_v)$ in $E_{SCC}$. A naive insertion can add up to $O(|E|)$ duplicate edges between the same component pairs. Checking for duplicates dynamically using lookup structures (like hash sets or AVL trees) per adjacency list would take $O(|E| \log |V|)$ or $O(|E|)$ expected time, which might be slow or use complex data structures.
*   **The Radix Sort Solution:**
    1.  Compute the SCCs of $G$ in $O(|V| + |E|)$ time (using Kosaraju's or Tarjan's). For each vertex $v \in V$, store its component identifier $comp[v] \in \{1, \dots, |V_{SCC}|\}$.
    2.  Construct a temporary array of edges $L$. For each directed edge $(u, v) \in E$:
        *   If $comp[u] \neq comp[v]$, append the pair $(comp[u], comp[v])$ to $L$.
    3.  Sort the pairs in $L$ using **Radix Sort** (with Counting Sort as the stable sorting primitive) in exactly 2 passes. Since the keys are integers in the range $[1, |V_{SCC}|] \subseteq [1, |V|]$, this sorting takes $O(|V| + |E|)$ time.
    4.  After sorting, all identical pairs appear consecutively in $L$. Do a single linear scan over $L$:
        *   For each pair $(C_a, C_b)$ at index $i$, if $i = 0$ or $(L[i] \neq L[i-1])$, add $C_b$ to the adjacency list of $C_a$.
*   **Complexity:** Takes $O(|V| + |E|)$ time and $O(|V| + |E|)$ auxiliary space, providing a deterministic and simple linear-time construction.

#### 4.5.2 Semi-Connected Graph Detection
*   **Problem:** A directed graph $G = (V, E)$ is *semi-connected* if for every pair of vertices $u, v \in V$, there exists a path $u \rightsquigarrow v$, a path $v \rightsquigarrow u$, or both. Propose an $O(|V| + |E|)$ time algorithm to determine if $G$ is semi-connected.
*   **The Reduction:**
    1.  *Condensation:* Decompose $G$ into its Strongly Connected Components $G_{SCC} = (V_{SCC}, E_{SCC})$ in $O(|V| + |E|)$ time.
    2.  *Property:* $G$ is semi-connected if and only if $G_{SCC}$ is semi-connected.
    3.  *DAG Property:* In a Directed Acyclic Graph (like $G_{SCC}$), a graph is semi-connected if and only if there exists a **Hamiltonian path** (a path that visits every node exactly once).
    4.  *Hamiltonian Path in a DAG:* A DAG contains a Hamiltonian path if and only if its **topological sort is unique**. This is equivalent to checking if there is a directed edge $(v_i, v_{i+1}) \in E_{SCC}$ between every consecutive pair of vertices in the topological sort $v_1, \dots, v_k$.
*   **The Algorithm:**
    1.  Compute $G_{SCC}$ in $O(|V| + |E|)$ time.
    2.  Run Topological Sort on $G_{SCC}$ to get a sequence $v_1, \dots, v_k$.
    3.  Verify that for every $i \in \{1, \dots, k-1\}$, the directed edge $(v_i, v_{i+1})$ exists in $E_{SCC}$.
    4.  If all edges exist, return True (semi-connected); otherwise, return False.
*   **Complexity:** All steps (SCC decomposition, topological sorting, consecutive edge verification) run in **$O(|V| + |E|)$** time.

### 5. Algorithmic Complexity Matrix

This comprehensive guide tracks the absolute time and space complexity baselines for all graph traversal and component decomposition primitives under both standard representations.

|**Algorithm Primitive**|**Adjacency List Time Complexity**|**Adjacency Matrix Time Complexity**|**Space Complexity**|**Primary Exam Use Case / Core Function**|
|---|---|---|---|---|
|**Breadth-First Search (BFS)**|$\Theta(\vert V\vert + \vert E\vert)$|$\Theta(\vert V\vert^2)$|$\Theta(\vert V\vert)$|Finds single-source shortest paths on unweighted graphs; unrolls layer-by-level sets.|
|**Depth-First Search (DFS)**|$\Theta(\vert V\vert + \vert E\vert)$|$\Theta(\vert V\vert^2)$|$\Theta(\vert V\vert)$|Classifies structural back-edges; determines cycle existence; computes baseline discovery/finishing tags.|
|**Topological Sort**|$\Theta(\vert V\vert + \vert E\vert)$|$\Theta(\vert V\vert^2)$|$\Theta(\vert V\vert)$|Linearly orders a DAG's vertices by listing them in strictly reversing order of finishing times $f[u]$.|
|**Kosaraju's SCC Decomposition**|$\Theta(\vert V\vert + \vert E\vert)$|$\Theta(\vert V\vert^2)$|$\Theta(\vert V\vert)$|Isolates strongly connected vertex equivalence clusters via double-pass DFS operations.|
|**Tarjan's SCC Algorithm**|$\Theta(\vert V\vert + \vert E\vert)$|$\Theta(\vert V\vert^2)$|$\Theta(\vert V\vert)$|Decomposes a graph into SCC clusters using a single structural pass via low-link bookkeeping stack mechanisms.|

Let me know when you are ready to generate **Module 3: Weighted Shortest Paths (Dijkstra, Bellman-Ford, Johnson's Algorithm, Potential Reweighting)**!

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]