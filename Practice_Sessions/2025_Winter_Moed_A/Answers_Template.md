# 📝 2025 Winter Moed A - Answers Sheet

Please write your answers in the designated placeholders below. When you are finished, type `Finished` or `Check my answers` in the chat to submit them for grading.

---

## Question 1: Constrained Minimum Spanning Trees (20 Points)

### Part a) [6 Points]
*Define a Minimum Spanning Tree (MST) of G.*

*Write your answer here:*


---

### Part b) [7 Points]
*Describe an algorithm that finds an MST with at most 2 edges incident to $s$, prove correctness, and analyze complexity.*

*Write your answer here:*


---

### Part c) [7 Points]
*Describe an algorithm that finds an MST with at least 2 edges incident to $s$, prove correctness, and analyze complexity.*

*Write your answer here:*


---

## Question 2: Algo-Mania Coupon Collection Strategy (20 Points)

### Part a) [15 Points]
*Precisely analyze the expected total cost of the algorithm as a function of $n$ and $k$.*

For each trial we get that the chance of getting an uncollected card (Using option 1 is) 
$$
\displaylines{
X_{i}=\frac{n-i+1}{n}\\
\text{This is a geometric random variable, therefore:}\\
\mathbb{E}[X_{i}] = \frac{n}{n-i+1}\\
\text{Then For option 2 we get exactly the cards we want}\\
\text{So the cost will be:}\\
C = 1 \cdot \sum_{i=1}^{k} X_{i} + 5 \cdot n-k\\
\mathbb{E}[C] = \mathbb{E}\left[ 1 \cdot \sum_{i=1}^{k} X_{i} + 5 \cdot n-k \right]\\
\mathbb{E}[C] = \mathbb{E}\left[ 1 \cdot \sum_{i=1}^{k} X_{i}  \right]+ 5 \cdot n-k\\
\mathbb{E}[C] = \mathbb{E}\left[ 1 \cdot \sum_{i=1}^{k} X_{i}  \right]+ 5 \cdot n-k\\
\mathbb{E}[C] = \sum_{i=1}^{k}\mathbb{E}[  X_{i} ]+ 5 \cdot n-k\\
\mathbb{E}[C] = \sum_{i=1}^{k}\mathbb{E}[  X_{i} ]+ 5 \cdot n-k\\
\mathbb{E}[C] = \sum_{i=1}^{k} \frac{n}{n-i+1}+ 5 \cdot n-k\\
\mathbb{E}[C] = \left( \frac{n}{n}+\ldots+\frac{n}{n-k-1} \right)+ 5 \cdot n-k\\
\mathbb{E}[C] = n \frac{\mathcal{H}_{n}}{\mathcal{H}_{n-k-2}} + 5(n-k)\\

}
$$

<div align="right">
<table style="border: 1px solid #ddd; border-radius: 4px; background: rgba(130, 130, 130, 0.07); padding: 8px; font-size: 13px; font-family: system-ui; width: fit-content; text-align: left;">
  <tr><td><strong>Part a Score:</strong></td><td><strong style="color: #c62828;">3 / 15</strong></td></tr>
  <tr><td colspan="2" style="border-top: 1px dotted #ccc; padding-top: 4px; color: #c62828;">
    - <strong>Phase Reversal:</strong> Option 1 is used to collect the first n-k cards, and Option 2 is used to collect the remaining k cards. Your formula reversed these.<br>
    - <strong>Harmonic Algebra Error:</strong> You wrote a division of Harmonic numbers instead of subtraction.<br>
    - <strong>Index Off-by-One:</strong> The summation indices are incorrect due to the phase reversal.
  </td></tr>
</table>
</div>

---

### Part b) [5 Points]
*Find and prove the value of $k$ that minimizes the expected total cost.*

*Write your answer here:*


---

## Question 3: Matrix Row and Column Sums Realization (20 Points)

*Describe your algorithm, prove correctness, and analyze complexity.*

*Write your answer here:*


---

## Question 4: Min-Plus Matrix Multiplication (20 Points)

### Part a) [14 Points]
*Describe an $O(n^\omega)$ algorithm for $M = 1$, prove correctness, and analyze complexity.*

*Write your answer here:*


---

### Part b) [6 Points]
*Describe an $O(M^2 \cdot n^\omega)$ algorithm for $M \ge 1$, prove correctness, and analyze complexity.*

*Write your answer here:*


---

## Question 5: RNA Secondary Structure Folding (20 Points)

*Describe the recursive formula, explain correctness, and outline the DP algorithm with complexities.*

*Write your answer here:*


---

## 📊 Exam Tally & Final Score

| Part / Question | Description | Score | Max Points | Feedback |
| :--- | :--- | :--- | :--- | :--- |
| **Q1a** | Define MST | **0** | 6 | Unanswered |
| **Q1b** | MST with $\le 2$ edges | **0** | 7 | Unanswered |
| **Q1c** | MST with $\ge 2$ edges | **0** | 7 | Unanswered |
| **Q2a** | Expected Cost Formula | **3** | 15 | Reversal of phases; algebra errors with Harmonic numbers. |
| **Q2b** | Minimizing $k$ | **0** | 5 | Unanswered |
| **Q3** | Matrix Sums Realization | **0** | 20 | Unanswered |
| **Q4a** | Min-Plus for $M=1$ | **0** | 14 | Unanswered |
| **Q4b** | Min-Plus for $M \ge 1$ | **0** | 6 | Unanswered |
| **Q5** | RNA secondary structure | **0** | 20 | Unanswered |
| **Total** | **Final Score** | **3** | **100** | **Grade: 3.0%** |
