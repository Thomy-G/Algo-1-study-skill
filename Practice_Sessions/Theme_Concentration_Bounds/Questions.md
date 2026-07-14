# Algorithms 1 (89-220) - Theme: Advanced Randomized Algorithms & Concentration Inequalities

Welcome to the practice session on **Advanced Randomized Algorithms & Concentration Inequalities**. This topic deals with advanced probability bounds (Markov, Chebyshev, Chernoff, Mill's Inequality) and randomized reductions, which are very common in research and advanced homework but less seen in standard exams.

---

## Question 1: Mill's Inequality and Normal Tails (30 Points)

*   **a) [15 Points]** State **Mill's Inequality** for a standard normal random variable $Z \sim N(0,1)$. Prove that for any $t > 0$:
    $$P(Z \ge t) \le \frac{1}{t \sqrt{2\pi}} e^{-t^2/2}$$
*   **b) [15 Points]** Compare the bound obtained from Mill's Inequality for $P(Z \ge 3)$ to the bound obtained by applying **Chebyshev's Inequality** (using the variance of $Z$). Which bound is tighter, and by what factor?

---

## Question 2: Randomized Hitting Set (35 Points)

Let $V$ be a ground set of size $n$. Suppose we are given a family of subsets $\mathcal{S} = \{S_1, S_2, \dots, S_m\}$ of $V$, where each subset has size at least $\epsilon n$ (for some constant $0 < \epsilon < 1$).
We construct a sample set $R \subset V$ by selecting $s$ elements from $V$ independently and uniformly at random with replacement.

*   **a) [15 Points]** Calculate the probability that the sample set $R$ fails to intersect a specific subset $S_i \in \mathcal{S}$. Show that this probability is at most $e^{-\epsilon s}$.
*   **b) [20 Points]** Prove that if we choose a sample size $s$ satisfying:
    $$s \ge \frac{1}{\epsilon} \ln \left( \frac{m}{\delta} \right)$$
    then with probability at least $1 - \delta$, the set $R$ will intersect **every** subset in $\mathcal{S}$ (i.e., $R$ is a valid $\epsilon$-hitting set).

---

## Question 3: Chebyshev's Higher Moments (35 Points)

Suppose we have a sum $Y = \sum_{i=1}^n X_i$ of independent random variables $X_i$, where each $X_i$ takes values in $\{-1, 1\}$ with equal probability ($P(X_i = 1) = P(X_i = -1) = 1/2$).
*   **a) [15 Points]** Calculate $E[Y]$, $E[Y^2]$, and $E[Y^4]$.
*   **b) [20 Points]** Use Chebyshev's inequality with the **4th moment** to prove a tighter upper bound on the tail probability $P(|Y| \ge t)$ for $t > 0$:
    $$P(|Y| \ge t) \le \frac{3n^2}{t^4}$$
    Explain why this bound is tighter than the standard Chebyshev bound (using the 2nd moment) when $t$ is large.
