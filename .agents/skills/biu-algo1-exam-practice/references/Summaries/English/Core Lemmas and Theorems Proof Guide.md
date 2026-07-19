---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-07-11
---
# Core Lemmas & Theorems Proof Guide

**MOC:** [[Algorithms 1 MOC]]  
**Course:** [[Algorithms 1]]  

---

This guide contains the mathematical proofs, derivations, and structural justifications for all **74 lemmas and theorems** of the **Algorithms 1 (89220)** curriculum.

---

## 1. Polynomials & Fast Fourier Transform (FFT) / פולינומים והתמרת פורייה מהירה (FFT)

### 1.1 Roots of Unity are Roots / שורשי היחידה הם שורשים
*   **Statement:** For all $0 \le k < n$, $\omega_n^k = e^{2\pi i k / n}$ satisfies $(\omega_n^k)^n = 1$.
*   **Proof:** By Euler's formula and exponentiation rules:
    $$(\omega_n^k)^n = \left( e^{2\pi i k / n} \right)^n = e^{2\pi i k} = \cos(2\pi k) + i\sin(2\pi k)$$
    Since $k$ is an integer, $\cos(2\pi k) = 1$ and $\sin(2\pi k) = 0$. Thus, $(\omega_n^k)^n = 1$. $\blacksquare$

### 1.2 Weak Negation / שלילה חלשה
*   **Statement:** $\omega_n^{n/2 + k} = -\omega_n^k$.
*   **Proof:** 
    $$\omega_n^{n/2 + k} = \omega_n^{n/2} \cdot \omega_n^k = e^{2\pi i (n/2)/n} \cdot \omega_n^k = e^{\pi i} \cdot \omega_n^k = -1 \cdot \omega_n^k = -\omega_n^k \quad \blacksquare$$

### 1.3 Halving Lemma / למת החצייה
*   **Statement:** If $n$ is even, the squares of the $n$ roots of unity of order $n$ are exactly the $n/2$ roots of unity of order $n/2$: $(\omega_n^k)^2 = \omega_{n/2}^k$.
*   **Proof:** 
    $$\left( \omega_n^k \right)^2 = \left( e^{2\pi i k / n} \right)^2 = e^{2\pi i k / (n/2)} = \omega_{n/2}^k \quad \blacksquare$$

### 1.4 Strong Negation / שלילה חזקה
*   **Statement:** Roots of unity of order $2^k$ satisfy weak negation and halving recursively at all levels.
*   **Proof:** Follows by induction on the recursion depth. Since $n$ is a power of 2, $n/2^j$ is even for all levels of recursion until $n=1$, ensuring that the halving lemma conditions hold at every step. $\blacksquare$

### 1.5 Distinct Roots of Unity / שורשי יחידה שונים
*   **Statement:** $\omega_n^0, \dots, \omega_n^{n-1}$ are $n$ distinct complex numbers.
*   **Proof:** Suppose $\omega_n^j = \omega_n^k$ for $0 \le j < k < n$. Then:
    $$\omega_n^{k - j} = 1 \implies e^{2\pi i (k-j)/n} = 1 \implies \frac{2\pi(k-j)}{n} = 2\pi m \implies k-j = m n$$
    Since $0 < k - j < n$, this is a contradiction. $\blacksquare$

### 1.6 Correctness of FFT / נכונות אלגוריתם FFT
*   **Statement:** $A(x) = A^{[0]}(x^2) + x A^{[1]}(x^2)$ evaluates the polynomial on all $n$ roots.
*   **Proof:** Let $x = \omega_n^k$. For $k < n/2$:
    $$A(\omega_n^k) = A^{[0]}(\omega_{n/2}^k) + \omega_n^k A^{[1]}(\omega_{n/2}^k)$$
    For $k \ge n/2$ (let $k = j + n/2$):
    $$A(\omega_n^{j+n/2}) = A^{[0]}(\omega_{n/2}^j) - \omega_n^j A^{[1]}(\omega_{n/2}^j)$$
    This matches the butterfly update equations. $\blacksquare$

### 1.7 Correctness of IFFT / נכונות אלגוריתם IFFT
*   **Statement:** $a_j = \frac{1}{n} \sum_{k=0}^{n-1} y_k \omega_n^{-jk}$ reconstructs the coefficients.
*   **Proof:** This is equivalent to showing that the inverse of the Vandermonde matrix $V_n(\omega_n)$ is $\frac{1}{n} V_n(\omega_n^{-1})$. The $(j, c)$ entry of their product is:
    $$\frac{1}{n} \sum_{k=0}^{n-1} \omega_n^{jk} \omega_n^{-kc} = \frac{1}{n} \sum_{k=0}^{n-1} \omega_n^{k(j-c)}$$
    If $j = c$, the sum is $\frac{1}{n} \cdot n = 1$. If $j \neq c$, it is a geometric series summing to 0. $\blacksquare$

---

## 2. Minimum Spanning Trees (MST) / עצי פריסה מינימליים (MST)

### 2.1 Cut Property / Greedy Choice / תכונת החתך / בחירה גרידית
*   **Statement:** Let $(S, V-S)$ be a cut respecting $A \subseteq \text{MST}$. If $e$ is a light edge crossing the cut, then $A \cup \{e\} \subseteq \text{MST}$.
*   **Proof:** If $e \notin T$ (where $T \supseteq A$ is an MST), then $T \cup \{e\}$ contains a cycle. This cycle crosses the cut at some other edge $e'$. Since $e$ is light, $w(e) \le w(e')$. Replacing $e'$ with $e$ yields a new tree $T' = T - \{e'\} \cup \{e\}$ of weight $w(T') \le w(T)$. Since $T$ is an MST, $w(T') = w(T)$, so $T'$ is also an MST containing $A \cup \{e\}$. $\blacksquare$

### 2.2 Exchange Lemma / למת החילוף
*   **Statement:** Adding a light edge $e$ crossing a cut to a spanning tree $T$, and removing an edge $e'$ on the resulting cycle that crosses the same cut, yields another spanning tree $T'$ of equal or lesser weight.
*   **Proof:** Edge $e$ links $S$ and $V-S$. The path in $T$ between $e$'s endpoints must cross the cut at some edge $e'$. Replacing $e'$ with $e$ removes the cycle and maintains connectivity, yielding a valid spanning tree. Since $e$ is light, $w(e) \le w(e') \implies w(T') \le w(T)$. $\blacksquare$

### 2.3 Optimal Substructure in Contraction / תת-מבנה אופטימלי בכיווץ
*   **Statement:** If $e$ is a light edge of $G$, and $T'$ is an MST of the contracted graph $G/e$, then $T' \cup \{e\}$ is an MST of $G$.
*   **Proof:** Any spanning tree of $G$ containing $e$ maps to a spanning tree of $G/e$ with weight $w(T) - w(e)$. Minimizing $w(T)$ is equivalent to minimizing $w(T/e)$. Since $T'$ is optimal for $G/e$, $T' \cup \{e\}$ is optimal for $G$. $\blacksquare$

### 2.4 Correctness of Generic MST / נכונות אלגוריתם MST גנרי
*   **Statement:** Repeatedly choosing a safe edge and contracting returns a valid MST.
*   **Proof:** Follows by induction using the Cut Property (which proves the safe edge is in some MST) and the Contraction Lemma. $\blacksquare$

### 2.5 Tree Criterion / קריטריון העץ
*   **Statement:** An undirected graph with $|V|-1$ edges is a tree if and only if it is connected or acyclic.
*   **Proof:** If it is connected and has $|V|-1$ edges, removing any cycle edge would maintain connectivity with fewer edges (impossible). If it is acyclic and has $|V|-1$ edges, the number of connected components is $|V| - |E| = 1$. $\blacksquare$

### 2.6 Kruskal Avoids Cycles / קרוסקל מונע מעגלים
*   **Statement:** An edge $e = (u, v)$ creates a cycle if and only if $u$ and $v$ are already in the same connected component.
*   **Proof:** A cycle is a path from a vertex back to itself. If $u$ and $v$ belong to the same tree component, there exists a path between them. Adding $(u, v)$ completes the cycle. $\blacksquare$

### 2.7 Prim Selects Safe Edges / פריים בוחר קשתות בטוחות
*   **Statement:** Prim's algorithm always relaxes and extracts the lightest edge crossing the cut $(S, V-S)$, where $S$ is the set of vertices currently in the tree.
*   **Proof:** The priority queue stores $key[v] = \min_{u \in S} w(u, v)$. Extract-Min selects $v \notin S$ minimizing $key[v]$, which is exactly the lightest edge crossing the cut. $\blacksquare$

### 2.8 Advanced Spanning Tree Proofs / הוכחות מתקדמות לעצי פריסה

#### 2.8.1 Boruvka Cycle-Free Selection / בחירה ללא מעגלים של בורובקה
*   **Statement:** In any phase of Boruvka's algorithm, choosing the lightest incident edge for each tree component under a lexicographically extended total order $\preccurlyeq$ cannot introduce cycles.
*   **Proof:** Suppose for contradiction that a cycle of components $C_1 \to C_2 \to \dots \to C_k \to C_1$ is formed. Let $e_i$ be the edge chosen by component $C_i$ to connect to $C_{i+1}$ (with $C_{k+1} = C_1$).
    *   Since $C_i$ chose $e_i$ instead of $e_{i-1}$, and $e_{i-1}$ is also incident to $C_i$, the total order implies $e_i \prec e_{i-1}$ for all $i \in [1, k]$ (where $e_0 = e_k$).
    *   This gives a chain of strict inequalities:
        $$e_1 \prec e_k \prec e_{k-1} \prec \dots \prec e_1$$
        which implies $e_1 \prec e_1$, a contradiction. Thus, no cycles can be formed. $\blacksquare$

#### 2.8.2 Yao's MST Complexity / סיבוכיות אלגוריתם MST של יאו
*   **Statement:** Yao's algorithm runs in $O(|E| \log \log |V| + |V| \log |V|)$ time.
*   **Proof:** 
    1.  Partitioning each vertex's incident edges $E_v$ into $k$ blocks of equal size using linear-time QuickSelect takes $\sum_{v \in V} O(|E_v| \log k) = O(|E| \log k)$ time.
    2.  In each of the $\log \log |V|$ phases, for each active component, we only scan the current active block $E_v^{(i)}$. The expected size of scanned edges per phase is:
        $$\sum_{v \in V} O\left( \frac{|E_v|}{k} \right) = O\left( \frac{|E|}{k} \right)$$
    3.  Summing over all $\log \log |V|$ phases, the total scan time is $O\left( \frac{|E| \log \log |V|}{k} \right)$.
    4.  Setting $k = \log |V|$ minimizes the total cost:
        $$\text{Total Time} = \underbrace{O(|E| \log k)}_{\text{Partitioning}} + \underbrace{O\left( \frac{|E| \log \log |V|}{k} \right) + O(|V| \log |V|)}_{\text{Boruvka phases}} = \mathbf{O(|E| \log \log |V| + |V| \log |V|)} \quad \blacksquare$$

#### 2.8.3 KKT Randomized MST Complexity / סיבוכיות אלגוריתם MST האקראי של KKT
*   **Statement:** The Karger-Klein-Tarjan (KKT) randomized MST algorithm runs in expected $O(|V| + |E|)$ time.
*   **Proof:** Let $T(V, E)$ be the expected runtime.
    1.  Performing 3 iterations of Boruvka shrinks the graph to $V_1 = V/8$ and $E_1 \le E$ in $O(|V| + |E|)$ time.
    2.  Sampling $2|V_1|$ edges uniformly to solve $F_2 = \text{MST-KKT}(G_2)$ takes $T(|V|/8, |V|/4)$ expected time.
    3.  Filtering $F_2$-heavy edges takes $O(|V_1| + |E_1|) = O(|V| + |E|)$ time. The probability that an arbitrary edge $e \in E_1$ is $F_2$-light is at most:
        $$\Pr(e \text{ is } F_2\text{-light}) \le \frac{|V_1| - 1}{|E_2|} \le \frac{|V|/8}{|V|/4} = \frac{1}{2}$$
    4.  The expected number of $F_2$-light edges is at most $|E_1|/2 \le |E|/2$.
    5.  Running KKT recursively on $G_3(V_1, E_3)$ takes $T(|V|/8, |E|/2)$ expected time.
    6.  The recurrence relation is:
        $$T(V, E) = T\left(\frac{|V|}{8}, \frac{|V|}{4}\right) + T\left(\frac{|V|}{8}, \frac{|E|}{2}\right) + O(|V| + |E|)$$
        By induction, $T(V, E) \le c(|V| + |E|)$ for a sufficiently large constant $c$, yielding expected **$O(|V| + |E|)$** time. $\blacksquare$

#### 2.9 Fibonacci Heap Amortized Proofs / הוכחות עלויות אמוטיזציה לערימת פיבונאצ'י
Let the potential function be $\Phi = t + 2m$, where $t$ is the number of trees in the root list, and $m$ is the number of marked nodes.
*   **Lemma 2.9.1: Amortized Cost of Insert**
    *   *Statement:* The amortized cost of inserting a node into a Fibonacci Heap is $O(1)$.
    *   *Proof:* The actual cost is $O(1)$ to create the node and link it into the root list.
        The number of trees increases by 1: $t' = t + 1$. The number of marked nodes is unchanged: $m' = m$.
        The change in potential is:
        $$\Delta \Phi = (t' + 2m') - (t + 2m) = (t + 1 + 2m) - (t + 2m) = 1$$
        The amortized cost is:
        $$C_{\text{amortized}} = C_{\text{actual}} + \Delta \Phi = O(1) + 1 = \mathbf{O(1)} \quad \blacksquare$$

*   **Lemma 2.9.2: Amortized Cost of Decrease-Key**
    *   *Statement:* The amortized cost of decrease-key is $O(1)$.
    *   *Proof:* 
        1. Let $c$ be the number of cascading cuts performed. Each cut takes $O(1)$ actual time, so the total actual cost is $O(c)$.
        2. Let's analyze the potential change:
           - Each of the $c$ cuts removes a node (which was a child) and makes it a root, increasing the number of trees in the root list by $c$: $t' = t + c$.
           - The first $c-1$ nodes cut were already marked. Cutting them unmarks them, reducing the count of marked nodes by $c-1$.
           - The $c$-th node cut (which was the first unmarked parent node in the chain) is now marked, increasing the marked count by 1.
           - Thus, the new number of marked nodes is at most:
             $$m' \le m - (c - 1) + 1 = m - c + 2$$
        3. The change in potential is:
           $$\Delta \Phi = (t' + 2m') - (t + 2m) \le (t + c + 2(m - c + 2)) - (t + 2m) = c - 2c + 4 = 4 - c$$
        4. The amortized cost is:
           $$C_{\text{amortized}} = C_{\text{actual}} + \Delta \Phi \le O(c) + (4 - c) = \mathbf{O(1)} \quad \blacksquare$$

*   **Lemma 2.9.3: Amortized Cost of Extract-Min**
    *   *Statement:* The amortized cost of extract-min is $O(D(n))$ where $D(n) = O(\log n)$ is the maximum degree of any node in the heap.
    *   *Proof:*
        1. Removing `min` takes $O(1)$ time. Moving `min`'s children (at most $D(n)$ children) to the root list takes $O(D(n))$ actual time. The number of roots becomes at most $t + D(n) - 1$.
        2. Consolidating the roots list:
           - Suppose we start consolidation with $R \le t + D(n) - 1$ roots.
           - Each merge reduces the number of trees by 1, taking $O(1)$ time. If we perform $M$ merges, the actual work of merging is $O(M)$.
           - The number of remaining trees after consolidation is at most $D(n) + 1$ (since no two trees share the same degree).
           - Thus, the final number of trees is $t' \le D(n) + 1$. The number of merges satisfies $R - M = t' \implies M = R - t'$.
           - The total actual work of consolidation is proportional to the number of roots scanned: $O(R) = O(t + D(n))$.
        3. The number of marked nodes does not increase: $m' \le m$.
        4. The change in potential is:
           $$\Delta \Phi = (t' + 2m') - (t + 2m) \le D(n) + 1 + 2m - t - 2m = D(n) + 1 - t$$
        5. The amortized cost is:
           $$C_{\text{amortized}} = C_{\text{actual}} + \Delta \Phi \le c_1(t + D(n)) + (D(n) + 1 - t)$$
           For a suitable scaling of the potential function unit, the $t$ term cancels out:
           $$C_{\text{amortized}} = \mathbf{O(D(n))} = \mathbf{O(\log n)} \quad \blacksquare$$

---


## 3. BFS (Breadth-First Search) / חיפוש לרוחב (BFS)

### 3.1 Triangle Inequality (Unweighted) / אי-שוויון המשולש (לא משוקלל)
*   **Statement:** For any edge $(u, v) \in E$, $\delta(s, v) \le \delta(s, u) + 1$.
*   **Proof:** If there is a shortest path from $s$ to $u$ of length $\delta(s, u)$, appending $(u, v)$ creates a path of length $\delta(s, u) + 1$. The shortest path to $v$ can be no longer than this path. $\blacksquare$

### 3.2 Upper-Bound Property (BFS) / תכונת החסם העליון (BFS)
*   **Statement:** Throughout BFS, $d[v] \ge \delta(s, v)$ for all $v \in V$.
*   **Proof:** Holds initially ($d[s]=0 = \delta(s,s)$, and $d[v]=\infty \ge \delta(s,v)$). By induction, when we discover $v$ from $u$, we set $d[v] = d[u]+1$. By inductive hypothesis, $d[u] \ge \delta(s, u)$, so $d[v] \ge \delta(s, u)+1 \ge \delta(s, v)$ (via Triangle Inequality). $\blacksquare$

### 3.3 Queue Lemma / למת התור
*   **Statement:** Let $Q = \langle v_1, v_2, \dots, v_r \rangle$ be the BFS queue. Then $d[v_1] \le d[v_2] \le \dots \le d[v_r]$ and $d[v_r] \le d[v_1] + 1$.
*   **Proof:** Follows by induction on queue operations. When a vertex $u$ is dequeued, we enqueue its unvisited neighbors $v$ setting $d[v] = d[u]+1$. Since $d[u]$ is non-decreasing, the enqueued values remain sorted and within a difference of 1. $\blacksquare$

### 3.4 Correctness of BFS / נכונות אלגוריתם BFS
*   **Statement:** On BFS completion, $d[v] = \delta(s, v)$ for all reachable $v$.
*   **Proof:** Suppose for contradiction that some vertices have $d[v] > \delta(s, v)$. Let $v$ be the one with the minimum $\delta(s, v)$. Let $u$ be the predecessor of $v$ on the shortest path. Thus, $\delta(s, v) = \delta(s, u) + 1$. Since $u$ has a smaller shortest path distance, $d[u] = \delta(s, u)$. When $u$ was dequeued, $v$ was either:
    1. Unvisited: then we set $d[v] = d[u] + 1 = \delta(s, v)$ (contradiction).
    2. Visited and in queue/closed: then $d[v] \le d[u]+1$ (by the Queue Lemma), so $d[v] \le \delta(s, v)$ (contradiction). $\blacksquare$

### 3.5 Predecessor Subgraph is a Shortest-Path Tree / תת-גרף האבות הוא עץ מסלולים קצרים
*   **Statement:** The edges $(\pi[v], v)$ form a tree rooted at $s$ containing shortest paths.
*   **Proof:** Since $d[v] = d[\pi[v]] + 1$ for all $v \neq s$, the path to $v$ along $\pi$ has length exactly $d[v] = \delta(s, v)$. $\blacksquare$

---

## 4. DFS (Depth-First Search) / חיפוש לעומק (DFS)

### 4.1 Parenthesis Theorem / משפט הסוגריים
*   **Statement:** For any two vertices $u$ and $v$, the intervals $[d[u], f[u]]$ and $[d[v], f[v]]$ are either entirely disjoint or one is completely nested within the other.
*   **Proof:** Assume $d[u] < d[v]$.
    *   If $d[v] < f[u]$: Since $v$ is discovered while $u$ is active, DFS must finish $v$ (and all its descendants) before backtracking to finish $u$. Hence, $f[v] < f[u]$, meaning $[d[v], f[v]] \subset [d[u], f[u]]$.
    *   If $d[v] > f[u]$: Then the intervals are completely disjoint. $\blacksquare$

### 4.2 White-Path Theorem / משפט המסלול הלבן
*   **Statement:** $v$ is a descendant of $u$ in DFS iff at time $d[u]$ there is a path $u \rightsquigarrow v$ of white vertices.
*   **Proof:** 
    *   **$\implies$:** If $v$ is a descendant, the tree path from $u$ to $v$ is active after $d[u]$, so all its vertices must have been undiscovered (white) at time $d[u]$.
    *   **$\impliedby$:** Let $P$ be a white path. Assume for contradiction that $v$ is not a descendant. Let $w$ be the first vertex on $P$ that is not a descendant of $u$, and let $t$ be its predecessor. $t$ is a descendant, so $f[t] \le f[u]$. Since $w$ is white at $d[u]$, $d[w] > d[u]$. Since it is adjacent to $t$, we must discover $w$ before finishing $t$, so $d[w] < f[t]$. Thus, $d[u] < d[w] < f[t] \le f[u] \implies [d[w], f[w]] \subset [d[u], f[u]]$, meaning $w$ is a descendant (contradiction). $\blacksquare$

### 4.3 Edge Classification by Timestamps / סיווג קשתות לפי זמני גילוי וסיום
*   **Statement:** An edge $(u, v)$ is:
    *   **Tree/Forward Edge:** iff $d[u] < d[v] < f[v] < f[u]$.
    *   **Back Edge:** iff $d[v] \le d[u] < f[u] \le f[v]$.
    *   **Cross Edge:** iff $d[v] < f[v] < d[u] < f[u]$.
*   **Proof:** Follows directly from the Parenthesis Theorem and active status of vertices during the traversal. $\blacksquare$

### 4.4 Cycle iff Back Edge / מעגל אם ורק אם קשת אחורית
*   **Statement:** A directed graph has a cycle iff DFS finds a back edge.
*   **Proof:** 
    *   **$\impliedby$:** A back edge $(u, v)$ means $v$ is an ancestor of $u$. The tree path $v \rightsquigarrow u$ together with $(u, v)$ forms a cycle.
    *   **$\implies$:** Let $C$ be a cycle, and let $v$ be the first vertex of $C$ discovered by DFS. For any other vertex $u \in C$, there is a path $v \rightsquigarrow u$ along the cycle. Since $v$ is the first discovered, all vertices on this path are white. By the White-Path Theorem, $u$ becomes a descendant of $v$. The edge $(u, v)$ on the cycle is therefore a back edge because it points from descendant $u$ to ancestor $v$. $\blacksquare$

### 4.5 Topological Sort Correctness / נכונות מיון טופולוגי
*   **Statement:** In a DAG, ordering vertices by decreasing finishing times yields a topological sort.
*   **Proof:** For any edge $(u, v)$, we must show $f[u] > f[v]$. When $(u, v)$ is explored:
    *   If $v$ is white, it becomes a descendant of $u$, so $f[v] < f[u]$.
    *   If $v$ is gray, then $(u, v)$ is a back edge, which is impossible in a DAG.
    *   If $v$ is black, it is already finished, so $f[v] < d[u] < f[u]$. $\blacksquare$

---

## 5. DFS & SCC / רכיבים קשירים חזק (SCC) וחיפוש לעומק

### 5.1 Component Graph is a DAG / גרף הרכיבים הוא DAG
*   **Statement:** The condensation graph $G^{SCC}$ has no cycles.
*   **Proof:** If there were a cycle $C_1 \to C_2 \to \dots \to C_1$ between different components, then all vertices in these components would be mutually reachable. They would merge into a single SCC, contradicting that they were distinct. $\blacksquare$

### 5.2 Finishing Times Between Components / זמני סיום בין רכיבים
*   **Statement:** If there is an edge $u \to v$ where $u \in C_1$ and $v \in C_2$, then $f(C_1) > f(C_2)$, where $f(C) = \max_{x \in C} f[x]$.
*   **Proof:** 
    *   Case 1 ($d(C_1) < d(C_2)$): The first discovered vertex in $C_1$ can reach all of $C_2$ via white paths. Thus, all of $C_2$ finishes before $C_1$ finishes, so $f(C_1) > f(C_2)$.
    *   Case 2 ($d(C_1) > d(C_2)$): Since $G^{SCC}$ is a DAG, there is no path from $C_2$ to $C_1$. Thus, $C_2$ finishes completely without reaching $C_1$, so $f(C_1) > f(C_2)$. $\blacksquare$

### 5.3 Correctness of Kosaraju / נכונות אלגוריתם קוסראג'ו
*   **Statement:** DFS on $G^T$ in decreasing order of finishing times yields trees matching SCCs.
*   **Proof:** By the finishing time property, the vertex with the maximum finishing time must belong to a source component in $G^{SCC}$, which is a sink component in $(G^T)^{SCC}$. Running DFS on $G^T$ starting here will only traverse this component (as it cannot escape a sink component). Once cleared, the next remaining vertex of maximum finishing time belongs to the next sink component, and so on. $\blacksquare$

---

## 6. Shortest Paths Basics / יסודות מסלולים קצרים

### 6.1 Subpath of a Shortest Path / תת-מסלול של מסלול קצר הוא מסלול קצר
*   **Statement:** Any subpath of a shortest path is a shortest path.
*   **Proof:** Let $P = u \rightsquigarrow x \rightsquigarrow y \rightsquigarrow v$ be a shortest path. If there were a shorter path $P'_{xy}$ from $x$ to $y$, we could replace $x \rightsquigarrow y$ with $P'_{xy}$ to obtain a path from $u$ to $v$ shorter than $P$, contradicting its optimality. $\blacksquare$

### 6.2 Triangle Inequality (Weighted) / אי-שוויון המשולש (משוקלל)
*   **Statement:** $\delta(s, v) \le \delta(s, u) + w(u, v)$.
*   **Proof:** The shortest path to $u$ followed by edge $(u, v)$ forms a path to $v$ of weight $\delta(s, u) + w(u, v)$. The shortest path $\delta(s, v)$ is at most this weight. $\blacksquare$

### 6.3 Upper-Bound Property (Weighted) / תכונת החסם העליון (משוקלל)
*   **Statement:** $d[v] \ge \delta(s, v)$ at all times, and once $d[v] = \delta(s, v)$ it never changes.
*   **Proof:** Initially true. Suppose an edge $(u, v)$ is relaxed: $d[v] = \min(d[v], d[u] + w(u, v))$. By induction, $d[u] \ge \delta(s, u)$. Using the Triangle Inequality:
    $$d[u] + w(u, v) \ge \delta(s, u) + w(u, v) \ge \delta(s, v)$$
    Thus, $d[v]$ remains bounded below by $\delta(s, v)$. $\blacksquare$

### 6.4 Convergence Property / תכונת ההתכנסות
*   **Statement:** If $d[u] = \delta(s, u)$ and we relax $(u, v)$ on a shortest path, then $d[v] = \delta(s, v)$ afterward.
*   **Proof:** Since $v$ is on a shortest path after $u$, $\delta(s, v) = \delta(s, u) + w(u, v)$. After relaxing $(u, v)$:
    $$d[v] \le d[u] + w(u, v) = \delta(s, u) + w(u, v) = \delta(s, v)$$
    Since $d[v] \ge \delta(s, v)$ always, we have $d[v] = \delta(s, v)$. $\blacksquare$

### 6.5 Path-Relaxation Property / תכונת הרפיית המסלול
*   **Statement:** If $P = v_0 \to v_1 \to \dots \to v_k$ is a shortest path, and we relax its edges in order, then $d[v_k] = \delta(s, v_k)$.
*   **Proof:** Follows by induction. Initially $d[v_0] = d[s] = \delta(s, s)$. Relaxing $(v_0, v_1)$ converges $d[v_1]$ to $\delta(s, v_1)$ (by the Convergence Property). Inductively, relaxing $(v_{i-1}, v_i)$ yields $d[v_i] = \delta(s, v_i)$. $\blacksquare$

### 6.6 Unreachable Vertices Remain Infinite / צמתים לא נגישים נותרים באינסוף
*   **Statement:** If $\delta(s, v) = \infty$, then $d[v] = \infty$ always.
*   **Proof:** By the Upper-Bound property, $d[v] \ge \delta(s, v) = \infty \implies d[v] = \infty$. $\blacksquare$

---

## 7. SSSP in DAG / מסלולים קצרים ממקור יחיד ב-DAG

### 7.1 Topological Order Relaxes Paths in Sequence / סדר טופולוגי מרפה מסלולים לפי הסדר
*   **Statement:** Relaxing vertices in topological order solves SSSP in $O(V+E)$ time.
*   **Proof:** Any path in a DAG is a subsequence of the topological order. Thus, relaxing vertices in topological order guarantees that the edges of any shortest path are relaxed in order, satisfying the Path-Relaxation Property. $\blacksquare$

---

## 8. Bellman-Ford / אלגוריתם בלמן-פורד

### 8.1 Any Simple Path Has at Most |V|-1 Edges / לכל מסלול פשוט יש לכל היותר |V|-1 קשתות
*   **Statement:** If there are no negative cycles, any shortest path is simple and has $\le |V|-1$ edges.
*   **Proof:** A path with $\ge |V|$ edges must contain a cycle. If the cycle weight is positive or zero, removing it yields a path with fewer edges of equal or lesser weight. Thus, a simple shortest path of at most $|V|-1$ edges always exists. $\blacksquare$

### 8.2 Negative Cycle Detection / זיהוי מעגלים שליליים
*   **Statement:** If $d[v] > d[u] + w(u, v)$ after $|V|-1$ rounds, $G$ contains a reachable negative cycle.
*   **Proof:** Suppose for contradiction that there is no negative cycle, but we can still relax some edge $(u, v)$. This implies the path to $v$ has more than $|V|-1$ edges, which must contain a cycle. Since we can still decrease the distance, this cycle must have a negative weight. $\blacksquare$

---

## 9. Dijkstra / אלגוריתם דייקסטרה

### 9.1 Closed Set Invariant / אינווריאנטת הקבוצה הסגורה
*   **Statement:** When $u$ is added to $S$, $d[u] = \delta(s, u)$.
*   **Proof:** See detailed proof in Section 4.1. $\blacksquare$

### 9.2 Extract-Min Safety / בטיחות בחירת מינימום
*   **Statement:** The vertex minimizing $d[v]$ outside $S$ is safe to close.
*   **Proof:** Follows from the Closed Set Invariant. Any other path to $v$ must leave $S$ via some vertex $x$, and since weights are non-negative, the distance to $x$ (and thus any path through it) is at least $d[x] \ge d[v]$. $\blacksquare$

---

## 10. Floyd-Warshall / אלגוריתם פלויד-וורשאל

### 10.1 Intermediate Vertex Recursion / רקורסיית צומת הביניים
*   **Statement:** $d_{ij}^{(k)} = \min\left( d_{ij}^{(k-1)}, d_{ik}^{(k-1)} + d_{kj}^{(k-1)} \right)$.
*   **Proof:** A shortest path from $i$ to $j$ using intermediate vertices in $\{1, \dots, k\}$ either:
    1. Does not use vertex $k$: its weight is $d_{ij}^{(k-1)}$.
    2. Uses vertex $k$: it splits into $i \rightsquigarrow k$ and $k \rightsquigarrow j$. Since $k$ is visited only once, these subpaths only use intermediate vertices in $\{1, \dots, k-1\}$, having weights $d_{ik}^{(k-1)}$ and $d_{kj}^{(k-1)}$. $\blacksquare$

### 10.2 Negative Cycle Detection in Floyd-Warshall / זיהוי מעגלים שליליים בפלויד-וורשאל
*   **Statement:** $d_{ii}^{(n)} < 0$ iff $i$ is on a negative cycle.
*   **Proof:** $d_{ii}^{(n)}$ represents the shortest path from $i$ back to itself. If there is a negative cycle containing $i$, this path will have a negative weight. $\blacksquare$

---

## 11. Johnson / אלגוריתם ג'ונסון

### 11.1 Telescoping Property of Reweighting / תכונת הטלסקופיה של משקול מחדש
*   **Statement:** $w'(P) = w(P) + h(v_0) - h(v_k)$.
*   **Proof:** See detailed proof in Section 4.2. $\blacksquare$

### 11.2 Non-negativity of Reweighted Edges / אי-שליליות של קשתות ממושקלות מחדש
*   **Statement:** If $h(v) = \delta(s_0, v)$, then $w'(u, v) \ge 0$.
*   **Proof:** By the Triangle Inequality, $\delta(s_0, v) \le \delta(s_0, u) + w(u, v) \implies h(v) \le h(u) + w(u, v) \implies w(u, v) + h(u) - h(v) \ge 0$. Thus, $w'(u, v) \ge 0$. $\blacksquare$

### 11.3 Reconstructing Original Distance / שחזור המרחק המקורי
*   **Statement:** $\delta(u, v) = \delta'(u, v) - h(u) + h(v)$.
*   **Proof:** Since $w'(P) = w(P) + h(u) - h(v)$ for any path from $u$ to $v$, minimizing $w'(P)$ is equivalent to minimizing $w(P)$. Thus, $\delta'(u, v) = \delta(u, v) + h(u) - h(v)$, which yields the formula. $\blacksquare$

### 11.4 Feasible Landmark Potential / פוטנציאל לנדמרק פיזבילי
*   **Statement:** Let $G = (V, E)$ be a directed graph with edge weight function $w: E \to \mathbb{R}$ and let $S \subseteq V$ be a set of anchor landmark vertices. The function:
    $$p(u) = \max_{v_i \in S} \big\{ \delta(u, v_i) - \delta(t, v_i) \big\}$$
    is a feasible potential function (i.e. $\hat{w}(u, v) = w(u, v) + p(v) - p(u) \ge 0$ for all $(u, v) \in E$).
*   **Proof:**
    1.  For a potential function $p$ to be feasible, it must satisfy the reweighting condition:
        $$\hat{w}(u, v) = w(u, v) + p(v) - p(u) \ge 0 \implies p(u) \le w(u, v) + p(v)$$
        for all $(u, v) \in E$.
    2.  By the triangle inequality of the true shortest-path distance function $\delta$, for any edge $(u, v) \in E$ and any destination/anchor vertex $v_i \in S$, we have:
        $$\delta(u, v_i) \le w(u, v) + \delta(v, v_i)$$
    3.  Subtract the constant offset $\delta(t, v_i)$ from both sides:
        $$\delta(u, v_i) - \delta(t, v_i) \le w(u, v) + \delta(v, v_i) - \delta(t, v_i)$$
    4.  Since this inequality holds for every individual anchor vertex $v_i \in S$, it must also hold for the maximum over all anchors in $S$:
        $$\max_{v_i \in S} \big\{ \delta(u, v_i) - \delta(t, v_i) \big\} \le \max_{v_i \in S} \big\{ w(u, v) + \delta(v, v_i) - \delta(t, v_i) \big\}$$
    5.  Since $w(u, v)$ is a constant scalar relative to the choice of $v_i$, it can be factored out of the maximization:
        $$\max_{v_i \in S} \big\{ \delta(u, v_i) - \delta(t, v_i) \big\} \le w(u, v) + \max_{v_i \in S} \big\{ \delta(v, v_i) - \delta(t, v_i) \big\}$$
    6.  Substituting the definition of the potential function $p(u)$ and $p(v)$ yields:
        $$p(u) \le w(u, v) + p(v)$$
        which simplifies directly to:
        $$\hat{w}(u, v) = w(u, v) + p(v) - p(u) \ge 0 \quad \blacksquare$$

---

## 12. Seidel / אלגוריתם סיידל

### 12.1 Distance Relation between G and G' / קשר המרחקים בין G ל-G^2
*   **Statement:** Let $G^2$ be the graph where $(u, v) \in E(G^2)$ if $\delta_G(u, v) \le 2$. Then $\delta_{G^2}(u, v) = \lceil \delta_G(u, v) / 2 \rceil$.
*   **Proof:** 
    *   If $\delta_G(u, v) = 2k$, we can pair adjacent edges on the shortest path to form a path of length $k$ in $G^2$.
    *   If $\delta_G(u, v) = 2k-1$, we can pair them to form a path of length $k$ in $G^2$. In both cases, the distance in $G^2$ is exactly $\lceil \delta_G(u, v) / 2 \rceil$. $\blacksquare$

### 12.2 Distance Parity Reconstruction / שחזור זוגיות המרחק
*   **Statement:** We can determine if $\delta_G(u, v)$ is even or odd by checking if the sum of distances from $u$'s neighbors in $G^2$ is small or large.
*   **Proof:** Let $N_G(v)$ be the neighbors of $v$ in $G$. If $\delta_G(u, v) = 2d$, then for all $w \in N_G(v)$, $\delta_{G^2}(u, w) \ge d$. If $\delta_G(u, v) = 2d-1$, there exists some $w \in N_G(v)$ such that $\delta_{G^2}(u, w) = d-1$. This parity difference allows exact reconstruction. $\blacksquare$

---

## 13. A* Search / אלגוריתם חיפוש A*

### 13.1 Feasible Potential Function (Feasibility Criterion) / פונקציית פוטנציאל פיזבילית (קריטריון הפיזביליות)
*   **Definition:** A potential function $p: V \to \mathbb{R}$ is **feasible** (פיזבילי) with respect to an edge weight function $w: E \to \mathbb{R}^+$ if for every directed edge $(u,v) \in E$, the potential-adjusted weight (reduced cost) $\hat{w}(u,v)$ is non-negative:
    $$\hat{w}(u,v) = w(u,v) - p(u) + p(v) \ge 0 \quad \forall (u,v) \in E$$
    which is equivalent to the consistency inequality:
    $$p(u) - p(v) \le w(u,v) \quad \forall (u,v) \in E$$
    Furthermore, in the context of A* search terminating at a target $t$, we require $p(t) = 0$.

### 13.2 Admissibility / קבילות
*   **Statement:** A heuristic potential function $p(u)$ is **admissible** (קביל) if it never overestimates the actual distance to the target $t$:
    $$0 \le p(u) \le \delta(u, t) \quad \forall u \in V$$
    with $p(t) = 0$.
*   **Theorem (Consistency Implies Admissibility):** If $p(u)$ is feasible (consistent) and $p(t) = 0$, then $p(u)$ is admissible.
*   **Proof:** 
    1. Let $P = (u = v_0, v_1, \dots, v_k = t)$ be the shortest path from $u$ to the target $t$. Its length is $w(P) = \delta(u, t)$.
    2. Since $p$ is feasible, it satisfies the consistency inequality for all edges $(v_i, v_{i+1}) \in P$:
       $$p(v_i) - p(v_{i+1}) \le w(v_i, v_{i+1})$$
    3. Summing these inequalities along the entire path $P$:
       $$\sum_{i=0}^{k-1} (p(v_i) - p(v_{i+1})) \le \sum_{i=0}^{k-1} w(v_i, v_{i+1})$$
    4. The left side telescopes:
       $$p(u) - p(t) \le w(P)$$
    5. Since $p(t) = 0$ and $w(P) = \delta(u, t)$, this yields:
       $$p(u) \le \delta(u, t) \quad \blacksquare$$

### 13.3 Telescoping Potential / פוטנציאל טלסקופי
*   **Statement:** The reweighted path cost $\hat{w}(P) = w(P) - p(s) + p(t)$ preserves shortest paths.
*   **Proof:** For any path $P = (s = v_0, v_1, \dots, v_k = t)$ from $s$ to $t$:
    $$\hat{w}(P) = \sum_{i=0}^{k-1} \hat{w}(v_i, v_{i+1}) = \sum_{i=0}^{k-1} (w(v_i, v_{i+1}) - p(v_i) + p(v_{i+1}))$$
    $$\hat{w}(P) = \sum_{i=0}^{k-1} w(v_i, v_{i+1}) - p(s) + p(t) = w(P) - p(s) + p(t)$$
    Since $p(s) - p(t)$ is a constant offset that shifts the weight of all $s \to t$ paths by the same amount, a path minimizes $\hat{w}(P)$ if and only if it minimizes $w(P)$. Thus, the shortest paths are preserved. $\blacksquare$

### 13.4 Pruning Expensive Vertices / גיזום צמתים יקרים
*   **Statement:** A* will not visit any vertex $u$ for which $\delta(s, u) + p(u) > \delta(s, t)$ before reaching $t$.
*   **Proof:** A* processes vertices in order of $f(u) = d(s, u) + p(u)$. When the target $t$ is reached, $f(t) = \delta(s, t)$ (since $p(t) = 0$). Any vertex with $f(u) > \delta(s, t)$ will remain in the queue unvisited. $\blacksquare$

### 13.5 Perfect Potential / פוטנציאל מושלם
*   **Statement:** If $p(u) = \delta(u, t)$, A* visits only vertices on a shortest path.
*   **Proof:** Under this potential, the reweighted cost of any edge $(u, v)$ on a shortest path is $\hat{w}(u, v) = w(u, v) - \delta(u, t) + \delta(v, t) = 0$. For any other edge, it is positive. The algorithm will follow the 0-weight edges directly to the destination. $\blacksquare$


---

## 14. Flow Networks / רשתות זרימה
> **Study Videos:** [Ford-Fulkerson Theory](https://www.youtube.com/watch?v=LdOnanfc5TM) | [Ford-Fulkerson Code](https://www.youtube.com/watch?v=Xu8jjJnwvxE)

### 14.1 Flow Across a Cut / זרימה דרך חתך
*   **Statement:** The net flow crossing any cut $(S, T)$ is equal to the total flow value: $f(S, T) = |f|$.
*   **Proof:** 
    $$f(S, T) = f(S, V) - f(S, S) = f(S, V) = f(s, V) + \sum_{u \in S - \{s\}} f(u, V)$$
    By conservation of flow, the summation term is 0. Thus, $f(S, T) = f(s, V) = |f|$. $\blacksquare$

### 14.2 Cut Capacity Bound / חסם קיבולת החתך
*   **Statement:** For any flow $f$ and any cut $(S, T)$, $|f| \le c(S, T)$.
*   **Proof:** 
    $$|f| = f(S, T) = \sum_{u \in S} \sum_{v \in T} f(u, v) - \sum_{v \in T} \sum_{u \in S} f(v, u) \le \sum_{u \in S} \sum_{v \in T} f(u, v)$$
    Since $f(u, v) \le c(u, v)$, this is bounded above by $c(S, T)$. $\blacksquare$

### 14.3 Max-Flow Min-Cut Theorem / משפט זרימה מקסימלית וחתך מינימלי (Max-Flow Min-Cut)
*   **Statement:** $f$ is max flow $\iff$ no augmenting path in $G_f$ $\iff$ $|f| = c(S, T)$.
*   **Proof:** See detailed proof in Section 5.1. $\blacksquare$

### 14.4 Submodularity of Cut Capacities / סובמודולריות של קיבולי חתכים
*   **Statement:** For any flow network $G = (V, E)$ with non-negative capacity function $c$, let $c(S)$ denote the capacity of the cut $(S, V \setminus S)$. For any two subsets $S, S' \subseteq V$:
    $$c(S \cup S') + c(S \cap S') \le c(S) + c(S')$$
*   **Proof:**
    1. Partition $V$ into 4 disjoint regions based on membership in $S$ and $S'$:
       * $A = S \cap S'$ (in both)
       * $B = S \setminus S'$ (in $S$ only)
       * $C = S' \setminus S$ (in $S'$ only)
       * $D = V \setminus (S \cup S')$ (in neither)
    2. Write cut capacities in terms of directed edge capacities $c(X, Y) = \sum_{x \in X, y \in Y} c(x, y)$ between these regions:
       * $c(S) = c(A \cup B, C \cup D) = c(A, C) + c(A, D) + c(B, C) + c(B, D)$
       * $c(S') = c(A \cup C, B \cup D) = c(A, B) + c(A, D) + c(C, B) + c(C, D)$
       * $c(S \cup S') = c(A \cup B \cup C, D) = c(A, D) + c(B, D) + c(C, D)$
       * $c(S \cap S') = c(A, B \cup C \cup D) = c(A, B) + c(A, C) + c(A, D)$
    3. Summing the original cuts vs. union/intersection cuts:
       * $c(S) + c(S') = 2c(A, D) + c(A, C) + c(A, B) + c(B, D) + c(C, D) + c(B, C) + c(C, B)$
       * $c(S \cup S') + c(S \cap S') = 2c(A, D) + c(A, B) + c(A, C) + c(B, D) + c(C, D)$
    4. Subtracting the two equations:
       $$(c(S) + c(S')) - (c(S \cup S') + c(S \cap S')) = c(B, C) + c(C, B)$$
    5. Since capacities are non-negative ($c(B, C) \ge 0$ and $c(C, B) \ge 0$), this difference is non-negative:
       $$c(S \cup S') + c(S \cap S') \le c(S) + c(S') \quad \blacksquare$$

### 14.5 Union & Intersection of Minimum Cuts / איחוד וחיתוך של חתכים מינימליים
*   **Statement:** If $(S, V \setminus S)$ and $(S', V \setminus S')$ are two minimum $(s,t)$-cuts, then $(S \cup S', V \setminus (S \cup S'))$ and $(S \cap S', V \setminus (S \cap S'))$ are also minimum $(s,t)$-cuts.
*   **Proof:**
    1. Let $f^*$ be a maximum flow in $G$. By the Max-Flow Min-Cut Theorem, $c(S) = c(S') = |f^*|$.
    2. Since $s \in S$ and $s \in S'$, we have $s \in S \cap S'$ and $s \in S \cup S'$. Similarly, since $t \notin S$ and $t \notin S'$, we have $t \notin S \cap S'$ and $t \notin S \cup S'$. Thus, both $S \cap S'$ and $S \cup S'$ define valid $(s,t)$-cuts.
    3. By Weak Duality, any valid $(s,t)$-cut has capacity at least the maximum flow value:
       $$c(S \cap S') \ge |f^*| \quad \text{and} \quad c(S \cup S') \ge |f^*|$$
    4. By the submodularity of cut capacities (Section 14.4):
       $$c(S \cup S') + c(S \cap S') \le c(S) + c(S') = |f^*| + |f^*| = 2|f^*|$$
    5. Since both terms on the left are at least $|f^*|$, and their sum is at most $2|f^*|$, they must both be exactly equal to $|f^*|$:
       $$c(S \cup S') = |f^*| \quad \text{and} \quad c(S \cap S') = |f^*|$$
       which confirms that both are minimum $(s,t)$-cuts. $\blacksquare$

---


## 15. Ford-Fulkerson / אלגוריתם פורד-פולקרסון

### 15.1 Augmenting Residual Flows / שיפור זרימה שיורית
*   **Statement:** If $f'$ is a flow in $G_f$, then $f + f'$ is a valid flow in $G$ of value $|f| + |f'|$.
*   **Proof:** We verify capacity constraints and flow conservation:
    *   Capacity: $(f+f')(u,v) = f(u,v) + f'(u,v) \le f(u,v) + (c(u,v) - f(u,v)) = c(u,v)$.
    *   Conservation: For any $u \neq s, t$:
        $$\sum_v (f+f')(u, v) = \sum_v f(u,v) + \sum_v f'(u,v) = 0 + 0 = 0 \quad \blacksquare$$

### 15.2 Integrality Theorem / משפט השלמות (Integrality Theorem)
*   **Statement:** If edge capacities are integers, the maximum flow is integer, and Ford-Fulkerson terminates in $O(E|f^*|)$ time.
*   **Proof:** By induction, the bottleneck capacity of the augmenting path in each iteration is an integer (minimum of integer residual capacities). Thus, the flow remains integer-valued throughout. Since each step increases flow by at least 1, the algorithm must terminate in at most $|f^*|$ iterations. $\blacksquare$

---

## 16. Edmonds-Karp / אלגוריתם אדמונדס-קארפ
> **Study Videos:** [Edmonds-Karp Theory](https://www.youtube.com/watch?v=RppuJYwlcI8) | [Edmonds-Karp Code](https://www.youtube.com/watch?v=OViaWp9Q-Oc)

### 16.1 Monotonicity of Source Distances / מונוטוניות המרחקים מהמקור
*   **Statement:** For all $v \in V$, the shortest-path distance $\delta_f(s, v)$ in $G_f$ does not decrease after an augmentation.
*   **Proof by Contradiction:** Suppose some vertices have decreased distances. Let $v$ be the vertex with the minimum new distance $d' = \delta_{f'}(s, v)$ that decreased. Let $u$ be the predecessor of $v$ on the new shortest path. Thus, $\delta_{f'}(s, v) = \delta_{f'}(s, u) + 1$. Since $u$ has a smaller distance, its distance did not decrease: $\delta_{f'}(s, u) \ge \delta_f(s, u)$.
    *   If $(u, v)$ was in $G_f$, then $\delta_f(s, v) \le \delta_f(s, u) + 1 \le \delta_{f'}(s, u) + 1 = \delta_{f'}(s, v)$, contradicting that the distance decreased.
    *   If $(u, v)$ was not in $G_f$, the augmentation must have pushed flow along $(v, u)$, meaning $(v, u)$ was on the augmenting path, so $\delta_f(s, u) = \delta_f(s, v) + 1$. This implies:
        $$\delta_f(s, v) = \delta_f(s, u) - 1 \le \delta_{f'}(s, u) - 1 = \delta_{f'}(s, v) - 2 < \delta_{f'}(s, v)$$ (contradiction). $\blacksquare$

### 16.2 Monotonicity of Sink Distances / מונוטוניות המרחקים אל הבור
*   **Statement:** For all $v \in V$, $\delta_f(v, t)$ does not decrease after an augmentation.
*   **Proof:** Symmetric to the source distance monotonicity proof, using backward induction from the sink $t$. $\blacksquare$

### 16.3 Shortest-Path Inclusion / הכלה במסלול הקצר
*   **Statement:** If $\delta_{f'}(s, t) = \delta_f(s, t)$, any shortest path in $G_{f'}$ only uses edges that were already on shortest paths in $G_f$.
*   **Proof:** Follows from the monotonicity lemmas. If a new edge $(u, v)$ was introduced, it must be the reverse of an edge on the augmenting path, which implies its endpoints satisfy $\delta_f(s, u) = \delta_f(s, v) + 1$, meaning it cannot be used to shorten or maintain the same shortest path length. $\blacksquare$

### 16.4 Saturated Edges Do Not Return in Phase / קשתות רוויות אינן חוזרות באותו שלב מרחקים
*   **Statement:** An edge $(u, v)$ saturated in a phase cannot reappear in $G_f$ in the same phase.
*   **Proof:** If $(u, v)$ is saturated, it is removed from $G_f$. To reappear, flow must be pushed along the reverse edge $(v, u)$, which requires $\delta(s, v) = \delta(s, u) + 1$. But since it was saturated when $\delta(s, u) = \delta(s, v) - 1$, and distances are monotonic, this cannot occur within the same distance phase. $\blacksquare$

### 16.5 Number of Iterations (Edmonds-Karp) / מספר האיטרציות (אדמונדס-קארפ)
*   **Statement:** Edmonds-Karp performs at most $O(VE)$ augmentations.
*   **Proof:** Each augmentation contains at least one critical edge (an edge that is saturated and removed from $G_f$). For an edge $(u, v)$ to become critical again, its distance must increase by at least 2. Since the maximum distance is $|V|$, each of the $E$ edges can become critical at most $|V|/2$ times. Thus, there are at most $O(VE)$ total iterations. $\blacksquare$

---

## 17. Dinic / אלגוריתם דיניץ
> **Study Videos:** [Dinic's Theory](https://www.youtube.com/watch?v=M6cm8UeeziI) | [Dinic's Code](https://www.youtube.com/watch?v=_SdF4KK_dyM)

### 17.1 Blocking Flow Increases Distance / זרימה חוסמת מגדילה את המרחק
*   **Statement:** After finding a blocking flow in the level graph, the distance $\delta(s, t)$ in the next phase increases.
*   **Proof:** A blocking flow saturates at least one edge on *every* path of length $\delta(s, t)$ in the level graph. Any new path must use either a back edge or a non-level graph edge, both of which require a path length strictly greater than the current phase. $\blacksquare$

### 17.2 Layer Graph Update Cost / עלות עדכון גרף השכבות
*   **Statement:** The level graph can be updated, and a blocking flow found, in $O(V^2 E)$ or $O(VE \log V)$ time per phase.
*   **Proof:** Using DFS with backtracking, each step either advances along a path (at most $V$ times before enqueuing flow) or retreats, deleting a dead-end vertex. Since each vertex is deleted at most once, the total time spent backtracking is $O(V+E)$. $\blacksquare$

---

## 18. Hopcroft-Karp / אלגוריתם הופקרופט-קארפ
> **Study Videos:** [Bipartite Matching Theory](https://www.youtube.com/watch?v=GhjwOiJ4SqU) | [Mice & Owls Problem](https://www.youtube.com/watch?v=ar6x7dHfGHA) | [Elementary Math Problem](https://www.youtube.com/watch?v=zrGnYstL4ss)

### 18.1 Matching-to-Flow Correspondence / התאמה בין שידוך לזרימה
*   **Statement:** A matching of size $k$ exists in a bipartite graph $G = (L \cup R, E)$ iff there is an integer flow of value $k$ in the network with source $s \to L$ and $R \to t$.
*   **Proof:** 
    *   **$\implies$:** Set flow $f(s, u) = 1$ for matched $u \in L$, $f(v, t) = 1$ for matched $v \in R$, and $f(u, v) = 1$ for matched edges. This satisfies conservation and capacity constraints.
    *   **$\impliedby$:** Since capacities are 1, an integer flow of value $k$ must consist of $k$ vertex-disjoint paths of the form $s \to u \to v \to t$, which defines a valid matching of size $k$. $\blacksquare$

### 18.2 Vertex-Disjoint Augmenting Paths / מסלולי שיפור זרים בצמתים
*   **Statement:** A maximal set of shortest vertex-disjoint augmenting paths can be found in $O(E)$ time per phase.
*   **Proof:** We run BFS to build the level graph, then DFS to find paths. To ensure they are vertex-disjoint, we mark vertices as visited during DFS and never reuse them. $\blacksquare$

### 18.3 Phase Bound After Long Paths / חסם שלבי האלגוריתם לאחר מסלולים ארוכים
*   **Statement:** If the shortest augmenting path has length $\ell$, the current matching $M$ satisfies $|M^*| - |M| \le |V| / \ell$.
*   **Proof:** The symmetric difference $M^* \oplus M$ contains $|M^*| - |M|$ vertex-disjoint augmenting paths. Since each path must have length at least $\ell$, and they are vertex-disjoint, they can use at most $|V|$ vertices in total. Hence, $(\text{number of paths}) \cdot \ell \le |V| \implies |M^*| - |M| \le |V|/\ell$. $\blacksquare$

---

## 19. BMM & Transitive Closure / כפל מטריצות בוליאני וסגור טרנזיטיבי

### 19.1 Boolean Exponentiation Meaning / משמעות חזקה בוליאנית
*   **Statement:** $(A^k)_{ij} = 1$ iff there is a walk of length exactly $k$ from $i$ to $j$.
*   **Proof by Induction:** 
    *   Base case ($k=1$): $(A^1)_{ij} = 1 \iff (i, j) \in E$, which is a walk of length 1.
    *   Inductive step: $(A^{k+1})_{ij} = \bigvee_m (A^k)_{im} \wedge A_{mj}$. This is 1 iff there is some intermediate vertex $m$ such that there is a walk $i \rightsquigarrow m$ of length $k$ and an edge $(m, j)$, which is equivalent to a walk of length $k+1$. $\blacksquare$

### 19.2 Self-Loops Convert 'Up to k' to 'Exactly k' / לולאות עצמיות הופכות 'עד k' ל'בדיוק k'
*   **Statement:** In $A \lor I$, $(A \lor I)^k_{ij} = 1$ iff there is a path of length $\le k$ in $G$.
*   **Proof:** The identity matrix $I$ represents self-loops. A walk of length $r < k$ can be extended to a walk of length exactly $k$ by traversing the self-loop at $v$ exactly $k-r$ times. $\blacksquare$

### 19.3 BMM to Transitive Closure (With Log Factor) / סגור טרנזיטיבי באמצעות BMM (עם גורם לוגריתמי)
*   **Statement:** TC can be computed in $O(T_{BMM}(n) \log n)$ time.
*   **Proof:** Compute $(A \lor I)^n$ by repeated squaring: $(A \lor I)^2$, $(A \lor I)^4$, etc. This requires $\log n$ matrix multiplications. $\blacksquare$

### 19.4 BMM to Transitive Closure (Without Log Factor) / סגור טרנזיטיבי באמצעות BMM (ללא גורם לוגריתמי)
*   **Statement:** TC can be computed in $O(T_{BMM}(n))$ time.
*   **Proof:** See detailed proof in Section 6.1. $\blacksquare$

---

## 20. Matrix Multiplication Verification / אימות כפל מטריצות

### 20.1 Freivalds' One-Sided Error / שגיאה חד-צדדית של פרייבלדס
*   **Statement:** $\Pr(ABr = Cr) \le 1/2$ if $AB \neq C$.
*   **Proof:**
    1.  Let $D = A \cdot B - C$. Since $A \cdot B \neq C$, the matrix $D \neq 0$, meaning it contains at least one non-zero entry. Let this non-zero entry be $D_{i,j} \neq 0$ at row $i$ and column $j$.
    2.  Consider the $i$-th entry of the vector $D \cdot r$:
        $$(D \cdot r)_i = \sum_{k=1}^n D_{i,k} r_k = D_{i,j} r_j + \sum_{k \neq j} D_{i,k} r_k$$
    3.  The algorithm returns `True` (incorrectly claiming equality) only if $D \cdot r = \vec{0}$, which requires $(D \cdot r)_i = 0$.
    4.  Condition on the choices of all random variables $r_k$ for $k \neq j$. Let $S = \sum_{k \neq j} D_{i,k} r_k$ be the sum of these other terms. For $(D \cdot r)_i$ to be zero, we must have:
        $$D_{i,j} r_j + S = 0 \implies D_{i,j} r_j = -S$$
    5.  Since $D_{i,j} \neq 0$ and $r_j \in \{0, 1\}$, there is at most *one* value of $r_j$ that satisfies this equation:
        *   If $S = 0$, then $r_j$ must be 0 (since $D_{i,j} \neq 0$).
        *   If $S = -D_{i,j}$, then $r_j$ must be 1.
        *   If $S$ is any other value, no choice of $r_j \in \{0, 1\}$ can satisfy it.
    6.  Since $r_j$ is chosen uniformly at random from $\{0, 1\}$ independently of the other $r_k$, the probability that $r_j$ takes this unique satisfying value is at most $\frac{1}{2}$.
    7.  Thus, the probability that $(D \cdot r)_i = 0$ given any fixed choices for all other $r_k$ is at most $\frac{1}{2}$. By the law of total probability, this bounds the overall probability:
        $$\Pr(D \cdot r = \vec{0}) \le \Pr((D \cdot r)_i = 0) \le \frac{1}{2} \quad \blacksquare$$


### 20.2 Amplification / הגברת הסתברות (Amplification)
*   **Statement:** The error probability after $k$ independent runs is $\le 2^{-k}$.
*   **Proof:** Since each run is independent, the probability that the algorithm incorrectly accepts in all $k$ iterations is the product of the individual error probabilities:
    $$\Pr(\text{False Accept in all } k \text{ runs}) \le \prod_{i=1}^k \frac{1}{2} = \left(\frac{1}{2}\right)^k = 2^{-k} \quad \blacksquare$$

---

## 21. Randomized Algorithms Basics / יסודות של אלגוריתמים אקראיים

### 21.1 Linearity of Expectation / ליניאריות של תוחלת
*   **Statement:** $E[X+Y] = E[X] + E[Y]$.
*   **Proof:** By definition of expectation over the joint probability space:
    $$E[X+Y] = \sum_x \sum_y (x+y) \Pr(X=x, Y=y)$$
    $$E[X+Y] = \sum_x x \sum_y \Pr(X=x, Y=y) + \sum_y y \sum_x \Pr(X=x, Y=y)$$
    Since $\sum_y \Pr(X=x, Y=y) = \Pr(X=x)$ and $\sum_x \Pr(X=x, Y=y) = \Pr(Y=y)$:
    $$E[X+Y] = \sum_x x \Pr(X=x) + \sum_y y \Pr(Y=y) = E[X] + E[Y] \quad \blacksquare$$

### 21.2 Union Bound / חסם האיחוד (Union Bound)
*   **Statement:** $\Pr(\bigcup_i E_i) \le \sum_i \Pr(E_i)$.
*   **Proof:** Follows by induction from the base case of two events:
    $$\Pr(A \cup B) = \Pr(A) + \Pr(B) - \Pr(A \cap B) \le \Pr(A) + \Pr(B) \quad \blacksquare$$

---

## 22. Randomized Quicksort / מיון מהיר אקראי

### 22.1 Pairwise Comparison Probability / הסתברות השוואה זוגית
*   **Statement:** Let $z_i, z_j$ be the $i$-th and $j$-th smallest elements. The probability that they are compared is $2 / (j - i + 1)$.
*   **Proof:** Elements $z_i$ and $z_j$ are compared iff the first element selected as a pivot from the set $Z_{ij} = \{z_i, \dots, z_j\}$ is either $z_i$ or $z_j$. Since pivots are chosen uniformly at random, each of the $j - i + 1$ elements in $Z_{ij}$ has an equal probability of being chosen first. Thus, the probability of selecting $z_i$ or $z_j$ first is $2 / (j - i + 1)$. $\blacksquare$

### 22.2 Expected Time / זמן ריצה צפוי
*   **Statement:** The expected number of comparisons is $O(n \log n)$.
*   **Proof:** Let $X_{ij}$ be the indicator variable that $z_i$ and $z_j$ are compared. The total number of comparisons is $X = \sum_{i=1}^{n-1} \sum_{j=i+1}^n X_{ij}$. By linearity of expectation:
    $$E[X] = \sum_{i=1}^{n-1} \sum_{j=i+1}^n E[X_{ij}] = \sum_{i=1}^{n-1} \sum_{j=i+1}^n \frac{2}{j-i+1}$$
    Let $k = j - i$. The sum simplifies to:
    $$E[X] = \sum_{i=1}^{n-1} \sum_{k=1}^{n-i} \frac{2}{k+1} < 2 \sum_{i=1}^n \sum_{k=1}^n \frac{1}{k} = 2 n H_n$$
    Since $H_n \approx \ln n + O(1)$, $E[X] = \mathbf{O(n \log n)}$ comparisons. $\blacksquare$

### 22.3 Paranoid QuickSort Expected Runtime / זמן ריצה צפוי של Paranoid QuickSort
*   **Statement:** The expected runtime of Paranoid QuickSort on an array of size $n$ is $O(n \log n)$ in the worst case.
*   **Proof:**
    1.  At each step of size $m$, we select a pivot uniformly at random and partition the array. The split is accepted if and only if both subproblems are of size at least $m/4$ (a "good pivot").
    2.  An element is a good pivot if it falls between the 25th and 75th percentiles of the sorted array. Exactly half of the elements satisfy this condition, so the probability of choosing a good pivot in any single trial is $p = 1/2$.
    3.  The number of trials required to find a good pivot is a geometric random variable $K \sim \text{Geom}(1/2)$. The expected number of attempts is:
        $$E[K] = \frac{1}{p} = 2$$
    4.  Each partitioning attempt on an array of size $m$ takes $O(m)$ time. Thus, the expected time spent finding a good pivot at a node of size $m$ is $2 \cdot O(m) = O(m)$.
    5.  Let $T'(n) = E[T(n)]$ be the expected runtime. Since the split is guaranteed to yield subproblems of size at most $3n/4$, we obtain the recurrence:
        $$T'(n) \le T'(n - x) + T'(x) + O(n) \le T'(3n/4) + T'(n/4) + O(n)$$
        where $n/4 \le x \le 3n/4$.
    6.  Solving this recurrence using the Master Theorem or tree expansion shows that the tree depth is strictly bounded by $\log_{4/3} n = O(\log n)$. Since the expected work per level is $O(n)$, the total expected runtime is **$\mathbf{O(n \log n)}$**. $\blacksquare$

---


## 23. Distributed Symmetry Breaking / שבירת סימטריה מבוזרת

### 23.1 Anonymous Leader Election / בחירת מנהיג מבוזרת אנונימית
*   **Statement:** In an anonymous network (without unique IDs), a randomized leader election algorithm where each of the $n$ players independently succeeds with probability $p = 1/n$ chooses a unique leader in $O(1)$ rounds with constant probability.
*   **Proof:** Let $N$ be the number of players who succeed. $N \sim \text{Bin}(n, p)$ with $p = 1/n$.
    The probability that exactly one player succeeds is:
    $$\Pr(N = 1) = \binom{n}{1} p (1-p)^{n-1} = n \cdot \frac{1}{n} \left(1 - \frac{1}{n}\right)^{n-1} = \left(1 - \frac{1}{n}\right)^{n-1}$$
    Taking the limit as $n \to \infty$:
    $$\lim_{n \to \infty} \left(1 - \frac{1}{n}\right)^{n-1} = \lim_{n \to \infty} \frac{(1 - 1/n)^n}{1 - 1/n} = \frac{e^{-1}}{1} = \frac{1}{e} \approx 0.368$$
    Since the probability of choosing a unique leader in a single round is at least a positive constant (approx. $0.368$), the number of rounds until success is a geometric random variable with success probability $\ge 0.368$, yielding expected rounds $E[R] \le 1/0.368 \approx 2.72 \in O(1)$. $\blacksquare$

### 23.2 Distributed $2\Delta$-Coloring / צביעה מבוזרת ב-$2\Delta$ צבעים
*   **Statement:** The distributed $2\Delta$-coloring algorithm on a graph of maximum degree $\Delta$ terminates in expected $O(\log n)$ rounds.
*   **Proof:** Let $v$ be an active node in round $i$.
    1.  Let $\Delta_{active}$ be the number of active neighbors of $v$. By definition, $\Delta_{active} \le \Delta$.
    2.  The palette size of $v$ is at least $2\Delta - (\text{colors claimed by neighbors}) \ge 2\Delta - (\Delta - \Delta_{active}) = \Delta + \Delta_{active}$.
    3.  The probability that $v$ selects a color not chosen by any active neighbor in this round is:
        $$\Pr(v \text{ succeeds}) \ge \frac{|\text{palette}| - \Delta_{active}}{|\text{palette}|} \ge \frac{\Delta}{\Delta + \Delta_{active}} \ge \frac{\Delta}{2\Delta} = \frac{1}{2}$$
    4.  Let $n_i$ be the number of uncolored vertices after round $i$. Since each node succeeds with probability at least $1/2$, the expected number of remaining active nodes is $E[n_{i}] \le n_{i-1} / 2$.
    5.  Define a round as "good" if it colors at least $1/4$ of the remaining nodes. Using Markov's inequality on the uncolored nodes $n_i$:
        $$\Pr\left(n_i \ge \frac{3}{4} n_{i-1}\right) \le \frac{E[n_i]}{\frac{3}{4} n_{i-1}} \le \frac{\frac{1}{2} n_{i-1}}{\frac{3}{4} n_{i-1}} = \frac{2}{3}$$
        Thus, the probability of a good round is $\Pr(\text{good}) \ge 1 - 2/3 = 1/3$.
    6.  We need at most $K = \log_{4/3} n + 1$ good rounds to color the entire graph. The number of rounds to achieve $K$ good rounds is a sum of $K$ independent geometric variables with $p \ge 1/3$.
    7.  By linearity of expectation, the expected total rounds is:
        $$E[R] \le 3 \cdot (\log_{4/3} n + 1) = \mathbf{O(\log n)} \quad \blacksquare$$

### 23.3 Distributed Symmetry Breaking via Polynomial ID Ranges / שבירת סימטריה מבוזרת באמצעות טווחי מזהים פולינומיים
*   **Statement:** If $n$ nodes select random IDs uniformly from a polynomial range $\{1, \dots, n^c\}$, the probability of at least one collision is bounded by $1/n^{c-2}$.
*   **Proof:**
    1.  Let $ID(u)$ be the random ID chosen by node $u$. For any pair of distinct nodes $u \neq v$, the probability of they select the same ID is:
        $$\Pr(ID(u) == ID(v)) = \frac{1}{n^c}$$
    2.  Let $E_{uv}$ be the event that $ID(u) == ID(v)$. The event that at least one collision occurs in the network is the union of $E_{uv}$ over all possible distinct pairs:
        $$\Pr(\text{collision}) = \Pr\left( \bigcup_{u \neq v} E_{uv} \right)$$
    3.  By the Union Bound (Section 21.2):
        $$\Pr(\text{collision}) \le \sum_{u \neq v} \Pr(E_{uv})$$
    4.  Since there are exactly $\binom{n}{2} = \frac{n(n-1)}{2}$ distinct pairs of nodes:
        $$\Pr(\text{collision}) \le \binom{n}{2} \cdot \frac{1}{n^c} = \frac{n(n-1)}{2 n^c} < \frac{n^2}{2 n^c} < \frac{1}{n^{c-2}} \quad \blacksquare$$

---


## 24. Coupon Collector Properties / בעיית אוסף הקופונים (קניית מתנות)

### 24.1 Expected Cost / תוחלת עלות
*   **Statement:** The expected number of random coupons bought to collect all $n$ types is $n H_n \approx n \ln n$.
*   **Proof:** Let $X$ be the number of coupons bought. Let $X_i$ be the number of coupons bought to collect the $i$-th new type after already having collected $i-1$ types.
    *   Once $i-1$ types are collected, the probability of getting a new type is $p_i = \frac{n - (i-1)}{n} = \frac{n - i + 1}{n}$.
    *   $X_i$ is a geometric random variable with success probability $p_i$, so its expected value is:
        $$E[X_i] = \frac{1}{p_i} = \frac{n}{n - i + 1}$$
    *   By linearity of expectation, the total expected coupons is:
        $$E[X] = \sum_{i=1}^n E[X_i] = \sum_{i=1}^n \frac{n}{n - i + 1} = n \sum_{j=1}^n \frac{1}{j} = n H_n \quad \blacksquare$$

### 24.2 High Probability Upper Bound / חסם עליון בהסתברות גבוהה
*   **Statement:** For any constant $c$, the probability that we need more than $(c+1)n \ln n$ coupons is at most $1/n^c$.
*   **Proof:** Let $M = (c+1)n \ln n$. The process fails to collect all coupons in $M$ steps if and only if there exists some coupon type $j$ that has not been chosen.
    1.  For a single coupon type $j$, the probability that it is not chosen in $M$ independent trials is:
        $$\Pr(j \text{ is not chosen}) = \left(1 - \frac{1}{n}\right)^M$$
    2.  Using the inequality $1 - x \le e^{-x}$:
        $$\Pr(j \text{ is not chosen}) \le e^{-M / n} = e^{-(c+1)\ln n} = \frac{1}{n^{c+1}}$$
    3.  By the Union Bound over all $n$ coupon types:
        $$\Pr(C \ge M) \le \sum_{j=1}^n \Pr(j \text{ is not chosen}) \le n \cdot \frac{1}{n^{c+1}} = \frac{1}{n^c}$$
    4.  Thus, with probability at least $1 - 1/n^c$, all coupons are collected within $(c+1)n \ln n$ steps. $\blacksquare$

---

## 25. Concentration Bounds Derivations / גזירת גבולות ריכוז

### 25.1 Chebyshev’s Higher Moments / מומנטים גבוהים של צ'בישב
*   **Statement:** For any random variable $X$ with mean $\mu$ and any even integer $2l \ge 2$:
    $$\Pr(|X - \mu| \ge a) \le \frac{E[(X - \mu)^{2l}]}{a^{2l}}$$
*   **Proof:** 
    1.  The event $|X - \mu| \ge a$ is identical to the event $(X - \mu)^{2l} \ge a^{2l}$, since raising to an even power preserves order for absolute values.
    2.  Define the non-negative random variable $Y = (X - \mu)^{2l}$.
    3.  Applying Markov's inequality to $Y$ with threshold $a^{2l}$:
        $$\Pr(|X - \mu| \ge a) = \Pr(Y \ge a^{2l}) \le \frac{E[Y]}{a^{2l}} = \frac{E[(X - \mu)^{2l}]}{a^{2l}} \quad \blacksquare$$

### 25.2 Mill’s Inequality (Gaussian Tail) / אי-שוויון מיל (זנב גאוסי)
*   **Statement:** For a normally distributed variable $X \sim \mathcal{N}(0, \sigma^2)$, the tail probability satisfies:
    $$\Pr(|X| \ge t) \le \sqrt{\frac{2}{\pi}} \frac{\sigma}{t} e^{-t^2 / 2\sigma^2} \quad \forall t > 0$$
*   **Proof:** Since the distribution is symmetric, $\Pr(|X| \ge t) = 2 \Pr(X \ge t)$.
    1.  The probability density function is $f(x) = \frac{1}{\sqrt{2\pi}\sigma} e^{-x^2 / 2\sigma^2}$.
    2.  Write the tail probability as an integral:
        $$\Pr(X \ge t) = \int_{t}^{\infty} \frac{1}{\sqrt{2\pi}\sigma} e^{-x^2 / 2\sigma^2} dx$$
    3.  Since $x/t \ge 1$ for all $x \ge t$:
        $$\Pr(X \ge t) \le \int_{t}^{\infty} \frac{x}{t} \frac{1}{\sqrt{2\pi}\sigma} e^{-x^2 / 2\sigma^2} dx = \frac{1}{t \sqrt{2\pi}\sigma} \int_{t}^{\infty} x e^{-x^2 / 2\sigma^2} dx$$
    4.  Evaluate the integral using substitution $u = x^2 / 2\sigma^2$, $du = (x / \sigma^2) dx$:
        $$\int_{t}^{\infty} x e^{-x^2 / 2\sigma^2} dx = \sigma^2 \int_{t^2 / 2\sigma^2}^{\infty} e^{-u} du = \sigma^2 e^{-t^2 / 2\sigma^2}$$
    5.  Substitute back:
        $$\Pr(X \ge t) \le \frac{\sigma^2}{t \sqrt{2\pi}\sigma} e^{-t^2 / 2\sigma^2} = \frac{1}{\sqrt{2\pi}} \frac{\sigma}{t} e^{-t^2 / 2\sigma^2}$$
    6.  Multiply by 2 for the two-sided tail:
        $$\Pr(|X| \ge t) \le \sqrt{\frac{2}{\pi}} \frac{\sigma}{t} e^{-t^2 / 2\sigma^2} \quad \blacksquare$$

---

## 26. Advanced Matrix Reductions / רדוקציות מטריצות מתקדמות

### 26.1 Equivalence of Triangle Detection and BMM / שקילות זיהוי משולש וכפל מטריצות בוליאני
*   **Statement:** If we can solve Triangle Detection (TD) on a graph of $N$ vertices in $O(N^{3-\varepsilon})$ time, then we can solve Boolean Matrix Multiplication (BMM) of size $n \times n$ in $O(n^{3-\varepsilon/3})$ time.
*   **Proof:** 
    1.  Given $A, B \in \mathbb{B}^{n \times n}$, build a tripartite graph $G$ with partitions $I, J, K$ of size $n$ each. Edges exist between $i \in I, j \in J \iff A_{ij}=1$, between $j \in J, k \in K \iff B_{jk}=1$, and all edges exist between $k \in K, i \in I$.
    2.  A triangle $(i, j, k)$ exists $\iff A_{ij} \cdot B_{jk} = 1$, which corresponds to a non-zero contribution to $C_{ik} = (A \cdot B)_{ik}$.
    3.  Divide each partition $I, J, K$ into $t$ sub-blocks of size $n/t$. There are $t^3$ triplets of blocks.
    4.  In each triplet, run a Triangle Detection algorithm on the induced subgraph of size $N = 3n/t$. If a triangle is found, set $C_{ik} = 1$ and delete the edge $(k, i)$ to avoid finding it again.
    5.  At most $n^2$ edges can be deleted. A call to the TD algorithm is successful (results in deletion) or unsuccessful (returns no triangle, happening at most once per triplet).
    6.  The number of TD calls is at most $n^2 + t^3$. The runtime is:
        $$\text{Time} = (n^2 + t^3) \cdot O\left( \left(\frac{n}{t}\right)^{3-\varepsilon} \right)$$
    7.  Setting $t = n^{2/3}$ minimizes this expression:
        $$\text{Time} = (n^2 + n^2) \cdot O\left( n^{\frac{1}{3}(3-\varepsilon)} \right) = \mathbf{O(n^{3-\varepsilon/3})} \quad \blacksquare$$

### 26.2 Bounded Weight Distance Product via BMM / מכפלת מרחקים במשקלים חסומים באמצעות BMM
*   **Statement:** Let $A, B \in ([M] \cup \{\infty\})^{n \times n}$. We can compute the distance product $C = A \star B$ where $C_{ij} = \min_{k} (A_{ik} + B_{kj})$ in $O(M^2 n^\omega)$ time using BMM.
*   **Proof:**
    1.  For each $r \in [1, M]$, construct indicator matrix $A_r$ where $(A_r)_{xy} = 1 \iff A_{xy} = r$.
    2.  For each $s \in [1, M]$, construct indicator matrix $B_s$ where $(B_s)_{xy} = 1 \iff B_{xy} = s$.
    3.  Compute $M^2$ Boolean matrix multiplications: $C_{r, s} = A_r \cdot B_s$.
    4.  $(C_{r, s})_{ij} = 1 \iff \exists k$ such that $A_{ik} = r$ and $B_{kj} = s$.
    5.  The minimum sum is $C_{ij} = \min \{ r+s \mid (C_{r, s})_{ij} = 1 \}$.
    6.  Since there are $M^2$ standard BMM steps, each taking $O(n^\omega)$ time, the total complexity is **$O(M^2 \cdot n^\omega)$** time. $\blacksquare$

### 26.3 Paths and Boolean Matrix Exponentiation / מסלולים וחזקות של מטריצות בוליאניות
*   **Statement:** Let $A$ be the Boolean adjacency matrix of a directed graph $G = (V, E)$, and let $A' = A \lor I$. Then:
    1. $(A^k)_{ij} = 1 \iff$ there exists a directed walk of length exactly $k$ from $i$ to $j$ in $G$.
    2. $((A')^{|V|-1})_{ij} = 1 \iff$ there exists a directed path from $i$ to $j$ in $G$ (reachability).
*   **Proof:**
    1.  **Induction on $k \ge 1$ for Boolean Exponentiation Invariant:**
        *   *Base Case ($k=1$):* $(A^1)_{ij} = A_{ij} = 1 \iff (i, j) \in E$, which is a walk of length 1.
        *   *Inductive Step:* Assume the statement holds for $k$, so $(A^k)_{im} = 1 \iff$ there is a walk of length $k$ from $i$ to $m$. By Boolean matrix multiplication:
            $$(A^{k+1})_{ij} = \bigvee_{m=1}^{|V|} \left( (A^k)_{im} \land A_{mj} \right)$$
            Thus, $(A^{k+1})_{ij} = 1$ if and only if there is some vertex $m$ such that $(A^k)_{im} = 1$ and $A_{mj} = 1$. By induction, this is true if and only if there is a walk of length $k$ from $i$ to $m$ and an edge $(m, j) \in E$, which is equivalent to a walk of length $k+1$ from $i$ to $j$.
    2.  **Self-Loops Expansion:**
        *   The addition of the identity matrix $I$ represents adding a self-loop $(v, v)$ of Boolean value 1 on every vertex.
        *   Any path of length $r \le |V|-1$ from $i$ to $j$ can be extended to a walk of length exactly $|V|-1$ by traversing the self-loop at any vertex along the path exactly $(|V|-1) - r$ times.
        *   Thus, $((A')^{|V|-1})_{ij} = 1 \iff$ there is a path of length at most $|V|-1$ from $i$ to $j$ in $G$, which is the definition of reachability. $\blacksquare$

### 26.4 Sparse Triangle Detection (Alon-Yuster-Zwick) / זיהוי משולש בגרפים דלילים (אלון-יוסטר-צוויק)
*   **Statement:** The Alon-Yuster-Zwick algorithm detects whether a graph contains a triangle in $O(|E|^{\frac{2\omega}{\omega+1}})$ time.
*   **Proof:**
    1.  Classify vertices using a degree threshold $d$. A vertex $v$ is heavy if $\text{deg}(v) \ge d$, else it is light.
    2.  **Case 1: Finding a triangle containing a light vertex.**
        For every edge $(u, v) \in E$, check if either $u$ or $v$ is light. Without loss of generality, let $v$ be light. Since $\text{deg}(v) < d$, we can iterate over all of $v$'s neighbors $w$ in $O(\text{deg}(v)) = O(d)$ time and check if $\{u, w\} \in E$ in $O(1)$ time.
        The total time to search over all edges with a light endpoint is bounded by:
        $$\sum_{(u, v) \in E \text{ s.t. } v \in V_{light}} \text{deg}(v) \le \sum_{u \in V} \sum_{v \in N_L(u)} d \le 2|E| \cdot d = O(|E| \cdot d)$$
    3.  **Case 2: Finding a triangle containing only heavy vertices.**
        The number of heavy vertices is bounded by $|V_{heavy}| \le \frac{2|E|}{d}$. We construct the induced subgraph containing only heavy vertices and search for a triangle using Fast Matrix Multiplication in time:
        $$O(|V_{heavy}|^\omega) = O\left( \left(\frac{|E|}{d}\right)^\omega \right)$$
    4.  **Optimizing the threshold $d$:**
        Equate the time complexities of both cases to minimize the total runtime:
        $$|E| \cdot d = \left(\frac{|E|}{d}\right)^\omega \implies d^{\omega+1} = |E|^{\omega-1} \implies d = |E|^{\frac{\omega-1}{\omega+1}}$$
        Substituting this optimal $d$ back into the cost formula gives the total complexity:
        $$\text{Total Time} = O(|E| \cdot d) = O\left(|E| \cdot |E|^{\frac{\omega-1}{\omega+1}}\right) = \mathbf{O\left(|E|^{\frac{2\omega}{\omega+1}}\right)}$$
        Using $\omega \approx 2.373$, the exponent is $\frac{2(2.373)}{2.373 + 1} \approx 1.407$, giving a runtime of **$O(|E|^{1.41})$** for sparse graphs. $\blacksquare$

---

## 27. Practice Session Theorems / משפטים משיעורי תרגול

### 27.1 Unique Minimum Cut Verification / אימות ייחודיות החתך המינימלי
*   **Theorem Statement:** For a flow network $G = (V, E)$ with a max flow $f$, let $S_f$ be the set of nodes reachable from $s$ in the residual graph $G_f$, and $T_f$ be the set of nodes that can reach $t$ in $G_f$. The minimum $(s, t)$-cut is unique if and only if $S_f \cup T_f = V$.
*   **Proof:**
    *   **Forward Direction ($\implies$):** Let $(S, T)$ be any minimum $(s, t)$-cut. For any $u \in S$ and $v \in T$, we must have $f(u, v) = c(u, v)$ and $f(v, u) = 0$, which means $c_f(u, v) = 0$ in the residual network $G_f$. Thus, there are no edges in $G_f$ from $S$ to $T$.
        1. Since $s \in S$ and no edges leave $S$ in $G_f$, all nodes reachable from $s$ must be in $S$. Therefore, $S_f \subseteq S$.
        2. Since $t \in T$ and no edges enter $T$ in $G_f$, any node that can reach $t$ must be in $T$. Therefore, $T_f \subseteq T \implies S \subseteq V \setminus T_f$.
    *   This gives the sandwich bounds for any minimum cut $S$:
        $$S_f \subseteq S \subseteq V \setminus T_f$$
    *   If $S_f \cup T_f = V$, then $S_f = V \setminus T_f$, which forces $S = S_f$, proving the minimum cut is unique.
    *   **Backward Direction ($\impliedby$):** If $S_f \cup T_f \neq V$, there exists some node $v \notin S_f \cup T_f$. We can construct two distinct partitions: $S_1 = S_f$ and $S_2 = V \setminus T_f$. Both $S_1$ and $S_2$ are valid minimum cuts since they satisfy the sandwich bounds. Since $v \notin S_1$ but $v \in S_2$, they are distinct cuts, meaning the min-cut is not unique. $\blacksquare$

### 27.2 Kruskal Edge Sorting Theorem / משפט מיון הקשתות של קרוסקל
*   **Theorem Statement:** For any valid minimum spanning tree $T$ of a graph $G = (V, E)$, there exists a sorting order $\pi$ of the edges of $G$ such that Kruskal's algorithm on $\pi$ returns exactly $T$.
*   **Proof:**
    1. Let $T = (V, E_T)$ be a minimum spanning tree of $G$.
    2. Sort the edges of $G$ primarily by weight in ascending order.
    3. To break ties among edges of the same weight, place edges that belong to $E_T$ *before* edges that do not belong to $E_T$.
    4. Run Kruskal's algorithm on this sorted sequence.
    5. During Kruskal's execution, when considering edges of weight $w$, all tree edges of weight $w$ are examined before any non-tree edges of weight $w$. Since $T$ is acyclic, Kruskal's will successfully add every tree edge of weight $w$ to the forest. Once all tree edges of weight $w$ are added, any non-tree edge of weight $w$ must form a cycle with the already-added tree edges of weight $w$ and smaller weight edges, and will thus be rejected.
    6. Therefore, the algorithm returns exactly the tree $T$. $\blacksquare$

### 27.3 Reverse-Delete MST Algorithm Correctness / נכונות אלגוריתם מחיקה הפוכה (Reverse-Delete)
*   **Theorem Statement:** The Reverse-Delete algorithm (sorting edges in descending order and deleting an edge if it does not disconnect the graph) correctly finds a minimum spanning tree of $G$.
*   **Proof:**
    *   An edge $e$ is deleted if and only if it is part of a cycle (since removing an edge from a cycle cannot disconnect the graph).
    *   Because we check edges in descending order of weight, the deleted edge must be the heaviest edge on that cycle.
    *   By the **Cycle Property**, the heaviest edge on any cycle cannot belong to the MST. Thus, all deletions are correct, and the remaining edges form a minimum spanning tree. $\blacksquare$

### 27.4 Randomized Hitting Set Bounds / חסמי קבוצה פוגעת אקראית
*   **Theorem Statement:** Given a universal set $U$ of size $n$, and a family of $k$ subsets $\mathcal{S} = \{S_1, \dots, S_k\}$ of $U$ such that each subset satisfies $|S_i| \ge R$. A sample $H \subseteq U$ of $m \ge \frac{n}{R} \ln(k/\epsilon)$ elements selected uniformly at random is a Hitting Set (i.e. $H \cap S_i \neq \emptyset$ for all $i \in [1, k]$) with probability $\ge 1 - \epsilon$.
*   **Proof:**
    1. For a single choice of an element $x \in U$ and any fixed set $S_i$:
       $$\Pr(x \in S_i) = \frac{|S_i|}{n} \ge \frac{R}{n}$$
    2. The probability that none of the $m$ random choices lands in $S_i$ is:
       $$\Pr(H \cap S_i = \emptyset) = \left(1 - \frac{|S_i|}{n}\right)^m \le \left(1 - \frac{R}{n}\right)^m \le e^{-m R/n}$$
    3. To ensure that the probability that *any* subset is missed is at most $\epsilon$, we apply the Union Bound over all $k$ subsets:
       $$\Pr(H \text{ is not a Hitting Set}) = \Pr\left(\bigcup_{i=1}^k (H \cap S_i = \emptyset)\right) \le \sum_{i=1}^k \Pr(H \cap S_i = \emptyset) \le k \cdot e^{-m R/n}$$
    4. Setting the upper bound to $\epsilon$:
       $$k \cdot e^{-m R/n} \le \epsilon \implies e^{-m R/n} \le \frac{\epsilon}{k} \implies -m \frac{R}{n} \le \ln\left(\frac{\epsilon}{k}\right) \implies m \ge \frac{n}{R} \ln\left(\frac{k}{\epsilon}\right) \quad \blacksquare$$


## 28. Linear Programming & Low-Dimensional Seidel's LP / תכנות ליניארי ואלגוריתם סיידל במימד נמוך

### 28.1 Convexity and Half-Plane Intersections / קמורות וחיתוכי חצאי-מישורים
*   **Theorem Statement:** The intersection of $n$ half-planes forms a convex set.
*   **Proof:** 
    1. Let $S = \bigcap_{i=1}^n h_i$, where each $h_i$ is a half-plane.
    2. A half-plane $h_i = \{(x, y) \mid a_i x + b_i y \le c_i\}$ is convex: for any $p, q \in h_i$, the line segment $\lambda p + (1-\lambda)q$ for $\lambda \in [0, 1]$ satisfies:
       $$a_i (\lambda p_x + (1-\lambda)q_x) + b_i (\lambda p_y + (1-\lambda)q_y) = \lambda (a_i p_x + b_i p_y) + (1-\lambda)(a_i q_x + b_i q_y) \le \lambda c_i + (1-\lambda)c_i = c_i$$
       Thus, $\lambda p + (1-\lambda)q \in h_i$.
    3. Since the intersection of any collection of convex sets is convex, the feasible region $S$ is a convex set. $\blacksquare$

### 28.2 The 2D LP Incremental Step Invariant / אינווריאנטת השלב האינקרמנטלי בתכנות ליניארי דו-מימדי
*   **Theorem Statement:** Let $C_{i-1}$ be the convex feasible region of the first $i-1$ constraints and let $v_{i-1}$ be its optimal vertex. If $v_{i-1} \notin h_i$ and the new feasible region $C_i = C_{i-1} \cap h_i \neq \emptyset$, then the new optimal vertex $v_i$ must lie on the bounding line $\ell_i$ of $h_i$.
*   **Proof by Contradiction:**
    1. Assume for contradiction that the new optimal vertex $v_i \notin \ell_i$, meaning it lies strictly inside the half-plane $h_i$.
    2. Draw a line segment connecting the old optimum $v_{i-1}$ and the new optimum $v_i$:
       $$P = \{ \lambda v_{i-1} + (1-\lambda)v_i \mid \lambda \in [0, 1] \}$$
    3. Since both $v_{i-1}$ and $v_i$ belong to $C_{i-1}$, and $C_{i-1}$ is convex, the entire segment $P$ is contained in $C_{i-1}$.
    4. The objective function $f_{\vec{c}}(x) = \vec{c} \cdot x$ is linear and monotonic. Since $v_{i-1}$ was the unique optimizer in $C_{i-1}$, any step away from $v_{i-1}$ along the segment $P$ towards $v_i$ must strictly decrease the objective value:
       $$f_{\vec{c}}(x) \text{ decreases monotonically as } \lambda \to 0 \implies f_{\vec{c}}(v_{i-1}) > f_{\vec{c}}(x) > f_{\vec{c}}(v_i) \quad \forall x \in P \setminus \{v_{i-1}, v_i\}$$
    5. Since $v_{i-1} \notin h_i$ (violates the new constraint) and $v_i \in h_i$ (satisfies it), the segment $P$ must cross the boundary line $\ell_i$ of $h_i$ at some intermediate point $q = \lambda^* v_{i-1} + (1-\lambda^*)v_i$ for some $\lambda^* \in (0, 1)$.
    6. Since $q \in P \subseteq C_{i-1}$ and $q \in \ell_i \subset h_i$, it satisfies all constraints: $q \in C_i$.
    7. Since $\lambda^* > 0$, we have $f_{\vec{c}}(q) > f_{\vec{c}}(v_i)$.
    8. This contradicts that $v_i$ is the optimal vertex of $C_i$. Thus, our assumption was incorrect, and $v_i$ must lie on the bounding line $\ell_i$. $\blacksquare$

### 28.3 Inductive Proof of Seidel's LP Recurrence / הוכחת אינדוקציה של נוסחת הנסיגה של סיידל
*   **Theorem Statement:** The recurrence:
    $$T(d, n) \le O(d \cdot n) + \sum_{i=1}^n \frac{d}{i} T(d-1, i-1)$$
    has the solution $T(d, n) = O(d! \cdot n)$ for any constant dimension $d \ge 1$.
*   **Proof by Induction on $d$:**
    1. **Base Case ($d=1$):**
       In 1 dimension, the algorithm runs in $T(1, n) = O(n)$ time.
    2. **Inductive Step:**
       Assume that for dimension $d-1$, the expected runtime satisfies $T(d-1, k) \le c_0 \cdot (d-1)! \cdot k$ for some constant $c_0$.
       Substituting this hypothesis into the recurrence:
       $$T(d, n) \le c_1 \cdot d \cdot n + \sum_{i=1}^n \frac{d}{i} \left( c_0 \cdot (d-1)! \cdot (i-1) \right)$$
    3. Simplify the terms inside the summation:
       $$\sum_{i=1}^n \frac{d \cdot (d-1)! \cdot c_0 \cdot (i-1)}{i} = c_0 \cdot d! \sum_{i=1}^n \frac{i-1}{i}$$
    4. Since $\frac{i-1}{i} < 1$, we can upper-bound the sum:
       $$T(d, n) \le c_1 \cdot d \cdot n + c_0 \cdot d! \sum_{i=1}^n 1 = c_1 \cdot d \cdot n + c_0 \cdot d! \cdot n$$
    5. For a sufficiently large constant $c \ge \max(c_1, c_0)$, we get:
       $$T(d, n) \le c \cdot d! \cdot n$$
       Thus, the expected runtime is $O(d! \cdot n)$. $\blacksquare$
