---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-17
---
# Module 1-Divide & Conquer, Polynomials, and the Fast Fourier Transform (FFT)

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 1: Divide & Conquer, Polynomials, and the Fast Fourier Transform (FFT)

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering the foundational polynomial and signal evaluation architectures of this course. Mastery of these concepts is essential for successfully solving the algebraic divide-and-conquer and structural transformation sections in past exams.

### 1. Foundational Vocabulary & Core Representations

In past exams, you are frequently required to manipulate dual representations of polynomials or leverage the fine-grained geometric properties of complex numbers on the unit circle. The three key conceptual systems you must master are:

#### 1.1 Dual Representations of Polynomials

A polynomial $A(x) = \sum_{i=0}^{n-1} a_i x^i = a_0 + a_1x + a_2x^2 + \dots + a_{n-1}x^{n-1}$ of degree $n-1$ (bounded degree $n$) can be maintained using two completely distinct operational primitives:

|**Representation Type**|**Structural Memory Layout**|**Evaluation Operational Cost**|**Multiplication Operational Cost**|
|---|---|---|---|
|**Coefficient Representation** (ייצוג מקדמים)|An ordered array/vector of coefficients: $(a_0, a_1, \dots, a_{n-1})$.|$O(n)$ via Horner's Method: $b_k = a_k + x_0 \cdot b_{k+1}$.|$O(n^2)$ via standard discrete convolution: $c_i = \sum_{j=0}^i a_j b_{i-j}$.|
|**Point-Value Representation** (ייצוג נקודות)|A set of $n$ distinct coordinate pairs: $\{(x_0, y_0), (x_1, y_1), \dots, (x_{n-1}, y_{n-1})\}$.|$O(n^2)$ via standard Lagrange Interpolation formula.|$O(n)$ via pointwise multiplication ($z_k = y_k \cdot y'_k$).|

#### 1.2 The Complex Roots of Unity

To transition between Coefficient and Point-Value forms without sustaining an $O(n^2)$ processing bottleneck, we evaluate polynomials explicitly at the **$n$-th complex roots of unity**, which are the algebraic solutions to the geometric equation $z^n = 1$.

$$\omega_n = e^{2\pi i / n} = \cos\left(\frac{2\pi}{n}\right) + i \sin\left(\frac{2\pi}{n}\right)$$

The $n$ distinct structural roots are generated sequentially by taking powers: $\omega_n^0, \omega_n^1, \dots, \omega_n^{n-1}$.

#### 1.3 Essential Mathematical Lemmas (The "Big Three")

To prove code soundness and maintain divide-and-conquer runtime recurrences, you must memorize three rigid properties:

- **Cancellation Lemma:** For any integers $n \ge 0$, $k \ge 0$, and $d > 0$:
    
    $$\omega_{dn}^{dk} = \omega_n^k$$
    
- **Halving Lemma:** If $n$ is an even positive integer, the $n$ squared roots of unity are exactly the $n/2$ distinct $(n/2)$-th roots of unity:
    
    $$(\omega_n^{k + n/2})^2 = (\omega_n^k)^2 = \omega_{n/2}^k$$
    
- **Summation Lemma:** For any integer $n \ge 1$ and non-zero power $k$ not divisible by $n$:
    
    $$\sum_{j=0}^{n-1} (\omega_n^k)^j = 0$$
    

> [!TIP]
> 
> **Exam Tip (From 2024 Moed A):** You may be explicitly asked to compute the absolute sum or product of all $n$-th roots of unity.
> 
> - **Sum:** Always equals 0 for $n > 1$ (by the Summation Lemma).
>     
> - **Product:** Equals $(-1)^{n-1}$. Know these identities by heart for the short-answer section!
    

#### 1.4 Polynomial Evaluation & Interpolation Primitives

##### Horner's Method (אלגוריתם הורנר)
Used to evaluate a polynomial $A(x) = a_0 + a_1x + \dots + a_{n-1}x^{n-1}$ at a specific point $x_0$ in $O(n)$ time by parenthesizing the expression:
$$A(x_0) = a_0 + x_0(a_1 + x_0(a_2 + \dots + x_0(a_{n-2} + x_0 a_{n-1})\dots))$$

```plaintext
HORNER-EVAL(a, x0):
    val = 0
    for i = n-1 down to 0:
        val = a[i] + x0 * val
    return val
```

##### Lagrange Interpolation (אינטרפולציית לגרנז')
Constructs the unique polynomial $A(x)$ of degree at most $n-1$ that passes through $n$ distinct points $\{(x_0, y_0), \dots, (x_{n-1}, y_{n-1})\}$ in $O(n^2)$ time:
$$A(x) = \sum_{i=0}^{n-1} y_i \cdot \ell_i(x)$$
where the Lagrange basis polynomials $\ell_i(x)$ are defined as:
$$\ell_i(x) = \prod_{j \neq i} \frac{x - x_j}{x_i - x_j}$$

### 2. Classification of Multiplications & The FFT Framework

The Fast Fourier Transform handles the rapid conversion between representations by splitting a polynomial cleanly down parity lines to transform an $O(n^2)$ multi-point evaluation task into a symmetric hierarchical layout.

#### 2.1 The Paradigm Hierarchy

```
                  Polynomial Evaluation (Degree n)
                                 │
           ┌─────────────────────┴─────────────────────┐
     Even-Indexed Coefficients                  Odd-Indexed Coefficients
A_even(x) = a0 + a2x + a4x^2 + ...         A_odd(x) = a1 + a3x + a5x^2 + ...
                                 │
                                 ▼
                     Recombination Butterfly Rule
               A(x) = A_even(x^2) + x * A_odd(x^2)
```

#### 2.2 Algorithmic Presentations & Code Layouts

##### Forward Fast Fourier Transform (FFT)

Evaluates coefficient array $a$ at all $n$ roots of unity $\omega_n^k$ simultaneously by enforcing structural subproblem reuse.

Plaintext

```
FFT(a):
    n = len(a)
    if n == 1: 
        return a
    omega_n = exp(2 * pi * i / n)
    omega = 1
    
    aeven = [a[0], a[2], ..., a[n-2]]
    aodd  = [a[1], a[3], ..., a[n-1]]
    
    Yeven = FFT(aeven)
    Yodd  = FFT(aodd)
    
    Y = [0] * n
    for j = 0 to (n/2) - 1:
        Y[j]       = Yeven[j] + omega * Yodd[j]
        Y[j + n/2] = Yeven[j] - omega * Yodd[j]
        omega = omega * omega_n
    return Y
```

- **Complexity Matrix:** $T(n) = 2T(n/2) + O(n) \implies \mathbf{O(n \log n)}$.
    

##### Inverse Fast Fourier Transform (IFFT)

Interpolates values back to authentic coefficients. It mimics the forward algorithm exactly but implements **negative powers** ($\omega_n^{-1}$) and scales the final vectorized output down by $1/n$:

```plaintext
IFFT(y):
    a_prime = IFFT-RECURSIVE(y)
    n = len(y)
    a = [0] * n
    for j = 0 to n-1:
        a[j] = a_prime[j] / n
    return a

IFFT-RECURSIVE(y):
    n = len(y)
    if n == 1:
        return y
    omega_n = exp(-2 * pi * i / n) // Negative exponent
    omega = 1
    yeven = [y[0], y[2], ..., y[n-2]]
    yodd  = [y[1], y[3], ..., y[n-1]]
    Aeven = IFFT-RECURSIVE(yeven)
    Aodd  = IFFT-RECURSIVE(yodd)
    a_prime = [0] * n
    for j = 0 to (n/2) - 1:
        a_prime[j]       = Aeven[j] + omega * Aodd[j]
        a_prime[j + n/2] = Aeven[j] - omega * Aodd[j]
        omega = omega * omega_n
    return a_prime
```

- **Complexity Matrix:** $\mathbf{O(n \log n)}$.

#### 2.3 Strassen's Matrix Multiplication (כפל מטריצות שטראסן)

*   **Objective:** Multiply two $n \times n$ matrices in $O(n^{\log_2 7}) \approx O(n^{2.81})$ time, improving upon the naive $O(n^3)$ algorithm.
*   **The Divide-and-Conquer Recurrence:** 
    Divide matrices $A, B$ into four $n/2 \times n/2$ submatrices:
    $$A = \begin{pmatrix} A_{11} & A_{12} \\ A_{21} & A_{22} \end{pmatrix}, \quad B = \begin{pmatrix} B_{11} & B_{12} \\ B_{21} & B_{22} \end{pmatrix}$$
*   **The 7 Multiplications:** Compute the following 7 subproblems:
    *   $P_1 = A_{11} \cdot (B_{12} - B_{22})$
    *   $P_2 = (A_{11} + A_{12}) \cdot B_{22}$
    *   $P_3 = (A_{21} + A_{22}) \cdot B_{11}$
    *   $P_4 = A_{22} \cdot (B_{21} - B_{11})$
    *   $P_5 = (A_{11} + A_{22}) \cdot (B_{11} + B_{22})$
    *   $P_6 = (A_{12} - A_{22}) \cdot (B_{21} + B_{22})$
    *   $P_7 = (A_{11} - A_{21}) \cdot (B_{11} + B_{12})$
*   **Recombining into $C = A \cdot B$:**
    *   $C_{11} = P_5 + P_4 - P_2 + P_6$
    *   $C_{12} = P_1 + P_2$
    *   $C_{21} = P_3 + P_4$
    *   $C_{22} = P_5 + P_1 - P_3 - P_7$
*   **Time Complexity Recurrence:**
    $$T(n) = 7T(n/2) + \Theta(n^2) \implies T(n) = \mathbf{\Theta(n^{\log_2 7})} \approx \mathbf{O(n^{2.81})}$$
    

### 3. Exam-Style Applications & Problem Solving Techniques

#### 3.1 Paradigm A: Advanced String Matching with Numeric Distance Properties

**Concept:** When requested to calculate alignment matching configurations or sequence differences over strings with unique distance conditions (e.g., matching within an absolute window variance or character threshold), you must represent characters as symbolic polynomial indicators and always **reverse the pattern coefficients** to enforce indexing alignment.

- **Reversal Transformation Rule:** If text $T$ has length $n$ and pattern $P$ has length $m$, define:
    
    $$A(x) = \sum_{i=0}^{n-1} t_i x^i \quad \text{and} \quad B(x) = \sum_{j=0}^{m-1} p_{m-1-j} x^j$$
    
- The structural expansion coefficient of $x^{i + m - 1}$ in the product polynomial $C(x) = A(x) \cdot B(x)$ isolates and accumulates exactly the combined matching metric at text alignment offset $i$.
    

#### 3.2 Worked Example: Computing Thresholded "3-Match" Metrics (Exercise 2 Variant)

Suppose you are given a text string $T$ of length $n$ and a pattern string $P$ of length $m$ ($m \le n$) over the numerical alphabet $\Sigma = \{1, 2, 3, \dots, 20\}$. Two characters $a, b \in \Sigma$ are legally designated as "3-compatible" if and only if $|a - b| \le 3$. Design an algorithm running in $O(n \log n)$ time that discovers all offset indices $i$ where _every_ single character in $P$ matches its aligned text counterpart within this absolute deviation bound of 3.

##### Step-by-Step Problem Resolution:

1. **Construct Specific Identity Indicator Signals:**
    
    Since the alphabet contains fixed values up to 20, create indicator functions for every possible numeric character value $\sigma \in \Sigma$. For each value $\sigma$:
    
    - Build text array $T_\sigma$ where $T_\sigma[i] = 1$ if $T[i] == \sigma$, else $0$.
        
    - Build pattern array $P_\sigma$ where $P_\sigma[j] = 1$ if $P[j] == \sigma$, else $0$.
        
2. **Incorporate the Compatibility Metric:**
    
    For each text character value $\sigma \in [1, 20]$, it can safely interact with any pattern character $v$ satisfying $\sigma - 3 \le v \le \sigma + 3$. To gather this into a streamlined calculation loop, we build a combined pattern companion polynomial for each value of $\sigma$:
    
    $$B_\sigma(x) = \sum_{v = \max(1, \sigma-3)}^{\min(20, \sigma+3)} \left( \sum_{j=0}^{m-1} P_v[m - 1 - j] x^j \right)$$
    
    _(Note: This step accurately captures the pattern coefficient reversal)._
    
3. **Compute the Convolutions:**
    
    Initialize a comprehensive score accumulator array $M$ of size $n + m$ with zero values. For each character option $\sigma \in \{1, \dots, 20\}$:
    
    - Transform $T_\sigma(x)$ into point-value form using forward **FFT**.
        
    - Transform $B_\sigma(x)$ into point-value form using forward **FFT**.
        
    - Compute the pointwise multiplication of the two evaluated arrays.
        
    - Interpolate back via **IFFT** to obtain the temporary product vector $C_\sigma$.
        
    - Add $C_\sigma$ directly into your master tracking score array: $M = M + C_\sigma$.
        
4. **Isolate and Validate Valid Shifts:**
    
    For every offset index $i$ ranging safely from $0$ to $n - m$:
    
    - Inspect the total structural score stored at index position $i + m - 1$ in array $M$.
        
    - If $M[i + m - 1] == m$, this guarantees that all $m$ pattern indices successfully satisfied the $|a - b| \le 3$ requirement at this shift. Output index $i$ to the result matrix.
        
5. **Complexity Analysis Verification:**
    
    - Generating the indicator arrays takes $O(20 \cdot n) = O(n)$ time.
        
    - Running the FFT/IFFT loop takes $20 \cdot O(n \log n) = O(n \log n)$ time.
        
    - Checking the final score array takes $O(n)$ time.
        
    - Total runtime: $\mathbf{O(n \log n)}$, satisfying the exam constraint completely.
        

#### 3.3 Paradigm B: Asymmetric / Sparse Polynomial Multiplication (2024 Moed A / Moed B)

> [!NOTE]
> 
> **Exam Target:** Standard FFT multiplies two arbitrary degree-$n$ polynomials in $O(n \log n)$. But what if one polynomial is extremely sparse, containing only $k$ non-zero terms where $k \ll n$? Using standard FFT is inefficient here.

#### 3.4 Worked Example: Multiplying a Dense and an $O(1)$-Sparse Polynomial

Given a dense polynomial $A(x) = \sum_{i=0}^{n-1} a_i x^i$ via its coefficients, and a $k$-sparse polynomial $B(x) = \sum_{z=1}^{k} b_{j_z} x^{j_z}$ where only $k$ coefficients are non-zero ($k = O(1)$), design an algorithm to compute $C(x) = A(x) \cdot B(x)$ in **$O(n)$ time**.

##### Step-by-Step Problem Resolution:

1. **Leverage Algebraic Distribution Rule:**
    
    Do not convert to point-value form. Instead, expand the multiplication linearly across the sparse terms of $B(x)$:
    
    $$C(x) = A(x) \cdot \left( b_{j_1}x^{j_1} + b_{j_2}x^{j_2} + \dots + b_{j_k}x^{j_k} \right) = \sum_{z=1}^{k} \left( b_{j_z} \cdot A(x) \cdot x^{j_z} \right)$$
    
2. **Shift and Scale Primitives:**
    
    Initialize a zero array for $C(x)$ coefficients up to index $(n-1) + \max(j_z)$. For each of the $k$ active terms $(b_{j_z}, j_z)$:
    
    - Loop through $A(x)$'s coefficient array from $i = 0$ to $n-1$.
        
    - Compute the product $b_{j_z} \cdot a_i$ and add this value directly to index position $i + j_z$ in $C$:
        
        $$C[i + j_z] \leftarrow C[i + j_z] + (b_{j_z} \cdot a_i)$$
        
3. **Complexity Analysis Verification:**
    
    - For each active term, we perform $O(n)$ scalar additions and multiplications.
        
    - There are exactly $k$ terms, resulting in an overall time complexity of $k \cdot O(n)$.
        
    - Since $k = O(1)$, the runtime collapses to **$\mathbf{O(n)}$**, completely bypassing the $O(n \log n)$ bottleneck.

### 4. Advanced Algebraic & Matching Applications (Homework & Exam Reductions)

#### 4.1 $O(n^2)$ Lagrange Interpolation via Polynomial Division
*   **Problem:** Given $n$ distinct points $\{(x_0, y_0), \dots, (x_{n-1}, y_{n-1})\}$, interpolate the unique polynomial $A(x) = \sum_{i=0}^{n-1} a_i x^i$ of degree at most $n-1$ in $O(n^2)$ time.
*   **The Bottleneck:** Naive evaluation of the Lagrange basis polynomials $\ell_i(x) = \prod_{j \neq i} \frac{x - x_j}{x_i - x_j}$ takes $O(n)$ time per basis, leading to a total time of $O(n^2)$ to compute all basis polynomials. However, summing them directly would take $O(n^2)$ multiplications of polynomials of degree $n$, which naively takes $O(n^3)$ time.
*   **The Division Trick:**
    1.  Precompute the master polynomial $M(x) = \prod_{j=0}^{n-1} (x - x_j)$ by multiplying terms one by one. The $j$-th multiplication multiplies a degree-$j$ polynomial by a degree-1 polynomial, taking $O(j)$ time. Total time for $M(x)$ is $\sum_{j=1}^{n-1} O(j) = O(n^2)$.
    2.  For each $k \in \{0, \dots, n-1\}$, the numerator of $\ell_k(x)$ is $M_k(x) = \frac{M(x)}{x - x_k}$. Since $M(x)$ is given by its coefficients, we can compute the quotient $M_k(x)$ in $O(n)$ time using Horner's method / synthetic division.
    3.  Evaluate $z_k = M_k(x_k)$ in $O(n)$ time (using Horner's evaluation).
    4.  Scale the coefficients of $M_k(x)$ by the scalar $\frac{y_k}{z_k}$ in $O(n)$ time, and add it to the accumulator polynomial $A(x)$.
*   **Complexity:** Preprocessing takes $O(n^2)$ time. The loop runs $n$ times and performs $O(n)$ operations per iteration, giving a total time complexity of **$O(n^2)$**.

#### 4.2 $O(n \log^2 n)$ Polynomial Construction from $n$ Roots
*   **Problem:** Given $n$ roots $(r_1, r_2, \dots, r_n)$ of a polynomial $A(x)$ of degree $n$, find the coefficients of $A(x) = \prod_{i=1}^n (x - r_i)$ in $O(n \log^2 n)$ time.
*   **The Divide-and-Conquer Strategy:**
    1.  *Base Case:* If $n = 1$, return the polynomial $x - r_1$ (represented as coefficients $[-r_1, 1]$).
    2.  *Divide:* Split the roots into two halves: $L = (r_1, \dots, r_{n/2})$ and $R = (r_{n/2+1}, \dots, r_n)$.
    3.  *Conquer:* Recursively construct the polynomials $P_L(x)$ and $P_R(x)$ for the two halves.
    4.  *Combine:* Multiply $P_L(x)$ and $P_R(x)$ using **FFT** in $O(n \log n)$ time.
*   **Recurrence Relation:**
    $$T(n) = 2T(n/2) + O(n \log n) \implies T(n) = \mathbf{O(n \log^2 n)}$$
    which can be verified by the recursion tree: at depth $d$, there are $2^d$ subproblems of size $n/2^d$, each taking $O((n/2^d) \log(n/2^d))$ time to multiply, summing to $O(n \log n)$ per level. With $\log n$ levels, the total is $O(n \log^2 n)$.

#### 4.3 Coin Sum Combinations using FFT Exponentiation & Clipping
*   **Problem:** Given distinct coin values $\{v_1, \dots, v_k\}$ and a quantity limit $t$, find the possible sums and combination counts. Represent the coin options as a representative polynomial $P(x) = \sum_{i=1}^k x^{v_i}$.
*   **Reductions:**
    1.  *Exactly $t$ Coins (Possible Sums):* Compute the polynomial $(P(x))^t$. The coefficient of $x^S$ is $>0$ iff sum $S$ is achievable with exactly $t$ coins. Use **Fast Exponentiation** (repeated squaring):
        *   $T(t) = T(t/2) + O(d \log d)$ where $d \le t \cdot v_k$ is the maximum degree.
        *   Since the degree doubles at each level, the last multiplication of degree $t v_k / 2$ dominates: $O(t v_k \log (t v_k))$ time.
    2.  *At Most $t$ Coins (Possible Sums):* Allow selecting a "dummy" coin of value 0. Define $P'(x) = 1 + P(x) = x^0 + \sum_{i=1}^k x^{v_i}$, and compute $(P'(x))^t$ using the same fast exponentiation in $O(t v_k \log (t v_k))$ time.
    3.  *Number of Ways to Sum to $z$ using $t$ Coins:* We need the coefficient of $x^z$ in $(P(x))^t$. To optimize, during each polynomial multiplication step in the fast exponentiation, **clip** the resulting polynomials by discarding all terms with degree $> z$.
        *   Since the maximum degree of any intermediate polynomial is now bounded by $z$, each FFT multiplication takes $O(z \log z)$ time.
        *   Total time for $\log t$ multiplications is **$O(z \log z \log t)$** (or $O(z \log z)$ if we prune dynamically, since $t \le z$).

#### 4.4 Upper-Triangular Toeplitz Matrix Multiplication in $O(n \log n)$
*   **Problem:** Multiply two $n \times n$ upper-triangular Toeplitz matrices $A$ and $B$, where $A_{ij} = a_{j-i}$ (for $i \le j$) and $B_{ij} = b_{j-i}$ (for $i \le j$), in $O(n \log n)$ time.
*   **The Reduction:**
    1.  Observe the product matrix $C = A \cdot B$. For $i \le j$ and $d = j - i$:
        $$C_{i, i+d} = \sum_{k=i}^{i+d} A_{i,k} \cdot B_{k, i+d} = \sum_{k=i}^{i+d} a_{k-i} \cdot b_{i+d-k} = \sum_{r=0}^d a_r \cdot b_{d-r}$$
    2.  The value on the $d$-th diagonal is independent of the row index $i$. Thus, $C$ is also an upper-triangular Toeplitz matrix, with entries $c_d = \sum_{r=0}^d a_r b_{d-r}$.
    3.  This is exactly the coefficient of $x^d$ in the polynomial product $C(x) = A(x) \cdot B(x)$, where $A(x) = \sum_{i=0}^{n-1} a_i x^i$ and $B(x) = \sum_{i=0}^{n-1} b_i x^i$.
    4.  Compute $C(x)$ using a single **FFT** in **$O(n \log n)$** time. The first $n$ coefficients of $C(x)$ fully represent the product matrix $C$, requiring $O(n)$ space instead of $O(n^2)$.

#### 4.5 Multi-Alphabet Hamming Distance ($|\Sigma|=3$)
*   **Problem:** Compute the Hamming distance between text $T$ of length $n$ and pattern $P$ of length $m$ over the alphabet $\Sigma = \{0, 1, 2\}$ in $O(n \log m)$ time.
*   **The Reduction:**
    1.  For each character $\sigma \in \{0, 1, 2\}$, define binary indicator arrays:
        *   $T_\sigma[i] = 1$ if $T[i] == \sigma$, else $0$.
        *   $P_\sigma[j] = 1$ if $P[j] == \sigma$, else $0$.
    2.  The number of matches at offset $i$ is the sum of matches for each character:
        $$\text{Matches}[i] = \sum_{\sigma \in \{0, 1, 2\}} (T_\sigma \otimes P_\sigma)[i]$$
        where $\otimes$ denotes cross-correlation, computed by reversing the pattern $P_\sigma^R$ and multiplying as polynomials using FFT.
    3.  The Hamming distance at offset $i$ is $m - \text{Matches}[i]$.
*   **Complexity:** Requires 3 FFT multiplications on polynomials of degree $n$, taking **$O(n \log m)$** time.

#### 4.6 Special Triplets (Arithmetic Progressions of 1s) in Binary Strings
*   **Problem:** Count all triplets of indices $(i, j, k)$ in a binary string $S$ of length $n$ such that $1 \le i < j < k \le n$, $S[i] = S[j] = S[k] = 1$, and $k - j = j - i$ (meaning $2j = i + k$), in $O(n \log n)$ time.
*   **The Reduction:**
    1.  Represent $S$ as a polynomial $A(x) = \sum_{i=1}^n S[i] x^i$.
    2.  Compute $B(x) = A(x) \cdot A(x) = \sum_{d=2}^{2n} b_d x^d$ using FFT in $O(n \log n)$ time.
    3.  Note that $b_{2j} = \sum_{i+k=2j} S[i] \cdot S[k]$.
    4.  In this sum:
        *   The term where $i = k = j$ contributes $S[j] \cdot S[j] = 1$ (if $S[j]=1$).
        *   Every distinct pair $(i, k)$ where $i < k$ and $i+k=2j$ is counted twice (as $S[i]S[k]$ and $S[k]S[i]$), contributing 2.
    5.  Therefore, for each middle index $j$ where $S[j] = 1$, the number of valid pairs is:
        $$\text{Pairs}(j) = \frac{b_{2j} - 1}{2}$$
    6.  Sum these values over all $j$ where $S[j] = 1$ to get the total special triplets count.
*   **Complexity:** Dominated by the FFT square computation, taking **$O(n \log n)$** time.

#### 4.7 Complete Pattern Matching with Wildcards (Jokers) using FFT
*   **Problem:** Match pattern $P$ of length $m$ in text $T$ of length $n$ where the wildcard character $\diamond$ matches any character.
*   **Case 1: Jokers in Pattern Only ($k_P$ jokers)**
    *   Simply run standard matching (character indicator FFTs) on regular characters, and add $k_P$ to the matches count of every offset.
*   **Case 2: Jokers in Text Only**
    *   Precompute a prefix sum array of jokers in $T$: $W[i] = \sum_{j=1}^i [T[j] == \diamond]$.
    *   For each offset $i$, the number of text jokers aligned with the pattern is $W[i+m] - W[i]$.
    *   Add this value to the matches count of offset $i$.
*   **Case 3: Jokers in Both (Pattern and Text)**
    *   Adding pattern jokers $k_P$ and text jokers $W[i+m]-W[i]$ to the normal character matches would **double-count** positions where a pattern joker aligns with a text joker.
    *   *Correction Step:* Subtract the number of joker-joker alignments.
    *   Define binary indicator arrays:
        *   $T'[i] = 1$ if $T[i] == \diamond$, else $0$.
        *   $P'[j] = 1$ if $P[j] == \diamond$, else $0$.
    *   Multiply $T'$ and the reversed $P'^R$ using a single **FFT** in $O(n \log m)$ time to find the double-counted joker-joker alignment counts $C_{jokers}[i]$ for each offset $i$.
    *   The corrected total matches count at offset $i$ is:
        $$C_{total}[i] = C_{chars}[i] + k_P + (W[i+m] - W[i]) - C_{jokers}[i]$$
    *   If $C_{total}[i] == m$, then offset $i$ is a valid match.
*   **Complexity:** Running character matching takes $O(n \sqrt{m \log m})$ time, and the joker-joker alignment takes $O(n \log m)$ time, leading to a total time of **$O(n \sqrt{m \log m})$**.

#### 4.8 3SUM on Bounded Integers via FFT
*   **Problem:** Given a set $S \subseteq [-U, U]$ containing $n$ integers, determine if there exist three elements $a, b, c \in S$ (not necessarily distinct) such that $a + b + c = 0$ (or $a + b = c$), in $O(U \log U)$ time.
*   **The Reduction:**
    1.  Represent $S$ as a characteristic polynomial $P(x) = \sum_{s \in S} x^{s+U}$ by shifting elements to ensure all exponents are non-negative. The degree of $P(x)$ is at most $2U$.
    2.  Compute the square $Q(x) = P(x) \cdot P(x) = \sum_{d=0}^{4U} q_d x^d$ using FFT in $O(U \log U)$ time.
    3.  The coefficient $q_d$ counts the number of pairs $(a, b) \in S \times S$ such that:
        $$(a + U) + (b + U) = d \implies a + b = d - 2U$$
    4.  To check if $a + b = c$ for some $c \in S$:
        *   An element $c \in S$ shifted by $2U$ has exponent $c + 2U$.
        *   For each $c \in S$, look up the coefficient of $x^{c+2U}$ in $Q(x)$. If $q_{c+2U} > 0$, then there exist $a, b \in S$ such that $a+b = c$.
        *   *(Note: To enforce that $a, b, c$ must be distinct elements, we can subtract the case where $a=b$ or $a=c$ or $b=c$ in $O(1)$ time per lookup).*
*   **Complexity:** Dominated by the single FFT polynomial multiplication, which takes **$O(U \log U)$** time.

#### 4.9 General Alphabet Hamming Distance via Heavy/Light Splitting
*   **Problem:** Compute the Hamming distance between text $T$ of length $n$ and pattern $P$ of length $m$ over a general alphabet $\Sigma$ (where $|\Sigma|$ is large) in $O(n \sqrt{m \log m})$ time.
*   **The Problem with Naive FFT:** If we run the indicator-based FFT matching for each character $\sigma \in \Sigma$, the complexity is $O(|\Sigma| \cdot n \log m)$ time. When $|\Sigma| \ge m$, this degrades to $O(m n \log m)$, which is worse than the naive $O(nm)$ checking.
*   **The Heavy/Light Splitting Strategy:**
    1.  Choose a threshold $k = \sqrt{m \log m}$.
    2.  Classify characters in $\Sigma$ based on their frequency in the pattern $P$:
        *   **Heavy Characters ($\Sigma_{heavy}$):** Characters that appear $\ge k$ times in $P$. Since the length of $P$ is $m$, there are at most:
            $$|\Sigma_{heavy}| \le \frac{m}{k} = \sqrt{\frac{m}{\log m}}$$
            heavy characters.
        *   **Light Characters ($\Sigma_{light}$):** Characters that appear $< k$ times in $P$.
    3.  **Process Heavy Characters via FFT:**
        For each heavy character $\sigma \in \Sigma_{heavy}$:
        *   Construct indicator arrays $T_\sigma[i]$ and $P_\sigma[j]$ and compute their cross-correlation via FFT.
        *   This requires $|\Sigma_{heavy}|$ FFT multiplications, taking:
            $$O\left( |\Sigma_{heavy}| \cdot n \log m \right) = O\left( \frac{m}{k} \cdot n \log m \right) = O\left( n \sqrt{m \log m} \right) \text{ time.}$$
    4.  **Process Light Characters via Linear Scan:**
        *   Initialize a match counter array `Matches[0...n-m]` to zero.
        *   Precompute a list of positions for each light character in $P$: for each character $\sigma$, store a list $L_\sigma = \{ j \mid P[j] == \sigma \}$. Since $\sigma$ is light, $|L_\sigma| < k$.
        *   Scan the text $T[i]$ from $i = 1$ to $n$:
            *   Let $\sigma = T[i]$ be the current character. If $\sigma$ is light, iterate over all its occurrences in the pattern $j \in L_\sigma$:
                *   This match corresponds to an alignment starting at offset $i - j$. Increment `Matches[i-j]`.
            *   Since each light character occurs $< k$ times in $P$, the number of pattern indices $j \in L_\sigma$ is at most $k$. Thus, each text character $T[i]$ triggers at most $k$ increments, taking:
                $$O(n \cdot k) = O\left( n \sqrt{m \log m} \right) \text{ time.}$$
    5.  Combine the match counts from both steps. The Hamming distance at offset $i$ is $m - \text{Matches}[i]$.
*   **Complexity:** Both the heavy and light phases balance perfectly, yielding a total runtime of **$O\left(n \sqrt{m \log m}\right)$**.

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]