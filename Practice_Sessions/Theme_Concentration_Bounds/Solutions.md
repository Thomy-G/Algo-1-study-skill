# Algorithms 1 (89-220) - Theme: Concentration Bounds - Solutions Manual

This document contains the official, rigorous solutions for the Advanced Randomized Algorithms & Concentration Inequalities practice session.

---

## Question 1: Mill's Inequality and Normal Tails (30 Points)

### Part a) [15 Points]
**Proof of Mill's Inequality:**
1. The probability that a standard normal variable $Z \sim N(0,1)$ satisfies $Z \ge t$ is defined by the integral:
   $$P(Z \ge t) = \int_t^\infty \frac{1}{\sqrt{2\pi}} e^{-x^2/2} dx$$
2. Since $t > 0$ and the variable of integration $x$ satisfies $x \ge t$, we can multiply the integrand by $\frac{x}{t} \ge 1$ to obtain an upper bound:
   $$P(Z \ge t) \le \int_t^\infty \frac{x}{t} \cdot \frac{1}{\sqrt{2\pi}} e^{-x^2/2} dx = \frac{1}{t \sqrt{2\pi}} \int_t^\infty x e^{-x^2/2} dx$$
3. We can compute the integral directly using substitution:
   $$\int_t^\infty x e^{-x^2/2} dx = \left[ -e^{-x^2/2} \right]_t^\infty = 0 - (-e^{-t^2/2}) = e^{-t^2/2}$$
4. Substituting this back into the inequality yields:
   $$P(Z \ge t) \le \frac{1}{t \sqrt{2\pi}} e^{-t^2/2}$$
This completes the proof. $\blacksquare$

---

### Part b) [15 Points]
**Comparison for $P(Z \ge 3)$:**
1. **Mill's Inequality Bound:**
   $$P(Z \ge 3) \le \frac{1}{3 \sqrt{2\pi}} e^{-9/2} = \frac{1}{3 \sqrt{2\pi}} e^{-4.5} \approx \frac{1}{3 \cdot 2.5066} \cdot 0.0111 \approx 0.00147$$
2. **Chebyshev's Inequality Bound:**
   Since $Z \sim N(0,1)$, we have $E[Z] = 0$ and $\text{Var}(Z) = 1$.
   By symmetry of the normal distribution, $P(Z \ge 3) = \frac{1}{2} P(|Z| \ge 3)$.
   Applying Chebyshev's inequality to the two-sided tail:
   $$P(|Z| \ge 3) \le \frac{\text{Var}(Z)}{3^2} = \frac{1}{9}$$
   Thus:
   $$P(Z \ge 3) \le \frac{1}{2} \cdot \frac{1}{9} = \frac{1}{18} \approx 0.05556$$
3. **Comparison:**
   Mill's Inequality yields a bound of $0.00147$, while Chebyshev's yields $0.05556$.
   Mill's bound is tighter by a factor of:
   $$\frac{0.05556}{0.00147} \approx 37.7 \text{ times}$$
   This is because Mill's inequality exploits the exponential decay of the normal distribution, whereas Chebyshev's inequality only assumes the existence of the variance.

---

## Question 2: Randomized Hitting Set (35 Points)

### Part a) [15 Points]
**Proof:**
1. For any specific sample element $x \in R$, since it is chosen uniformly at random from $V$, the probability that $x \notin S_i$ is:
   $$P(x \notin S_i) = 1 - \frac{|S_i|}{n}$$
2. Since we are given that $|S_i| \ge \epsilon n$, we have:
   $$P(x \notin S_i) \le 1 - \epsilon$$
3. Since the $s$ samples are chosen independently with replacement, the probability that all $s$ elements fail to hit $S_i$ is:
   $$P(R \cap S_i = \emptyset) \le (1 - \epsilon)^s$$
4. Using the standard inequality $1 - x \le e^{-x}$ (valid for all $x \in \mathbb{R}$):
   $$P(R \cap S_i = \emptyset) \le e^{-\epsilon s}$$
This completes the proof. $\blacksquare$

---

### Part b) [20 Points]
**Proof:**
1. Let $E_i$ be the bad event that the sample set $R$ fails to intersect $S_i$ (i.e. $R \cap S_i = \emptyset$).
2. The probability that $R$ fails to hit at least one subset in $\mathcal{S}$ is the union of these bad events:
   $$P(\text{Failure}) = P\left( \bigcup_{i=1}^m E_i \right)$$
3. Applying the Union Bound:
   $$P(\text{Failure}) \le \sum_{i=1}^m P(E_i) \le m \cdot e^{-\epsilon s}$$
4. We want to ensure that this failure probability is at most $\delta$. Thus, we set:
   $$m \cdot e^{-\epsilon s} \le \delta \implies e^{-\epsilon s} \le \frac{\delta}{m}$$
5. Taking the natural logarithm of both sides and multiplying by $-1$:
   $$\epsilon s \ge \ln\left(\frac{m}{\delta}\right) \implies s \ge \frac{1}{\epsilon} \ln\left(\frac{m}{\delta}\right)$$
This completes the proof. $\blacksquare$

---

## Question 3: Chebyshev's Higher Moments (35 Points)

### Part a) [15 Points]
1.  **Expected Value $E[Y]$:**
    $$E[Y] = \sum_{i=1}^n E[X_i] = 0 \quad (\text{since } E[X_i] = \frac{1}{2}(1) + \frac{1}{2}(-1) = 0)$$
2.  **Second Moment $E[Y^2]$:**
    $$E[Y^2] = E\left[ \sum_{i=1}^n \sum_{j=1}^n X_i X_j \right] = \sum_{i=1}^n E[X_i^2] + \sum_{i \ne j} E[X_i X_j]$$
    Since $X_i^2 = 1$ always, $E[X_i^2] = 1$. Since $X_i, X_j$ are independent for $i \ne j$, $E[X_i X_j] = 0$. Thus:
    $$E[Y^2] = n$$
3.  **Fourth Moment $E[Y^4]$:**
    $$E[Y^4] = E\left[ \left( \sum_{i=1}^n X_i \right)^4 \right] = E\left[ \sum_{i, j, k, l} X_i X_j X_k X_l \right]$$
    - A term $E[X_i X_j X_k X_l]$ is non-zero only if every variable in the term has an even power.
    - The only non-zero terms are:
      - Terms of the form $X_i^4$: There are $n$ such terms, and $E[X_i^4] = 1$.
      - Terms of the form $X_i^2 X_j^2$ with $i \ne j$: There are $\binom{4}{2} \cdot \binom{n}{2} = 3n(n-1)$ such terms, and $E[X_i^2 X_j^2] = 1$.
    - Adding these together:
      $$E[Y^4] = n + 3n(n-1) = 3n^2 - 2n \le 3n^2$$

---

### Part b) [20 Points]
**Proof of Tail Bound:**
1. Using Markov's inequality on the non-negative random variable $Y^4$:
   $$P(|Y| \ge t) = P(Y^4 \ge t^4) \le \frac{E[Y^4]}{t^4}$$
2. Substituting the bound $E[Y^4] \le 3n^2$ from part (a):
   $$P(|Y| \ge t) \le \frac{3n^2}{t^4}$$

**Comparison with 2nd Moment:**
- The standard Chebyshev bound (2nd moment) is:
  $$P(|Y| \ge t) \le \frac{E[Y^2]}{t^2} = \frac{n}{t^2}$$
- Let $t = c\sqrt{n}$ represent a deviation of $c$ standard deviations:
  - 2nd moment bound: $P(|Y| \ge c\sqrt{n}) \le \frac{1}{c^2}$
  - 4th moment bound: $P(|Y| \ge c\sqrt{n}) \le \frac{3n^2}{c^4 n^2} = \frac{3}{c^4}$
- As $c$ increases, the 4th moment bound decreases as $O(c^{-4})$, which is much faster than the $O(c^{-2})$ decay of the 2nd moment. For large deviations (e.g. $c \ge 2$), the 4th moment bound is significantly tighter. $\blacksquare$
