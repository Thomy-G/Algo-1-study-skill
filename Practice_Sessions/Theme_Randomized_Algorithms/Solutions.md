# 📝 Practice Session Solutions - Randomized Algorithms

---

## Question 1: Freivalds' Matrix Multiplication Verification

### Part a) [10 Points]
To verify if $A \cdot B = C$ in $O(n^2)$ time:
1. Choose a random vector $r \in \{0, 1\}^n$ where each component $r_i$ is chosen independently and uniformly at random from $\{0, 1\}$.
2. Compute the product $x = B \cdot r$. This is a matrix-vector multiplication and takes $O(n^2)$ time.
3. Compute the product $y = A \cdot x = A \cdot (B \cdot r)$ in $O(n^2)$ time.
4. Compute the product $z = C \cdot r$ in $O(n^2)$ time.
5. If $y = z$, output `TRUE`. Otherwise, output `FALSE`.

### Part b) [12 Points]
- **Proof:**
  - Let $D = A \cdot B - C$. Since $A \cdot B \neq C$, the matrix $D$ is not the zero matrix, so it has at least one non-zero entry.
  - Let $d_{ij} \neq 0$ be a non-zero entry in $D$. Since we are operating over $\mathbb{F}_2$, $d_{ij} = 1$.
  - The $i$-th component of the vector $D \cdot r$ is given by:
    $$(D \cdot r)_i = \sum_{k=1}^n d_{ik} r_k \pmod 2$$
  - We want to find the probability that this component is 0:
    $$\sum_{k=1}^n d_{ik} r_k = 0 \iff d_{ij} r_j + \sum_{k \neq j} d_{ik} r_k = 0 \pmod 2$$
  - Since $d_{ij} = 1$:
    $$r_j = \sum_{k \neq j} d_{ik} r_k \pmod 2$$
  - Let $S = \sum_{k \neq j} d_{ik} r_k \pmod 2$. The value of $S$ depends entirely on the random choices of $r_k$ for $k \neq j$.
  - Since $r_j$ is chosen independently of all other $r_k$, the probability that $r_j$ equals $S$ is exactly $1/2$ (since $r_j$ can be either 0 or 1 with equal probability).
  - Therefore, the probability that the $i$-th component is 0 is at most $1/2$.
  - For $D \cdot r = 0$ to hold, every component of $D \cdot r$ must be 0. Thus:
    $$\Pr[A \cdot B \cdot r = C \cdot r] = \Pr[D \cdot r = 0] \le \Pr[(D \cdot r)_i = 0] \le \frac{1}{2} \quad \blacksquare$$

### Part c) [8 Points]
- To reduce the error probability to at most $\delta$, we run the algorithm $k$ times independently with $k$ independent random vectors $r^{(1)}, r^{(2)}, \dots, r^{(k)}$.
- If the algorithm outputs `TRUE` in all $k$ iterations, we declare $A \cdot B = C$. If even one run outputs `FALSE`, we declare $A \cdot B \neq C$.
- The probability of error in $k$ independent trials is at most:
  $$\left(\frac{1}{2}\right)^k \le \delta \iff 2^k \ge \frac{1}{\delta} \iff k \ge \log_2 \left(\frac{1}{\delta}\right)$$
- We choose $k = \lceil \log_2(1/\delta) \rceil$.
- Each trial takes $O(n^2)$ time. The total runtime is:
  $$\text{Total Time Complexity} = O\left(n^2 \log \left(\frac{1}{\delta}\right)\right)$$

---

## Question 2: Expected Complexity of Randomized QuickSelect

### Part a) [10 Points]
```python
def RandomizedQuickSelect(A, k):
    # A: input array of size n, k: target index (0-indexed)
    if len(A) == 1:
        return A[0]
    
    # Choose a pivot uniformly at random
    pivot = random.choice(A)
    
    # Partition the array into three parts
    A_less = [x for x in A if x < pivot]
    A_equal = [x for x in A if x == pivot]
    A_greater = [x for x in A if x > pivot]
    
    if k < len(A_less):
        return RandomizedQuickSelect(A_less, k)
    elif k < len(A_less) + len(A_equal):
        return pivot
    else:
        new_k = k - len(A_less) - len(A_equal)
        return RandomizedQuickSelect(A_greater, new_k)
```

### Part b) [10 Points]
- Let $T(n)$ be the number of comparisons on an array of size $n$.
- When a pivot is chosen uniformly at random, its rank $i \in \{1, \dots, n\}$ is uniformly distributed with probability $1/n$.
- In the worst case (the search continues in the larger partition), the recurrence relation for the expected number of comparisons $E[T(n)]$ is:
  $$E[T(n)] \le n + \frac{1}{n} \sum_{i=1}^n E[T(\max(i-1, n-i))]$$
- The term $\max(i-1, n-i)$ takes values from $n/2$ to $n-1$, each exactly twice. Thus:
  $$E[T(n)] \le n + \frac{2}{n} \sum_{j=\lfloor n/2 \rfloor}^{n-1} E[T(j)]$$

### Part c) [15 Points]
- **Proof by Induction:** We prove $E[T(n)] \le c \cdot n$ for some constant $c$.
  - **Base Case:** For $n=1$, $E[T(1)] = 0 \le c \cdot 1$ (holds for any $c \ge 0$).
  - **Inductive Step:** Assume $E[T(j)] \le c \cdot j$ holds for all $j < n$. Then:
    $$E[T(n)] \le n + \frac{2}{n} \sum_{j=\lfloor n/2 \rfloor}^{n-1} c \cdot j = n + \frac{2c}{n} \sum_{j=\lfloor n/2 \rfloor}^{n-1} j$$
  - We bound the summation:
    $$\sum_{j=\lfloor n/2 \rfloor}^{n-1} j \le \sum_{j=1}^{n-1} j - \sum_{j=1}^{n/2 - 1} j \le \frac{n^2}{2} - \frac{n^2}{8} = \frac{3}{8} n^2$$
  - Substituting this back into the recurrence:
    $$E[T(n)] \le n + \frac{2c}{n} \left(\frac{3}{8} n^2\right) = n + \frac{3}{4} c n = n \left(1 + \frac{3}{4} c\right)$$
  - We want this to be at most $c \cdot n$:
    $$n \left(1 + \frac{3}{4} c\right) \le c n \iff 1 + \frac{3}{4} c \le c \iff 1 \le \frac{1}{4} c \iff c \ge 4$$
  - Choosing $c = 4$ completes the induction. Thus, $E[T(n)] \le 4n = O(n)$ time. $\blacksquare$

---

## Question 3: Skip List Expected Search Complexity

### Part a) [10 Points]
- When inserting a key, we flip a fair coin (probability of heads = 1/2) repeatedly until it lands heads.
- The number of tails flipped before the first heads determines the number of additional levels (the height of the tower $H$) for that element.
- The height $H$ is a geometric random variable with parameter $p = 1/2$:
  $$\Pr[H = k] = \left(\frac{1}{2}\right)^k \quad \text{for } k \ge 1$$
- The probability that a tower reaches level $k$ is $\Pr[H \ge k] = (1/2)^{k-1}$.

### Part b) [12 Points]
- **Proof:**
  - Let $H_{max}$ be the maximum height among all $n$ elements in the Skip List.
  - Using the Union Bound:
    $$\Pr[H_{max} \ge k] \le \sum_{i=1}^n \Pr[H_i \ge k] = n \cdot \left(\frac{1}{2}\right)^{k-1}$$
  - Let $k = c \log_2 n + 1$. Substituting this in:
    $$\Pr[H_{max} \ge c \log_2 n + 1] \le n \cdot \left(\frac{1}{2}\right)^{c \log_2 n} = n \cdot n^{-c} = n^{1-c}$$
  - For $c \ge 2$, the probability is at most $1/n$.
  - Therefore, the maximum height of the Skip List is at most $O(\log n)$ with high probability (w.h.p.).
  - The expected height is also bounded by:
    $$E[H_{max}] = \sum_{k=1}^\infty \Pr[H_{max} \ge k] \le O(\log n) + \sum_{k=O(\log n)}^\infty n^{1-k} = O(\log n) \quad \blacksquare$$

### Part c) [13 Points]
- **Proof via Backwards Analysis:**
  - Instead of analyzing the search path from the top-left element down to the target key, we trace the search path in **reverse** (backwards) starting from the target key at level 1 up to the top-left element.
  - At any element $v$ at level $j$ in the reverse path:
    * If $v$ was reached from above (vertical step down in the forward path), it means the tower of $v$ extends above level $j$.
    * If $v$ was reached from the left (horizontal step right in the forward path), it means the tower of $v$ did not extend above level $j$.
  - By the memoryless property of geometric coin flips, the probability that a tower of an element extends above level $j$ given that it reached level $j$ is exactly $1/2$.
  - Therefore, at each step in the reverse path:
    * We move **up** with probability $1/2$ (coin landing tails).
    * We move **left** with probability $1/2$ (coin landing heads).
  - The reverse walk terminates when we reach the top-left element of the Skip List.
  - The total number of vertical steps (moves up) is bounded by the height of the Skip List, which is $O(\log n)$ w.h.p.
  - The total number of horizontal steps (moves left) is bounded by the number of elements at the bottom level, but since we only move left at each level until we can move up, the total walk is equivalent to the number of coin flips needed to obtain $H_{max} = O(\log n)$ tails.
  - The expected number of coin flips to get $H$ tails is $2H = O(\log n)$.
  - Thus, the expected number of total steps in the search path is $2 E[H_{max}] = O(\log n)$ time. $\blacksquare$
