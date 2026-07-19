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
    - <strong>Incorrect Greedy Heuristic:</strong> Deleting an endpoint of the heaviest saturated edge crossing the min-cut does not guarantee the maximum flow reduction. <br>
      &nbsp;&nbsp;<em>Counterexample:</em> If all flow must pass through a single bottleneck vertex z upstream, deleting z reduces the max flow to 0. However, the heaviest saturated edge crossing the min-cut (a,b) might have capacity less than the total flow, so deleting a or b would only reduce the flow partially, making z the optimal deletion instead.<br>
    - <strong>Missing Systematic Evaluation:</strong> To find the correct vertex, you must systematically evaluate the new max flow for each candidate v ∈ V \ {s,t} by zeroing its flow and running augmenting paths in the residual graph of G \ {v}.
  </td></tr>
</table>
</div>


---

## Question 2 [20 Points]

$$
\displaylines{
b-a = c-b\\
2b-a = c\\
2b = a + c\\
b = \frac{a+c}{2}
}
$$
The input is A and output if there are 3 values that make an arithmetric progression.
We will make 2 polynomials A and B
$$
\displaylines{
A(x) = \sum_{a \in A} x^{a}\\
B(x)=\sum_{a \in A} x^{a}\\
\text{We will use FFT to multiply both of the polynomials resulting in C(x)}\\
\text{C(x) will have all the possible sums of the sub set in the exponents}\\
\text{We will perform a linear search over the resulting polynomial, if the coefficient is bigger equal to 3}\\
\text{we will check if there exists a value which is the power of that coefficient divided by 3}\\
\text{if such exists we will set that value to be b and we know there exists another pair a,c}\\
\text{because the coefficint was bigger equal 3}
}
$$
$$
\displaylines{
\text{Proof}\\
\text{For any }x^{i} \text{ its coefficient will be the sum of the multiplication of other coefficients that sum to i}\\
\text{given that in a and b have coefficients 1, the resulting coefficient for } x^{i} \text{ will increase per combination,}\\
\text{so we are effectively counting combinations in the original subset A that arrive at i, if there exists a}\\
\text{value that is half of i then there will be at least 1 combination, if the coefficient is at least 3}\\
\text{and there is a value half of i then there is 1 combination that is half of i and another combination}\\
\text{that contributes 2 (2 ways to sum to it, x+y = i and y+x = i), therefore by what was proved at the}\\
\text{beggining } b = \frac{a+c}{2}, \text{we have an arithmetric progression}
}
$$

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Question 2 Score:</strong></td><td><strong style="color: #2e7d32;">18 / 20</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #2e7d32;">
    - <strong>Excellent Solution!</strong> Your identification of the polynomial squaring (C(x) = A²(x)) using FFT to find sums of elements in A is completely correct.<br>
    - <strong>Correct Coefficient Logic:</strong> You correctly identified that for b ∈ A, the trivial pair (b,b) contributes 1 to the coefficient of x<sup>2b</sup>, while any non-trivial AP pair (a,c) with a ≠ c contributes 2 (due to symmetry: a+c and c+a). Thus, an AP exists if and only if the coefficient of x<sup>2b</sup> is ≥ 3.<br>
    - <strong>Minor Terminology/Typo Deductions (-2):</strong> You used "divided by 3" instead of "divided by 2" when describing the search in lines 47-49 (which you correctly formulated as "half of i" in the proof). Also, "power of that coefficient" is a translation slip for the <em>exponent of that term</em> (Hebrew: חזקה).
  </td></tr>
</table>
</div>


---

## Question 3 [20 Points]

We know that if we take this edge with the increased weight we will form 2 connected components, we will use bfs from each of the endpoints of the edge in the tree without that edge to find the vertices on each of the connected components, this will take $2O(|V|+|E|)$ but we know its really $O(|V|+|E|)$ after that we will arbitrarily create 2 inverse trees union finds that will take O(|V|) and the find will take O(1), after that we will go over the edges in the tree and use find to see if each of the endpoints are in different connected components and we will save the one with the lowest weight, after that we will use the one with the lowest weight to replace (if we find a different one ) the edge with increased weight, we know by the lemmas of MST that putting a new lightest edge between 2 MSTs in a forest will make a new MST.
The time complexity will be O(|V|+|E|)

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Question 3 Score:</strong></td><td><strong style="color: #e65100;">14 / 20</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #e65100;">
    - <strong>Correct Core Logic:</strong> Removing the modified edge splits the MST into two components, and the new MST is formed by re-connecting them using the minimum-weight crossing edge (Cut Property).<br>
    - <strong>Scan Bug (-4):</strong> You wrote "go over the edges in the tree" to find the min-weight replacement. The alternative crossing edges lie in G \ T, not in the tree itself. You must scan all edges in the original graph G.<br>
    - <strong>Unnecessary Data Structure (-2):</strong> Using Union-Find here is overcomplicated. You can simply record vertex component IDs (e.g. 1 or 2) in a boolean array during the BFS/DFS, allowing O(1) membership checks without any overhead.
  </td></tr>
</table>
</div>

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
| **Q2** | Arithmetic Progressions via FFT | **18** | 20 | Excellent polynomial squaring and coefficient counting. Small typos in description. |
| **Q3** | Dynamic MST Weight Increase | **14** | 20 | Correct cut split logic, but scanned tree edges instead of graph edges. |
| **Q4** | Universal Hashing Collision Bounds | **0** | 20 | Unanswered |
| **Q5** | Linear Programming & Geometry | **0** | 20 | Unanswered |
| **Total** | **Final Score** | **36** | **100** | **Grade: 36.0%** |
