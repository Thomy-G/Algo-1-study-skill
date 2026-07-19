# 📝 Ester Practice Exam 1 - Answers Sheet

Please write your answers in the designated placeholders below. When you are finished, type `Finished` or `Check my answers` in the chat to submit them for grading.

---

## Question 1 [20 Points]

Given that the graph has all positive capacities we are trying to take the edges that are the most influential to the flow, the vertex has infinite flow so its the edges we care about.
We can get the min cut max flow and take the edges that have the most flow in all of the cuts, but based on the complexity allowed we have to use an algorithm that doesn't actually find the max flow

We will do a topological sort of the vertices, that takes O(|V|+|E|) for every vertex, we will take the vertex that is unique in the top sort that splits the nodes effectively making the flow 0, if it doesn't exist we will take the vertices with the max possible flow based on max input edge and max output edge

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Question 1 Score:</strong></td><td><strong style="color: #c62828;">1 / 20</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #c62828;">
    - <strong>Wrong Algorithmic Assumption:</strong> A directed flow network is not necessarily a DAG, so a topological sort cannot be applied.<br>
    - <strong>Incorrect Logic:</strong> A vertex "splitting a top sort" or having high local edge capacity does not minimize the global max flow.<br>
    - <strong>Complexity Misunderstanding:</strong> Finding the max flow is necessary. Running max flow *once* on the original graph, and then running at most $F_v$ augmenting paths per candidate vertex $v$ achieves the required $O(|V| \cdot (|V| + |E|))$ time.
  </td></tr>
</table>
</div>


---

## Question 2 [20 Points]

*Write your answer here:*


---

## Question 3 [20 Points]

*Write your answer here:*


---

## Question 4 [20 Points]

*Write your answer here:*


---

## Question 5 [20 Points]

*Write your answer here:*


---

## 📊 Exam Tally & Final Score

| Part / Question | Description | Score | Max Points | Feedback |
| :--- | :--- | :--- | :--- | :--- |
| **Q1** | Vertex Deletion in Flow Networks | **1** | 20 | Wrong algorithm (topological sort is for DAGs only) and incorrect logic. |
| **Q2** | Arithmetic Progressions via FFT | **0** | 20 | Unanswered |
| **Q3** | Dynamic MST Weight Increase | **0** | 20 | Unanswered |
| **Q4** | Universal Hashing Collision Bounds | **0** | 20 | Unanswered |
| **Q5** | Linear Programming & Geometry | **0** | 20 | Unanswered |
| **Total** | **Final Score** | **1** | **100** | **Grade: 1.0%** |
