# 📝 Ester Practice Exam 1 - Answers Sheet

Please write your answers in the designated placeholders below. When you are finished, type `Finished` or `Check my answers` in the chat to submit them for grading.

---

## Question 1 [20 Points]

Given that the graph has all positive capacities we are trying to take the edges that are the most influential to the flow, the vertex has infinite flow so its the edges we care about.
We can get the min cut max flow and take the edges that have the most flow in all of the cuts.
We will run dinics algorithm to find the max flow min cut of the network, from this we will get the residual graph $G_{f}$ that contains the vertices and non saturated edges, we will run bfs from s to find the vertices that have saturated edges and we will take the edge with the highest capacity and take either of the vertices that are part of that edge, thus we will take the heaviest saturated edge

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Question 1 Score:</strong></td><td><strong style="color: #ef5350;">4 / 20</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #ef5350;">
    - <strong>Good Start:</strong> Running Dinic's algorithm first is correct.<br>
    - <strong>Incorrect Greedy Heuristic:</strong> Deleting an endpoint of the heaviest saturated edge crossing the min-cut does not guarantee the maximum flow reduction. 
      <em>Counterexample:</em> If all flow must pass through a single bottleneck vertex $z$ upstream, deleting $z$ reduces the max flow to 0. However, the heaviest saturated edge crossing the min-cut $(a,b)$ might have capacity less than the total flow, so deleting $a$ or $b$ would only reduce the flow partially, making $z$ the optimal deletion instead.<br>
    - <strong>Missing Systematic Evaluation:</strong> To find the correct vertex, you must systematically evaluate the new max flow for each candidate $v \in V \setminus \{s,t\}$ by zeroing its flow and running augmenting paths in the residual graph of $G \setminus \{v\}$.
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
| **Q1** | Vertex Deletion in Flow Networks | **4** | 20 | Run Dinic's once, but greedy heuristic (heaviest edge endpoint deletion) is incorrect. |
| **Q2** | Arithmetic Progressions via FFT | **0** | 20 | Unanswered |
| **Q3** | Dynamic MST Weight Increase | **0** | 20 | Unanswered |
| **Q4** | Universal Hashing Collision Bounds | **0** | 20 | Unanswered |
| **Q5** | Linear Programming & Geometry | **0** | 20 | Unanswered |
| **Total** | **Final Score** | **4** | **100** | **Grade: 4.0%** |
