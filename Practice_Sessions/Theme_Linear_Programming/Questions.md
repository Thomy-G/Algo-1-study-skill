# Algorithms 1 (89-220) - Theme: Linear Programming & Welzl's Algorithm

Welcome to the practice session on **Linear Programming & Welzl's Algorithm**. These topics are highly important in the curriculum but rarely appear in past exams.

---

## Question 1: LP Formulations and Feasible Regions (30 Points)

A factory produces two types of algorithms: **Dijkstra-Bots** ($x_1$) and **FFT-Bots** ($x_2$). 
*   Producing one Dijkstra-Bot requires 2 units of RAM and 1 unit of CPU.
*   Producing one FFT-Bot requires 1 unit of RAM and 3 units of CPU.
*   The factory has a daily limit of 8 units of RAM and 9 units of CPU.
*   The profit from selling one Dijkstra-Bot is 30 NIS, and from selling one FFT-Bot is 40 NIS.

*   **a) [10 Points]** Formulate this problem as a Linear Program (LP) in **standard maximization form** (define the variables, objective function, and constraints).
*   **b) [10 Points]** Draw the feasible region in the $(x_1, x_2)$ plane. Find all the vertices (extreme points) of the feasible region.
*   **c) [10 Points]** Find the optimal production plan $(x_1, x_2)$ that maximizes the daily profit, and calculate the maximum profit.

---

## Question 2: Seidel's Randomized LP Algorithm (35 Points)

Let $HL$ be a set of $n$ linear constraints in $d$ dimensions, and let $c$ be the objective vector we wish to maximize. Seidel's algorithm solves this LP recursively.
*   **a) [15 Points]** Describe the recursive step of Seidel's algorithm. Explain what happens when the randomly chosen constraint $h \in HL$ is violated by the optimal solution of the LP on $HL \setminus \{h\}$.
*   **b) [20 Points]** Let $T(d, n)$ be the expected runtime of Seidel's algorithm in $d$ dimensions with $n$ constraints. Write the recurrence relation for $T(d, n)$ and prove that for a constant dimension $d$, the expected runtime is $O(n)$ (i.e. linear in the number of constraints).

---

## Question 3: Welzl's Smallest Enclosing Disc (35 Points)

Welzl's algorithm finds the Smallest Enclosing Disc (SED) of a set of $P$ points in $O(n)$ expected time.
*   **a) [15 Points]** Explain the role of the support set (boundary set) $R$ in the recursive function `Welzl(P, R)`. What is the maximum size that $R$ can reach, and why?
*   **b) [20 Points]** Prove that Welzl's algorithm runs in expected $O(n)$ time. Define the backward analysis technique and explain how it is used to bound the probability of entering the recursive step.
