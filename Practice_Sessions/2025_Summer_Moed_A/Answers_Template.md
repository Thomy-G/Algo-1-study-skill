# 📝 2025 Summer Moed A - Answers Sheet

Please write your answers in the designated placeholders below. When you are finished, type `Finished` or `Check my answers` in the chat to submit them for grading.

---

## Question 1: Matrix Invariants & Graph Reductions (20 Points)

### Part a) [6 Points]
*Describe the matrix M with minimal 1s where M^2 is all 1s, and prove correctness.*

$$
\displaylines{
\text{For binary matrices:}\\
A \cdot B = C \implies C_{ij} = \bigvee_{k=1}^{n} (A_{ik} \land B_{kj})
}
$$

This means that the value of the M has to have a 1 in bot the same row and column

$$
\displaylines{
M^{2}_{ij} =  \bigvee_{k=1}^{n} (M_{ik} \land M_{kj}) \implies \forall i, j \exists k: M_{ik} = M_{kj} = 1 \implies \exists M_{ik} = M_{jk}^{T} = 1 \implies \\
\text{We need the value at each row at column where there is some k where both are always 1}\\
}
$$
Therefore we select a value $1\leq k\leq n$ where $\forall l\in[1,n]:M_{lk}=M_{kl}=1$ for example the cross where l = n/2 and we get for every case there number of 1s is 2n-1

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Part a Score:</strong></td><td><strong style="color: #2e7d32;">6 / 6</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #2e7d32;">
    - <strong>Correct Matrix & Proof:</strong> Successfully identified the optimal cross configuration with $2n-1$ ones and proved why it works.
  </td></tr>
</table>
</div>

---

### Part b) [7 Points]
*Describe the matrix M with minimal 1s where M^n is upper triangular 1s, and prove correctness.*

*Write your answer here:*


---

### Part c) [7 Points]
*Give the graph example with one negative edge where Dijkstra fails.*

*Write your answer here:*


---

## Question 2: Diameter of an Undirected Tree (20 Points)

### Part a) [10 Points]
*Describe the recursive formula and explain correctness.*

*Write your answer here:*


---

### Part b) [10 Points]
*Describe the DP algorithm and analyze complexities.*

*Write your answer here:*


---

## Question 3: Bottleneck Spanning Trees (20 Points)

### Part a) [10 Points]
*Prove that every MST is a BST.*

*Write your answer here:*


---

### Part b) [10 Points]
*Describe the O(|V|+|E|) BST tester algorithm and analyze complexity.*

*Write your answer here:*


---

## Question 4: Flow Networks and Cut Properties (20 Points)

### Part a) [10 Points]
*Prove or disprove: Max flow <= |E|/k in unit capacity networks.*

*Write your answer here:*


---

### Part b) [10 Points]
*Prove that the union/intersection cut is also a min cut.*

*Write your answer here:*


---

## Question 5: Tail Quicksort Complexity (20 Points)

### Part a) [5 Points]
*Prove that Tail_Quicksort correctly sorts.*

*Write your answer here:*


---

### Part b) [15 Points]
*Prove the O(n log n) expected comparisons.*

*Write your answer here:*


---

## 📊 Exam Tally & Final Score

| Part / Question | Description | Score | Max Points | Feedback |
| :--- | :--- | :--- | :--- | :--- |
| **Q1a** | Matrix $M^2 = J$ | **6** | 6 | Correctly identified cross configuration with $2n-1$ ones. |
| **Q1b** | Matrix $M^n$ Upper Tri. | **0** | 7 | Unanswered |
| **Q1c** | Dijkstra Counterexample | **0** | 7 | Unanswered |
| **Q2a** | Tree Diameter Formula | **0** | 10 | Unanswered |
| **Q2b** | Tree Diameter DP | **0** | 10 | Unanswered |
| **Q3a** | MST is BST Proof | **0** | 10 | Unanswered |
| **Q3b** | BST Tester $O(V+E)$ | **0** | 10 | Unanswered |
| **Q4a** | Max Flow $\le \|E\|/k$ | **0** | 10 | Unanswered |
| **Q4b** | Cut Union / Intersect | **0** | 10 | Unanswered |
| **Q5a** | Tail Quicksort Correctness | **0** | 5 | Unanswered |
| **Q5b** | Tail Quicksort Expected Comp. | **0** | 15 | Unanswered |
| **Total** | **Final Score** | **6** | **100** | **Grade: 6.0%** |
