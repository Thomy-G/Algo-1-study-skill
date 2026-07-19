# 📝 Ester Practice Exam 1 - Questions

---

## Question 1 [20 Points]
Let $G = (V, E)$ be a directed flow network with a source $s$, a sink $t$, and integer edge capacities $c(e) > 0$ for all $e \in E$. 
Suppose we want to find a vertex $v \in V \setminus \{s, t\}$ whose deletion from the graph (along with all its incoming and outgoing edges) reduces the maximum flow value from $s$ to $t$ by the largest amount.

*   **Your Task:** Describe an algorithm to find such a vertex $v$ that runs in $O(|V|^2 |E|)$ time. Prove its correctness and analyze its complexity.

---

## Question 2 [20 Points]
Let $A$ be a subset of $\{1, 2, \dots, M\}$. We say that three distinct elements $a, b, c \in A$ form an **arithmetic progression** if $b - a = c - b$.

*   **Your Task:** Describe an algorithm that determines if there exist three distinct elements in $A$ that form an arithmetic progression. Your algorithm must run in $O(M \log M)$ time. Explain its correctness and analyze its complexity.

---

## Question 3 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph with distinct edge weights, and let $T$ be its unique Minimum Spanning Tree (T).
Suppose we increase the weight of a single edge $e_0 \in T$ to a new weight $w'(e_0) > w(e_0)$.

*   **Your Task:** Describe an algorithm, as efficient as possible, to find the new Minimum Spanning Tree of the graph after this change. Prove the correctness of your algorithm and analyze its complexity.

---

## Question 4 [20 Points]
Let $\mathcal{H}$ be a universal family of hash functions mapping a universe $U$ to a table of size $m$. We choose a hash function $h \in \mathcal{H}$ uniformly at random to hash a set $S \subseteq U$ of $n$ keys.

*   **Your Task:** Prove that if the table size is $m = n^2$, then the probability that at least one collision occurs (i.e., there exist distinct $x, y \in S$ such that $h(x) = h(y)$) is at most $1/2$.

---

## Question 5 [20 Points]
A manufacturing company produces two products, $X$ and $Y$. 
- Each unit of $X$ requires 2 hours of machine time and 1 hour of labor.
- Each unit of $Y$ requires 1 hour of machine time and 3 hours of labor.
- The company has a maximum of 8 hours of machine time and 9 hours of labor available per day.
- The profit is \$40 per unit of $X$ and \$50 per unit of $Y$.

*   **Your Task:**
    1. Formulate this problem as a Linear Program (LP) in standard maximization form.
    2. Graph the feasible region in the 2D plane and identify all vertex coordinates.
    3. Find the optimal daily production plan and the maximum profit geometrically.
