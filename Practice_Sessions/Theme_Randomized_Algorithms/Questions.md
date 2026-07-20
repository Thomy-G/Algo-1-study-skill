# 📝 Practice Session - Randomized Algorithms

Welcome to the practice session on **Randomized Algorithms**. Below are 3 questions covering core randomized topics (verification, selection, and data structures) designed to test your design, analysis, and proof skills.

---

## Question 1 [30 Points]

Suppose we are given three $n \times n$ matrices $A, B$, and $C$ over a field $\mathbb{F}_2$. We want to verify if $A \cdot B = C$. Computing the matrix product $A \cdot B$ directly takes $O(n^\omega)$ time (where $\omega \approx 2.373$), but we want a faster randomized approach.

*   **a) [10 Points]** Describe Freivalds' algorithm to verify if $A \cdot B = C$ in $O(n^2)$ time.
*   **b) [12 Points]** Prove that if $A \cdot B \neq C$, the probability of error (i.e., the algorithm incorrectly outputting that they are equal) when choosing a random vector $r \in \{0, 1\n$ is at most $1/2$.
*   **c) [8 Points]** Explain how to modify the algorithm to ensure the error probability is at most $\delta$ for any given $\delta > 0$, and analyze the resulting runtime complexity.

---

## Question 2 [35 Points]

The QuickSelect algorithm is used to find the $k$-th smallest element in an unsorted array of $n$ elements. In the randomized version, the pivot is chosen uniformly at random.

*   **a) [10 Points]** Provide the pseudocode for the Randomized QuickSelect algorithm.
*   **b) [10 Points]** Formulate the recurrence relation for the expected number of element comparisons $E[T(n)]$ performed by the algorithm.
*   **c) [15 Points]** Prove that the expected runtime of Randomized QuickSelect is $O(n)$ by showing that the recurrence solves to $E[T(n)] \le c \cdot n$ for some constant $c$.

---

## Question 3 [35 Points]

A Skip List is a randomized, layered linked list structure that supports search, insertion, and deletion.

*   **a) [10 Points]** Describe how elements are inserted into a Skip List and how the height of each element's tower is determined using independent coin flips.
*   **b) [12 Points]** Prove that the expected height (number of levels) of a Skip List containing $n$ elements is $O(\log n)$, and that the height is at most $c \log n$ with high probability (w.h.p.) for some constant $c$.
*   **c) [13 Points]** Prove that the expected search time for any key in a Skip List is $O(\log n)$ using **backwards analysis** (analyzing the search path by tracing it backwards from the target key up to the top-left element).
