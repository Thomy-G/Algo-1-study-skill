# 📝 2025 Summer Moed B - Answers Sheet

Please write your answers in the designated placeholders below. When you are finished, type `Finished` or `Check my answers` in the chat to submit them for grading.

---

## Question 1: Vertex Capacities & Smallest Edge-Count Min Cut (25 Points)

### Part a) [13 Points]
*Describe the max-flow algorithm with vertex capacities, prove correctness, and analyze complexity.*

We will take the original vertices of the original graph and we will split them into 2, and make an edge with the vertex capacity between them and run a standard flow algorithm

$$
\displaylines{
V' = \Set{ v_{in}, v_{out} |v \in V }\\
E' = \Set{ (u_{out}, v_{in}) | (u,v) \in E } \cup \Set{ (v_{in},v_{out}) | v \in V}\\
c'(v) = \begin{cases}
c((u,v)) & (u_{out}, v_{in}) \\
l(u) & (u_{in}, u_{out})
\end{cases}\\
\text{Now that we have a standard flow network we can use a standard algorithm to find the flow}\\
\text{We will use dinics to get the max flow}\\
\text{The correctness in this algorithm is based on the fact that having an internal vertex capacity}\\
\text{is the same as having an edge with that capacity go from the input to the vertex to the output to}\\
\text{the vertex}\\
|V'| = 2|V|\\
|E'| = |E| + |V|\\
\text{Using dinics:}\\
O((2|V|)^{2}(|E|+|V|)) = O(4|V|^{3}+|V|^{2}|E|)
}
$$

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Part a Score:</strong></td><td><strong style="color: #ef6c00;">9 / 13</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #ef6c00;">
    - <strong>Incomplete Source/Sink Definition:</strong> Since you split all vertices (including $s,t$), you must explicitly set $s' = s_{out}$ and $t' = t_{in}$ in $G'$.<br>
    - <strong>Notation Typos in $c'$:</strong> The capacity function should be $c'(e)$, not $c'(v)$. Additionally, since you split $s,t$, you must set $l(s) = l(t) = \infty$ so their capacities aren't bounded.<br>
    - <strong>Hand-wavy Proof:</strong> While the intuition is correct, a formal proof of correctness requires showing that any valid flow $f$ in $G$ corresponds to a valid flow $f'$ in $G'$ of the same value.
  </td></tr>
</table>
</div>

---

### Part b) [12 Points]
*Describe the algorithm for min cut with minimum edges, and explain correctness.*

*Write your answer here:*
All of the minimum cuts have the same capacity

---

## Question 2: Dynamic MST Edge Weight Decrease (20 Points)

*Describe your algorithm, prove correctness, and analyze complexity.*

*Write your answer here:*


---

## Question 3: Bottleneck Paths in a DAG (15 Points)

*Describe the O(|V|+|E|) bottleneck path algorithm and explain correctness.*

Given the time complexity the only traversal algorithms available to us are BFS and DFS, we will use them from s to find all possible paths to t and keep track of the heaviest weight of each path and then compare the highest weight of each of them and take the minimum as our bottleneck

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Question 3 Score:</strong></td><td><strong style="color: #c62828;">2 / 15</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #c62828;">
    - <strong>Exponential Complexity:</strong> Finding all paths takes $O(2^{|V|})$ time in the worst case (since the number of paths in a DAG can be exponential), which violates the $O(|V| + |E|)$ constraint.<br>
    - <strong>Missing DP & Topological Sort:</strong> To achieve $O(|V| + |E|)$, you must process the vertices in topological order and use Dynamic Programming: $DP[v] = \min(DP[v], \max(DP[u], w(u,v)))$.
  </td></tr>
</table>
</div>

---

## Question 4: Dynamic Programming Egg Dropping (20 Points)

*Describe the recursive formula, DP algorithm, and analyze complexities.*

*Write your answer here:*


---

## Question 5: Expected Value in Secretary Problems (20 Points)

### Part a) [10 Points]
*Calculate the expected number of replacements for K=1.*

*Write your answer here:*


---

### Part b) [10 Points]
*Calculate the expected number of replacements for K >= 1.*

*Write your answer here:*


---

## 📊 Exam Tally & Final Score

| Part / Question | Description | Score | Max Points | Feedback |
| :--- | :--- | :--- | :--- | :--- |
| **Q1a** | Vertex Capacities | **9** | 13 | Correct reduction & complexity; notation typos & source/sink details missing. |
| **Q1b** | Min Cut Min Edges | **0** | 12 | Unanswered |
| **Q2** | Dynamic MST | **0** | 20 | Unanswered |
| **Q3** | Bottleneck DAG | **2** | 15 | Exponential time complexity due to finding all paths. |
| **Q4** | Egg Dropping DP | **0** | 20 | Unanswered |
| **Q5a** | Secretary K=1 | **0** | 10 | Unanswered |
| **Q5b** | Secretary K >= 1 | **0** | 10 | Unanswered |
| **Total** | **Final Score** | **11** | **100** | **Grade: 11.0%** |
