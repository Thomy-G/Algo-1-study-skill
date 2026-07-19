# 📝 Ester Practice Exam 2 - Answers Sheet

Please write your answers in the designated placeholders below. When you are finished, type `Finished` or `Check my answers` in the chat to submit them for grading.

---

## Question 1 [20 Points]


We will make a bipartite flow graph to represent the work
We will have vertices for source and sink s,t and a vertex for every worker and a vertex for each job
We will connect s to each worker j with an edge of capacity $c_{j}$, we will connect each worker j with each task i with a capacity with value $h_{ij}$ only if $(i,j) \in S$ and we will connect every job to t with a capacity of $d_{i}$ and we will call the job completed only if the capacity is fully saturated in a max flow algorithm

We can prove this is a good representation because by running max flow we are maximizing the amount of work done in a day (not necesarilly most jobs finished), we provide the amount of hours the workers do, then divide it between the projects by crossing the edge between workers and jobs and finally we use the amount of hours per job to cross to t and the saturated ones are the ones that are finished

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Q1a Score (Design):</strong></td><td><strong style="color: #2e7d32;">6 / 6</strong></td></tr>
  <tr><td><strong>Q1b Score (Demands):</strong></td><td><strong style="color: #2e7d32;">7 / 7</strong></td></tr>
  <tr><td><strong>Q1c Score (Proof):</strong></td><td><strong style="color: #e65100;">5 / 7</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: var(--text-muted);">
    - <strong>Excellent Reduction:</strong> Your standard flow model (s → W_j → T_i → t) is highly elegant and avoids the complexity of lower-bound circulations entirely by checking if the max flow value equals ∑ d_i.<br>
    - <strong>Informal Proof (-2):</strong> For part c, your proof of correctness is a bit hand-wavy ("maximizing the amount of work done... the saturated ones are the ones that are finished"). A rigorous proof requires showing the formal bijection: (1) showing that any feasible task assignment yields a valid flow of value ∑ d_i, and (2) showing that a max flow of value ∑ d_i must saturate all edges (T_i, t), yielding a valid assignment.
  </td></tr>
</table>
</div>

---

## Question 2 [20 Points]

### Part a) [6 Points]
*Write your answer here:*

### Part b) [8 Points]
*Write your answer here:*

### Part c) [6 Points]
*Write your answer here:*

---

## Question 3 [20 Points]

### Part a) [10 Points]
*Write your answer here:*

### Part b) [10 Points]
*Write your answer here:*

---

## Question 4 [20 Points]

### Part a) [12 Points]
*Write your answer here:*

### Part b) [8 Points]
*Write your answer here:*

---

## Question 5 [20 Points]

### Part a) [8 Points]
*Write your answer here:*

### Part b) [7 Points]
*Write your answer here:*

### Part c) [5 Points]
*Write your answer here:*

---

## 📊 Exam Tally & Final Score

| Part / Question | Description | Score | Max Points | Feedback |
| :--- | :--- | :--- | :--- | :--- |
| **Q1a** | Flow Network Design | **6** | 6 | Elegant bipartite standard flow graph model. |
| **Q1b** | Demand Lower Bounds conversion | **7** | 7 | Smart saturation criteria check (max flow = sum of demands). |
| **Q1c** | Reduction Correctness Proof | **5** | 7 | Correct idea, but proof was informal and lacked formal bijection steps. |
| **Q2a** | Wildcard Matching formulation | **0** | 6 | Unanswered |
| **Q2b** | FFT Polynomial expansion | **0** | 8 | Unanswered |
| **Q2c** | Block Partitioning Complexity | **0** | 6 | Unanswered |
| **Q3a** | Prim Priority Queue Modification | **0** | 10 | Unanswered |
| **Q3b** | Prim 1-2 complexity proof | **0** | 10 | Unanswered |
| **Q4a** | Karger single-run probability | **0** | 12 | Unanswered |
| **Q4b** | Karger multi-run bounds | **0** | 8 | Unanswered |
| **Q5a** | Seidel LP description | **0** | 8 | Unanswered |
| **Q5b** | Seidel LP expected complexity | **0** | 7 | Unanswered |
| **Q5c** | Seidel LP 1D base case | **0** | 5 | Unanswered |
| **Total** | **Final Score** | **18** | **100** | **Grade: 18.0%** |
