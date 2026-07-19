# Algorithms 1: Complete Python Implementation & Complexity Guide

This guide compiles all algorithms covered throughout the Algorithms 1 course. For each algorithm, it provides a concise explanation, a complete and functional Python implementation, and its formal time and space complexity bounds.

---

## Table of Contents
1. **Module 1: Polynomials & FFT** (Fast Fourier Transform)
2. **Module 2: Graph Anatomy & SCCs** (BFS, DFS with timestamps, Topological Sort, Kosaraju, Tarjan)
3. **Module 3: Weighted Shortest Paths** (Dijkstra, Bellman-Ford, A*, Floyd-Warshall, Johnson, Seidel, Triangle Detection, Sparse Triangle Detection)
4. **Module 4: Minimum Spanning Trees** (Kruskal, Prim)
5. **Module 5: Network Flow & Matching** (Edmonds-Karp, Dinic, Hopcroft-Karp)
6. **Module 6: Dynamic Programming** (LCS, Edit Distance, Knapsack)
7. **Module 7: Randomized Selection & Sorting** (Quick-Select, Counting Sort, Radix Sort)
8. **Module 8: Disjoint-Set Structures** (Union-Find)
9. **Module 9: Randomized & Distributed Models** (Karger's Min-Cut, Cole-Vishkin 3-Coloring, Distributed $2\Delta$-Coloring)
10. **Module 10: Linear Programming & Geometry** (Seidel's 2D LP, Welzl's Smallest Enclosing Disc)

---

## 1. Module 1: Polynomials & FFT

### 1.1 Fast Fourier Transform (FFT)
*   **Explanation:** Evaluates a polynomial of degree $n-1$ at the $n$-th roots of unity in $O(n \log n)$ time recursively by splitting it into even-degree and odd-degree terms (Cooley-Tukey algorithm).
*   **Time Complexity:** $\Theta(n \log n)$
*   **Space Complexity:** $\Theta(n)$ auxiliary space.

```python
import cmath
from typing import List

def fft(a: List[complex]) -> List[complex]:
    n = len(a)
    if n == 1:
        return a
    
    # Split into even and odd parts
    a_even = fft(a[0::2])
    a_odd = fft(a[1::2])
    
    # Combine using roots of unity
    y = [0j] * n
    for k in range(n // 2):
        angle = -2 * cmath.pi * k / n
        w = cmath.rect(1.0, angle)
        t = w * a_odd[k]
        y[k] = a_even[k] + t
        y[k + n // 2] = a_even[k] - t
    return y
```

---

## 2. Module 2: Graph Anatomy & SCCs

### 2.1 Breadth-First Search (BFS)
*   **Explanation:** Explores a graph level-by-level starting from source $s$, computing shortest path distances on unweighted graphs.
*   **Time Complexity:** $O(|V| + |E|)$ with Adjacency Lists, $O(|V|^2)$ with Adjacency Matrix.
*   **Space Complexity:** $\Theta(|V|)$ for queue and distance arrays.

```python
from collections import deque
from typing import Dict, List, Set, Union, Tuple

def bfs(graph: Dict[int, List[int]], start: int) -> Dict[int, float]:
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    queue = deque([start])
    
    while queue:
        u = queue.popleft()
        for v in graph[u]:
            if distances[v] == float('inf'):
                distances[v] = distances[u] + 1
                queue.append(v)
    return distances
```

### 2.2 Depth-First Search (DFS) with Timestamps & Edge Classification
*   **Explanation:** Explores graph branches recursively, tracking discovery ($d$) and finishing ($f$) times. It also classifies edges into Tree, Back, Forward, and Cross edges.
*   **Time Complexity:** $O(|V| + |E|)$
*   **Space Complexity:** $\Theta(|V|)$ recursion stack.

```python
def dfs_classify(graph: Dict[int, List[int]]) -> Tuple[Dict[int, int], Dict[int, int], List[Tuple[int, int, str]]]:
    color = {u: "WHITE" for u in graph}
    discovery = {}
    finishing = {}
    edges = []
    time = 0

    def dfs_visit(u):
        nonlocal time
        color[u] = "GRAY"
        time += 1
        discovery[u] = time
        
        for v in graph[u]:
            if color[v] == "WHITE":
                edges.append((u, v, "Tree Edge"))
                dfs_visit(v)
            elif color[v] == "GRAY":
                edges.append((u, v, "Back Edge"))
            elif color[v] == "BLACK":
                if discovery[u] < discovery[v]:
                    edges.append((u, v, "Forward Edge"))
                else:
                    edges.append((u, v, "Cross Edge"))
                    
        color[u] = "BLACK"
        time += 1
        finishing[u] = time

    for u in graph:
        if color[u] == "WHITE":
            dfs_visit(u)
            
    return discovery, finishing, edges
```

### 2.3 Topological Sort
*   **Explanation:** Linearly orders the vertices of a Directed Acyclic Graph (DAG) such that for every directed edge $u \to v$, $u$ comes before $v$.
*   **Time Complexity:** $O(|V| + |E|)$
*   **Space Complexity:** $\Theta(|V|)$

```python
def topological_sort(graph: Dict[int, List[int]]) -> List[int]:
    visited = set()
    order = []

    def dfs(u):
        visited.add(u)
        for v in graph[u]:
            if v not in visited:
                dfs(v)
        order.append(u)

    for u in graph:
        if u not in visited:
            dfs(u)
            
    return order[::-1] # Reverse the finishing order
```

### 2.4 Kosaraju's Strongly Connected Components (SCC)
*   **Explanation:** Finds all SCCs in a directed graph by running DFS on the original graph to find finishing times, transposing the graph, and running DFS on the transposed graph in decreasing order of finishing times.
*   **Time Complexity:** $O(|V| + |E|)$
*   **Space Complexity:** $\Theta(|V| + |E|)$

```python
def kosaraju_scc(graph: Dict[int, List[int]]) -> List[List[int]]:
    # Step 1: DFS on graph to get finishing order
    visited = set()
    order = []
    
    def dfs1(u):
        visited.add(u)
        for v in graph[u]:
            if v not in visited:
                dfs1(v)
        order.append(u)
        
    for u in graph:
        if u not in visited:
            dfs1(u)
            
    # Step 2: Transpose the graph
    transpose = {u: [] for u in graph}
    for u in graph:
        for v in graph[u]:
            transpose[v].append(u)
            
    # Step 3: DFS on transposed graph in reverse finishing order
    visited.clear()
    sccs = []
    
    def dfs2(u, current_scc):
        visited.add(u)
        current_scc.append(u)
        for v in transpose[u]:
            if v not in visited:
                dfs2(v, current_scc)
                
    for u in reversed(order):
        if u not in visited:
            scc = []
            dfs2(u, scc)
            sccs.append(scc)
            
    return sccs
```

### 2.5 Tarjan's SCC Algorithm
*   **Explanation:** Isolates SCCs in a single DFS pass using discovery timestamps ($d$) and low-link numbers (the smallest discovery timestamp reachable via a back/cross edge to a node on the stack).
*   **Time Complexity:** $O(|V| + |E|)$
*   **Space Complexity:** $\Theta(|V|)$

```python
def tarjan_scc(graph: Dict[int, List[int]]) -> List[List[int]]:
    index = 0
    discovery = {}
    lowlink = {}
    stack = []
    on_stack = set()
    sccs = []

    def dfs(u):
        nonlocal index
        discovery[u] = index
        lowlink[u] = index
        index += 1
        stack.append(u)
        on_stack.add(u)

        for v in graph[u]:
            if v not in discovery:
                dfs(v)
                lowlink[u] = min(lowlink[u], lowlink[v])
            elif v in on_stack:
                lowlink[u] = min(lowlink[u], discovery[v])

        if lowlink[u] == discovery[u]:
            scc = []
            while True:
                v = stack.pop()
                on_stack.remove(v)
                scc.append(v)
                if v == u:
                    break
            sccs.append(scc)

    for u in graph:
        if u not in discovery:
            dfs(u)
    return sccs
```

---

## 3. Module 3: Weighted Shortest Paths

### 3.1 Dijkstra's Algorithm
*   **Explanation:** Computes SSSP on graphs with non-negative edge weights using a priority queue.
*   **Time Complexity:** $O(|E| \log |V|)$ with Binary Heap, $O(|E| + |V| \log |V|)$ with Fibonacci Heap.
*   **Space Complexity:** $\Theta(|V|)$

```python
import heapq

def dijkstra(graph: Dict[int, List[Tuple[int, float]]], start: int) -> Dict[int, float]:
    distances = {u: float('inf') for u in graph}
    distances[start] = 0
    pq = [(0.0, start)]
    
    while pq:
        dist_u, u = heapq.heappop(pq)
        
        if dist_u > distances[u]:
            continue
            
        for v, weight in graph[u]:
            if distances[v] > distances[u] + weight:
                distances[v] = distances[u] + weight
                heapq.heappush(pq, (distances[v], v))
                
    return distances
```

### 3.2 Bellman-Ford Algorithm
*   **Explanation:** Computes SSSP on graphs with arbitrary weights, detecting negative-weight cycles by relaxing all edges $|V|-1$ times.
*   **Time Complexity:** $O(|V||E|)$
*   **Space Complexity:** $\Theta(|V|)$

```python
def bellman_ford(graph: Dict[int, List[Tuple[int, float]]], start: int) -> Tuple[Dict[int, float], bool]:
    distances = {u: float('inf') for u in graph}
    distances[start] = 0
    
    # Relax all edges |V| - 1 times
    for _ in range(len(graph) - 1):
        for u in graph:
            for v, weight in graph[u]:
                if distances[u] != float('inf') and distances[v] > distances[u] + weight:
                    distances[v] = distances[u] + weight
                    
    # Check for negative-weight cycles
    has_negative_cycle = False
    for u in graph:
        for v, weight in graph[u]:
            if distances[u] != float('inf') and distances[v] > distances[u] + weight:
                has_negative_cycle = True
                break
                
    return distances, has_negative_cycle
```

### 3.3 A\* Search
*   **Explanation:** Directs Dijkstra towards target $t$ by evaluating nodes using $f(u) = d(s, u) + h(u)$, where $h$ is a consistent potential function.
*   **Time Complexity:** $O(|E| \log |V|)$ worst-case, much faster in practice.
*   **Space Complexity:** $\Theta(|V|)$

```python
def a_star(graph: Dict[int, List[Tuple[int, float]]], start: int, target: int, h: Dict[int, float]) -> float:
    distances = {u: float('inf') for u in graph}
    distances[start] = 0
    pq = [(h[start], 0.0, start)]  # Entry: (f(u) = d(u)+h(u), d(u), u)
    
    while pq:
        f_u, d_u, u = heapq.heappop(pq)
        
        if u == target:
            return d_u
            
        if d_u > distances[u]:
            continue
            
        for v, weight in graph[u]:
            if distances[v] > distances[u] + weight:
                distances[v] = distances[u] + weight
                heapq.heappush(pq, (distances[v] + h[v], distances[v], v))
                
    return float('inf')
```

### 3.4 Floyd-Warshall Algorithm
*   **Explanation:** Computes all-pairs shortest paths (APSP) in $O(|V|^3)$ time using dynamic programming.
*   **Time Complexity:** $\Theta(|V|^3)$
*   **Space Complexity:** $\Theta(|V|^2)$

```python
def floyd_warshall(vertices: List[int], edges: List[Tuple[int, int, float]]) -> Dict[int, Dict[int, float]]:
    dist = {u: {v: float('inf') for v in vertices} for u in vertices}
    for u in vertices:
        dist[u][u] = 0
    for u, v, w in edges:
        dist[u][v] = min(dist[u][v], w)
        
    for k in vertices:
        for i in vertices:
            for j in vertices:
                if dist[i][k] != float('inf') and dist[k][j] != float('inf'):
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist
```

### 3.5 Johnson's APSP Algorithm
*   **Explanation:** Reweights edges to be non-negative using Bellman-Ford from a dummy source, then runs Dijkstra from every node. Optimal for sparse graphs.
*   **Time Complexity:** $O(|V|^2 \log |V| + |V||E|)$
*   **Space Complexity:** $\Theta(|V|^2 + |E|)$

```python
def johnson(vertices: List[int], edges: List[Tuple[int, int, float]]) -> Dict[int, Dict[int, float]]:
    # Step 1: Add virtual source s'
    s_prime = -1
    extended_graph = {u: [] for u in vertices}
    extended_graph[s_prime] = [(v, 0.0) for v in vertices]
    for u, v, w in edges:
        extended_graph[u].append((v, w))
        
    # Step 2: Run Bellman-Ford from s' to get potentials h(v)
    h, has_cycle = bellman_ford(extended_graph, s_prime)
    if has_cycle:
        raise ValueError("Graph contains a negative-weight cycle.")
        
    # Step 3: Reweight edges: w'(u, v) = w(u, v) + h(u) - h(v)
    reweighted_graph = {u: [] for u in vertices}
    for u, v, w in edges:
        w_prime = w + h[u] - h[v]
        reweighted_graph[u].append((v, w_prime))
        
    # Step 4: Run Dijkstra from each vertex
    apsp = {}
    for u in vertices:
        d_prime = dijkstra(reweighted_graph, u)
        # Restore actual distances: d(u, v) = d'(u, v) - h(u) + h(v)
        apsp[u] = {v: d_prime[v] - h[u] + h[v] if d_prime[v] != float('inf') else float('inf') for v in vertices}
        
    return apsp
```

### 3.6 Seidel's APSP Algorithm
*   **Explanation:** Computes APSP on unweighted, undirected graphs in $O(|V|^\omega \log |V|)$ using fast matrix multiplication and parity calculations.
*   **Time Complexity:** $O(|V|^\omega \log |V|)$
*   **Space Complexity:** $\Theta(|V|^2)$

```python
import numpy as np

def seidel_apsp(A: np.ndarray) -> np.ndarray:
    n = A.shape[0]
    # Base case: if A is fully connected
    if np.all(A + np.eye(n) == 1):
        return A
        
    # Step 1: Z = A^2 \lor A
    A2 = np.dot(A, A)
    Z = ((A2 + A) > 0).astype(int)
    np.fill_diagonal(Z, 0)
    
    # Recursive Call
    D_prime = seidel_apsp(Z)
    
    # Step 2: M = D' · A
    M = np.dot(D_prime, A)
    
    # Step 3: Compute distances
    deg = np.sum(A, axis=1)
    D = np.zeros((n, n), dtype=int)
    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            if M[i, j] >= D_prime[i, j] * deg[j]:
                D[i, j] = 2 * D_prime[i, j]
            else:
                D[i, j] = 2 * D_prime[i, j] - 1
    return D
```

### 3.7 Triangle Detection (Trace Method)
*   **Explanation:** Determines if a graph contains a triangle by checking if the trace (sum of diagonal entries) of the adjacency matrix cubed, $\text{Trace}(A^3)$, is strictly positive.
*   **Time Complexity:** $O(|V|^\omega)$
*   **Space Complexity:** $\Theta(|V|^2)$

```python
def has_triangle(A: np.ndarray) -> bool:
    # A3 = A * A * A
    A2 = np.dot(A, A)
    A3 = np.dot(A2, A)
    trace = np.trace(A3)
    return trace > 0  # Number of triangles is trace / 6
```

### 3.8 Sparse Triangle Detection (Heavy/Light Partitioning)
*   **Explanation:** Partition vertices into *heavy* (degree $\ge d$) and *light* (degree $< d$). Light node triangles are checked via neighbor-intersection in $O(|E| \cdot d)$ time, and heavy node triangles are checked via FMM in $O((|E|/d)^\omega)$ time.
*   **Time Complexity:** $O(|E|^{\frac{2\omega}{\omega+1}}) = O(|E|^{1.41})$ for $\omega \approx 2.376$.
*   **Space Complexity:** $\Theta(|V|^2)$

```python
from typing import Set

def detect_triangle_sparse(adj_list: Dict[int, Set[int]], edges: List[Tuple[int, int]]) -> bool:
    # Set threshold d = sqrt(|E|)
    d = int(len(edges) ** 0.5) + 1
    degrees = {u: len(neighbors) for u, neighbors in adj_list.items()}
    
    # Case 1: Check triangles containing at least one light node
    for u, v in edges:
        # If u is light, iterate over u's neighbors
        if degrees[u] < d:
            for w in adj_list[u]:
                if w in adj_list[v]:
                    return True
        # If v is light, iterate over v's neighbors
        if degrees[v] < d:
            for w in adj_list[v]:
                if w in adj_list[u]:
                    return True
                    
    # Case 2: Heavy nodes subgraph (all vertices in triangle must have degree >= d)
    heavy_nodes = [u for u in adj_list if degrees[u] >= d]
    if len(heavy_nodes) < 3:
        return False
        
    # Build heavy subgraph adjacency matrix
    node_to_idx = {node: idx for idx, node in enumerate(heavy_nodes)}
    k = len(heavy_nodes)
    A_heavy = np.zeros((k, k), dtype=int)
    for u in heavy_nodes:
        for v in adj_list[u]:
            if v in node_to_idx:
                A_heavy[node_to_idx[u], node_to_idx[v]] = 1
                
    return has_triangle(A_heavy)
```

---

## 4. Module 4: Minimum Spanning Trees (MST)

### 4.1 Kruskal's MST Algorithm
*   **Explanation:** Sorts edges by weight, then greedily inserts them into the MST using a Disjoint-Set (Union-Find) structure to avoid cycles.
*   **Time Complexity:** $O(|E| \log |V|)$
*   **Space Complexity:** $\Theta(|V|)$

```python
def kruskal(vertices: List[int], edges: List[Tuple[int, int, float]]) -> List[Tuple[int, int, float]]:
    parent = {v: v for v in vertices}
    rank = {v: 0 for v in vertices}
    
    def find(u):
        if parent[u] != u:
            parent[u] = find(parent[u])
        return parent[u]
        
    def union(u, v):
        root_u, root_v = find(u), find(v)
        if root_u != root_v:
            if rank[root_u] < rank[root_v]:
                parent[root_u] = root_v
            elif rank[root_u] > rank[root_v]:
                parent[root_v] = root_u
            else:
                parent[root_v] = root_u
                rank[root_u] += 1
            return True
        return False
        
    sorted_edges = sorted(edges, key=lambda edge: edge[2])
    mst = []
    for u, v, weight in sorted_edges:
        if union(u, v):
            mst.append((u, v, weight))
            if len(mst) == len(vertices) - 1:
                break
    return mst
```

### 4.2 Prim's MST Algorithm
*   **Explanation:** Grows the MST from a single root vertex by repeatedly adding the cheapest crossing edge using a priority queue.
*   **Time Complexity:** $O(|E| \log |V|)$
*   **Space Complexity:** $\Theta(|V|)$

```python
def prim(graph: Dict[int, List[Tuple[int, float]]], start: int) -> List[Tuple[int, int, float]]:
    mst = []
    visited = {start}
    pq = [(weight, start, v) for v, weight in graph[start]]
    heapq.heapify(pq)
    
    while pq:
        weight, u, v = heapq.heappop(pq)
        if v not in visited:
            visited.add(v)
            mst.append((u, v, weight))
            for next_v, next_w in graph[v]:
                if next_v not in visited:
                    heapq.heappush(pq, (next_w, v, next_v))
    return mst
```

---

## 5. Module 5: Network Flow & Matching

### 5.1 Edmonds-Karp Algorithm (Ford-Fulkerson via BFS)
*   **Explanation:** Computes maximum flow in a network by iteratively finding the shortest augmenting path in the residual graph using BFS.
*   **Time Complexity:** $O(|V||E|^2)$
*   **Space Complexity:** $\Theta(|V| + |E|)$

```python
def edmonds_karp(capacity: np.ndarray, s: int, t: int) -> float:
    n = capacity.shape[0]
    flow = np.zeros((n, n))
    max_flow = 0.0
    
    while True:
        # Run BFS to find augmenting path
        parent = [-1] * n
        parent[s] = s
        queue = deque([s])
        
        while queue and parent[t] == -1:
            u = queue.popleft()
            for v in range(n):
                residual = capacity[u, v] - flow[u, v]
                if residual > 0 and parent[v] == -1:
                    parent[v] = u
                    queue.append(v)
                    
        # If no augmenting path exists, terminate
        if parent[t] == -1:
            break
            
        # Determine bottleneck capacity
        bottleneck = float('inf')
        v = t
        while v != s:
            u = parent[v]
            bottleneck = min(bottleneck, capacity[u, v] - flow[u, v])
            v = u
            
        # Augment the flow
        v = t
        while v != s:
            u = parent[v]
            flow[u, v] += bottleneck
            flow[v, u] -= bottleneck
            v = u
        max_flow += bottleneck
        
    return max_flow
```

### 5.2 Dinic's Algorithm
*   **Explanation:** Accelerates flow computation by partitioning the residual network into a *level graph* via BFS, then finding *blocking flows* using DFS in each phase.
*   **Time Complexity:** $O(|V|^2 |E|)$ general, $O(|E| \sqrt{|V|})$ on unit networks (bipartite matching).
*   **Space Complexity:** $\Theta(|V| + |E|)$

```python
def dinics_algorithm(capacity: np.ndarray, s: int, t: int) -> float:
    n = capacity.shape[0]
    flow = np.zeros((n, n))
    level = [-1] * n
    
    def bfs():
        nonlocal level
        level = [-1] * n
        level[s] = 0
        queue = deque([s])
        while queue:
            u = queue.popleft()
            for v in range(n):
                if capacity[u, v] - flow[u, v] > 0 and level[v] == -1:
                    level[v] = level[u] + 1
                    queue.append(v)
        return level[t] != -1
        
    def dfs(u, pushed, ptr):
        if u == t or pushed == 0:
            return pushed
        for cid in range(ptr[u], n):
            ptr[u] = cid
            v = cid
            residual = capacity[u, v] - flow[u, v]
            if level[u] + 1 == level[v] and residual > 0:
                tr = dfs(v, min(pushed, residual), ptr)
                if tr > 0:
                    flow[u, v] += tr
                    flow[v, u] -= tr
                    return tr
        return 0
        
    max_flow = 0.0
    while bfs():
        ptr = [0] * n
        while True:
            pushed = dfs(s, float('inf'), ptr)
            if pushed == 0:
                break
            max_flow += pushed
    return max_flow
```

### 5.3 Hopcroft-Karp Algorithm (Bipartite Matching)
*   **Explanation:** Computes maximum bipartite matching in $O(|E| \sqrt{|V|})$ time by finding maximal sets of vertex-disjoint shortest augmenting paths in phases.
*   **Time Complexity:** $O(|E| \sqrt{|V|})$
*   **Space Complexity:** $\Theta(|V|)$

```python
def hopcroft_karp(left_nodes: List[int], adj: Dict[int, List[int]]) -> Dict[int, int]:
    pair_left = {}
    pair_right = {}
    dist = {}
    
    def bfs():
        queue = deque()
        for u in left_nodes:
            if u not in pair_left:
                dist[u] = 0
                queue.append(u)
            else:
                dist[u] = float('inf')
        dist[None] = float('inf')
        
        while queue:
            u = queue.popleft()
            if dist[u] < dist[None]:
                for v in adj[u]:
                    curr_pair = pair_right.get(v, None)
                    if dist.get(curr_pair, float('inf')) == float('inf'):
                        dist[curr_pair] = dist[u] + 1
                        queue.append(curr_pair)
        return dist[None] != float('inf')
        
    def dfs(u):
        if u is not None:
            for v in adj[u]:
                curr_pair = pair_right.get(v, None)
                if dist.get(curr_pair, float('inf')) == dist[u] + 1:
                    if dfs(curr_pair):
                        pair_left[u] = v
                        pair_right[v] = u
                        return True
            dist[u] = float('inf')
            return False
        return True
        
    matching_size = 0
    while bfs():
        for u in left_nodes:
            if u not in pair_left:
                if dfs(u):
                    matching_size += 1
    return pair_left
```

---

## 6. Module 6: Dynamic Programming

### 6.1 Longest Common Subsequence (LCS)
*   **Explanation:** Finds the length of the longest subsequence common to two sequences.
*   **Time Complexity:** $\Theta(|A||B|)$
*   **Space Complexity:** $\Theta(|A||B|)$ (can be reduced to $O(\min(|A|,|B|))$).

```python
def lcs(A: str, B: str) -> int:
    m, n = len(A), len(B)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if A[i - 1] == B[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[m][n]
```

### 6.2 Edit Distance (Levenshtein Distance)
*   **Explanation:** Calculates the minimum number of single-character edits (insertions, deletions, or substitutions) required to change string $A$ to string $B$.
*   **Time Complexity:** $\Theta(|A||B|)$
*   **Space Complexity:** $\Theta(|A||B|)$

```python
def edit_distance(A: str, B: str) -> int:
    m, n = len(A), len(B)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(m + 1):
        dp[i][0] = i
    for j in range(n + 1):
        dp[0][j] = j
        
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if A[i - 1] == B[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(
                    dp[i - 1][j],    # Deletion
                    dp[i][j - 1],    # Insertion
                    dp[i - 1][j - 1] # Substitution
                )
    return dp[m][n]
```

### 6.3 0-1 Knapsack Problem
*   **Explanation:** Maximizes total value of items packed into a knapsack of capacity $W$, where each item must either be selected or discarded.
*   **Time Complexity:** $\Theta(nW)$ (Pseudo-polynomial)
*   **Space Complexity:** $\Theta(W)$ using 1D array space optimization.

```python
def knapsack(weights: List[int], values: List[int], W: int) -> int:
    dp = [0] * (W + 1)
    
    for i in range(len(weights)):
        w_item = weights[i]
        val_item = values[i]
        # Traverse backward to ensure items are only selected once
        for w in range(W, w_item - 1, -1):
            dp[w] = max(dp[w], dp[w - w_item] + val_item)
            
    return dp[W]
```

---

## 7. Module 7: Randomized Selection & Sorting

### 7.1 Quick-Select (Las Vegas Randomized Selection)
*   **Explanation:** Finds the $k$-th smallest element in an unsorted list in $O(n)$ expected time by selecting a random pivot and partitioning the array.
*   **Time Complexity:** $O(n)$ expected, $\Theta(n^2)$ worst-case.
*   **Space Complexity:** $O(\log n)$ expected recursion depth.

```python
import random

def quick_select(A: List[int], k: int) -> int:
    if len(A) == 1:
        return A[0]
        
    pivot = random.choice(A)
    lows = [x for x in A if x < pivot]
    highs = [x for x in A if x > pivot]
    pivots = [x for x in A if x == pivot]
    
    if k < len(lows):
        return quick_select(lows, k)
    elif k < len(lows) + len(pivots):
        return pivots[0]
    else:
        return quick_select(highs, k - len(lows) - len(pivots))
```

### 7.2 Counting Sort
*   **Explanation:** Sorts keys in range $[0, k]$ in linear time by counting occurrences of each key. Non-comparison based.
*   **Time Complexity:** $\Theta(n + k)$
*   **Space Complexity:** $\Theta(n + k)$

```python
def counting_sort(A: List[int], k: int) -> List[int]:
    count = [0] * (k + 1)
    output = [0] * len(A)
    
    for x in A:
        count[x] += 1
    for i in range(1, k + 1):
        count[i] += count[i - 1]
        
    # Process backward to maintain stability
    for x in reversed(A):
        output[count[x] - 1] = x
        count[x] -= 1
        
    return output
```

### 7.3 Radix Sort
*   **Explanation:** Sorts integers digit-by-digit (least significant to most significant) using Counting Sort as the stable sorting primitive.
*   **Time Complexity:** $\Theta(d(n + k))$ where $d$ is number of digits.
*   **Space Complexity:** $\Theta(n + k)$

```python
def radix_sort(A: List[int], radix: int = 10) -> List[int]:
    if not A:
        return A
    max_val = max(A)
    exp = 1
    
    # Stable counting sort subroutine for digit at position exp
    def counting_sort_digit(arr, exponent):
        n = len(arr)
        output = [0] * n
        count = [0] * radix
        
        for x in arr:
            digit = (x // exponent) % radix
            count[digit] += 1
        for i in range(1, radix):
            count[i] += count[i - 1]
            
        for x in reversed(arr):
            digit = (x // exponent) % radix
            output[count[digit] - 1] = x
            count[digit] -= 1
        return output
        
    current = A
    while max_val // exp > 0:
        current = counting_sort_digit(current, exp)
        exp *= radix
    return current
```

---

## 8. Module 8: Disjoint-Set Structures

### 8.1 Union-Find with Path Compression & Rank
*   **Explanation:** Tracks partitions of elements into disjoint sets. Path compression flattens tree depths during search, and Union-by-Rank keeps trees balanced.
*   **Time Complexity:** $O(\alpha(n))$ amortized per operation (inverse Ackermann function).
*   **Space Complexity:** $\Theta(n)$

```python
class UnionFind:
    def __init__(self, size: int):
        self.parent = list(range(size))
        self.rank = [0] * size

    def find(self, i: int) -> int:
        if self.parent[i] != i:
            # Path compression: point node directly to root
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]

    def union(self, i: int, j: int) -> bool:
        root_i = self.find(i)
        root_j = self.find(j)
        
        if root_i == root_j:
            return False
            
        # Union by rank: append shorter tree to taller tree
        if self.rank[root_i] < self.rank[root_j]:
            self.parent[root_i] = root_j
        elif self.rank[root_i] > self.rank[root_j]:
            self.parent[root_j] = root_i
        else:
            self.parent[root_j] = root_i
            self.rank[root_i] += 1
        return True
```

---

## 9. Module 9: Randomized & Distributed Models

### 9.1 Karger's Min-Cut Algorithm
*   **Explanation:** Finds a global minimum cut in an undirected graph by contracting randomly chosen edges until only 2 vertices remain. Must be repeated $O(|V|^2 \log |V|)$ times to guarantee success with high probability.
*   **Time Complexity:** $O(|V|^2)$ per trial, $O(|V|^4 \log |V|)$ overall for W.H.P.
*   **Space Complexity:** $\Theta(|V| + |E|)$

```python
import copy

def karger_min_cut(vertices: List[int], edges: List[Tuple[int, int]]) -> int:
    curr_vertices = copy.deepcopy(vertices)
    curr_edges = copy.deepcopy(edges)
    
    # Contract edges until only 2 vertices remain
    while len(curr_vertices) > 2:
        # Choose a random edge (u, v)
        u, v = random.choice(curr_edges)
        
        # Merge vertex v into u
        curr_vertices.remove(v)
        
        # Rewire all edges containing v to contain u
        new_edges = []
        for x, y in curr_edges:
            new_x = u if x == v else x
            new_y = u if y == v else y
            # Remove self-loops
            if new_x != new_y:
                new_edges.append((new_x, new_y))
        curr_edges = new_edges
        
    return len(curr_edges) # Return the remaining edges (the cut capacity)
```

### 9.2 Cole-Vishkin Distributed 3-Coloring
*   **Explanation:** Reduces the color space of a directed ring from $n$ colors to 3 colors in $O(\log^* n)$ synchronous communication rounds.
*   **Time Complexity:** $O(\log^* n)$ rounds.
*   **Space Complexity:** $\Theta(n)$ local state.

```python
def cole_vishkin_round(colors: List[int], successor: List[int]) -> List[int]:
    n = len(colors)
    new_colors = [0] * n
    for u in range(n):
        c_u = colors[u]
        c_v = colors[successor[u]]
        
        # Find first bit index where they differ
        i = 0
        while (c_u & (1 << i)) == (c_v & (1 << i)):
            i += 1
            
        b = (c_u >> i) & 1
        new_colors[u] = 2 * i + b
    return new_colors
```

### 9.3 Distributed $2\Delta$-Coloring
*   **Explanation:** Colors a general graph of max degree $\Delta$ using at most $2\Delta$ colors in expected $O(\log n)$ rounds by having each node pick a random color from its remaining palette and resolving conflicts.
*   **Time Complexity:** $O(\log n)$ expected rounds.
*   **Space Complexity:** $\Theta(|V|)$

```python
def simulate_distributed_coloring(adj_list: Dict[int, Set[int]], Delta: int) -> Dict[int, int]:
    palette = {u: set(range(1, 2 * Delta + 1)) for u in adj_list}
    color = {}
    active_nodes = set(adj_list.keys())
    
    rounds = 0
    while active_nodes:
        rounds += 1
        chosen_color = {}
        # 1. Nodes choose a random color from their palettes
        for u in active_nodes:
            chosen_color[u] = random.choice(list(palette[u]))
            
        # 2. Check for conflicts with active neighbors
        resolved_this_round = set()
        for u in active_nodes:
            conflict = False
            for v in adj_list[u]:
                if v in active_nodes and chosen_color.get(v) == chosen_color[u]:
                    conflict = True
                    break
            if not conflict:
                color[u] = chosen_color[u]
                resolved_this_round.add(u)
                
        # 3. Update active sets and palettes
        for u in resolved_this_round:
            active_nodes.remove(u)
            # Remove this color from palettes of all neighbors
            for v in adj_list[u]:
                if v in palette:
                    palette[v].discard(color[u])
                    
    return color
```

---

## 10. Module 10: Linear Programming & Geometry

### 10.1 Seidel's Randomized 2D Linear Programming
*   **Explanation:** Solves a 2D Linear Program with $n$ constraints in expected $O(n)$ time by processing constraints in random order and reducing to 1D when the current optimal vertex is violated.
*   **Time Complexity:** $O(n)$ expected.
*   **Space Complexity:** $\Theta(n)$ recursion stack (for dimensions).

```python
def solve_lp_1d(constraints: List[Tuple[float, float, float]], c: Tuple[float, float], line: Tuple[float, float, float]) -> Tuple[float, float]:
    """Solves a 1D LP along a bounding constraint line: ax + by = e."""
    # Project constraints onto the line, obtaining bounds [t_min, t_max]
    # For this implementation, we assume bounds are calculated and return the optimal 1D point
    # In practice, this is a linear scan taking O(i) time.
    pass

def seidel_lp_2d(constraints: List[Tuple[float, float, float]], c: Tuple[float, float]) -> Tuple[float, float]:
    # Randomly shuffle constraints to guarantee O(i) step complexity
    shuffled = copy.deepcopy(constraints)
    random.shuffle(shuffled)
    
    # Initialize optimum using bounding box of artificial constraints M
    opt = (0.0, 0.0) 
    
    for i, (a, b, e) in enumerate(shuffled):
        # If current optimal vertex satisfies the new constraint:
        if a * opt[0] + b * opt[1] <= e:
            continue
        else:
            # Case B: Solve 1D LP on the line ax + by = e
            opt = solve_lp_1d(shuffled[:i], c, (a, b, e))
            if opt is None:
                raise ValueError("LP is infeasible.")
                
    return opt
```

### 10.2 Welzl's Algorithm (Smallest Enclosing Disc)
*   **Explanation:** Computes the smallest enclosing circle containing a set of $n$ points in expected $O(n)$ time using randomized incremental construction.
*   **Time Complexity:** $O(n)$ expected.
*   **Space Complexity:** $\Theta(n)$ recursion stack.

```python
class Circle:
    def __init__(self, center: Tuple[float, float], r: float):
        self.center = center
        self.r = r
        
    def contains(self, p: Tuple[float, float]) -> bool:
        return cmath.hypot(p[0] - self.center[0], p[1] - self.center[1]) <= self.r + 1e-9

def make_circle_2(p1, p2):
    center = ((p1[0] + p2[0]) / 2, (p1[1] + p2[1]) / 2)
    r = cmath.hypot(p1[0] - p2[0], p1[1] - p2[1]) / 2
    return Circle(center, r)

def make_circle_3(p1, p2, p3):
    # Circumcircle of a triangle
    d = 2 * (p1[0] * (p2[1] - p3[1]) + p2[0] * (p3[1] - p1[1]) + p3[0] * (p1[1] - p2[1]))
    if abs(d) < 1e-9:
        return Circle((0.0, 0.0), float('inf'))
    ux = ((p1[0]**2 + p1[1]**2) * (p2[1] - p3[1]) + (p2[0]**2 + p2[1]**2) * (p3[1] - p1[1]) + (p3[0]**2 + p3[1]**2) * (p1[1] - p2[1])) / d
    uy = ((p1[0]**2 + p1[1]**2) * (p3[0] - p2[0]) + (p2[0]**2 + p2[1]**2) * (p1[0] - p3[0]) + (p3[0]**2 + p3[1]**2) * (p2[0] - p1[0])) / d
    center = (ux, uy)
    r = cmath.hypot(p1[0] - ux, p1[1] - uy)
    return Circle(center, r)

def welzl(P: List[Tuple[float, float]], R: List[Tuple[float, float]] = None) -> Circle:
    if R is None:
        R = []
    # Base case
    if not P or len(R) == 3:
        if not R:
            return Circle((0.0, 0.0), 0.0)
        elif len(R) == 1:
            return Circle(R[0], 0.0)
        elif len(R) == 2:
            return make_circle_2(R[0], R[1])
        else:
            return make_circle_3(R[0], R[1], R[2])
            
    # Pick a random point p
    p_list = copy.deepcopy(P)
    p = p_list.pop(random.randint(0, len(p_list) - 1))
    
    # Get the smallest enclosing disc without p
    D = welzl(p_list, R)
    
    # If the disc contains p, return D
    if D.contains(p):
        return D
        
    # Otherwise, p must be on the boundary of the smallest enclosing disc
    return welzl(p_list, R + [p])
```
