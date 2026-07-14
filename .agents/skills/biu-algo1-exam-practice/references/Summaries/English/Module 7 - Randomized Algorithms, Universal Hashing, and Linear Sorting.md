---
type: uni_general
course: "[[Algorithms 1]]"
status: 🟢 Active
order: Summary
date_added: 2026-06-18
---
# Module 7 - Randomized Algorithms, Universal Hashing, and Linear Sorting

**MOC:** [[Algorithms 1 MOC]]
**Course:** [[Algorithms 1]]

# Algorithms 1: Ultimate Study Guide

## Module 7: Randomized Algorithms, Universal Hashing, and Linear Sorting

This module compiles the core theoretical concepts, rigorous mathematical formulas, and detailed step-by-step exam-style practice questions covering Randomized Algorithms, Universal Hashing, and Linear Time Sorting. It details the structural differences between algorithmic classes, specific tail bounds, and the exact problem-solving paradigms frequently tested in Bar-Ilan University exams. Mastery of these concepts is essential for successfully analyzing average-case performance and designing non-deterministic computational primitives.

### 1. Foundational Vocabulary & Probabilistic Design Classes

In past exams, you are frequently required to classify a randomized runtime profile or provide strict expected bounds on indicators. Randomized algorithms utilize an internal source of random bits to guide state transitions.

#### 1.1 The Randomized Complexity Hierarchy

|**Algorithmic Class**|**Correctness Guarantee**|**Operational Runtime Profile**|**Core Analysis Focus**|
|---|---|---|---|
|**Las Vegas Algorithm** (לאס וגאס)|**Strictly Invariant:** Always returns the mathematically correct answer (Probability = 1).|**Random Variable:** Depends entirely on internal random choices.|Analyzing the **Expected Runtime** ($E[T(n)]$) or worst-case with high probability.|
|**Monte Carlo Algorithm** (מונטה קרלו)|**Random Variable:** Has a fixed probability of error (Success Probability $< 1$).|**Strictly Deterministic:** Bound is independent of internal random flips.|Boosting success metrics via independent repetition loops.|
|**Atlantic City Algorithm**|**Random Variable:** Error probability is bounded ($< 0.5$).|**Random Variable:** Bounded expected runtime.|Rarely encountered; satisfies dual non-determinism.|

#### 1.2 Linearity of Expectation & Indicators

The absolute foundational engine for resolving complex expected runtimes is the **Linearity of Expectation**, which holds true _even if the random variables are highly dependent_:

$$E\left[ \sum_{i=1}^m X_i \right] = \sum_{i=1}^m E[X_i]$$

To evaluate complex combinatorial conditions (such as the number of inversions or collision clusters), always define Boolean **Indicator Random Variables**:

$$X_{ij} = I\{\text{Event } A\} = \begin{cases} 1 & \text{if Event } A \text{ occurs} \\ 0 & \text{otherwise} \end{cases} \implies E[X_{ij}] = \Pr(\text{Event } A)$$

#### 1.3 Paranoid QuickSort (לאס וגאס פרנואידי)
*   **Operational Strategy:** Standard Randomized QuickSort chooses a random pivot and immediately splits the array. If the choice is poor (e.g., selecting the minimum or maximum), the recursion depth can degrade to $O(n)$. **Paranoid QuickSort** enforces that we only accept a pivot if it splits the current array of size $m$ into subproblems of size at least $m/4$ on each side (a "good pivot").
*   **The Algorithm:**
    1.  Pick a pivot uniformly at random.
    2.  Partition the array around the pivot and check the split sizes $|L|$ and $|G|$.
    3.  If $|L| \ge m/4$ and $|G| \ge m/4$, accept the split and recursively sort both parts.
    4.  Otherwise, discard the partition, select a new random pivot, and repeat the trial.
*   **Complexity Invariant:** Since we discard any pivot that does not produce a $25\% - 75\%$ balance, the recursion tree depth is strictly bounded by $\log_{4/3} n = O(\log n)$. The expected number of pivot selection trials per level is a constant $O(1)$, which guarantees an expected runtime of **$O(n \log n)$** with high probability.

### 2. Universal Hashing Primitives

Simple Uniform Hashing (SUHA) is an idealized abstraction. To guarantee constant expected time $O(1)$ per operation without relying on uniform distribution assumptions, we use a **Universal Family of Hash Functions**.

#### 2.1 The Universality Criteria

A finite collection of hash functions $\mathcal{H}$ mapping universe $\mathcal{U}$ into buckets $\{0, 1, \dots, m-1\}$ is **Universal** (אוניברסלית) if and only if for every distinct pair of keys $x, y \in \mathcal{U}$ where $x \neq y$, the total number of functions causing an explicit collision satisfies:

$$|\{h \in \mathcal{H} \mid h(x) == h(y)\}| \le \frac{|\mathcal{H}|}{m} \implies \Pr_{h \sim \mathcal{H}}(h(x) == h(y)) \le \frac{1}{m}$$

#### 2.2 Expected Chain Bound Theorem

If we store $n$ keys in a hash table of size $m$ via chaining using a random function $h$ drawn uniformly from a universal family $\mathcal{H}$, the expected length of the chain for any key $x$ (whether currently in the table or not) is bounded by:

$$E[\text{Chain Length } T[h(x)]] \le 1 + \frac{n-1}{m} \le 1 + \alpha \quad \left(\text{where } \alpha = \frac{n}{m}\right)$$

Thus, if we choose $m = \Theta(n)$, every dictionary operation takes **$O(1)$ expected time**.

##### Mathematical Proof:
1.  Let $x \in \mathcal{U}$ be the target query key. For each key $y$ currently stored in the hash table, define a binary indicator variable $X_{xy}$ that tracks whether $x$ and $y$ map to the same bucket (collide) under $h$:
    $$X_{xy} = I\{h(x) == h(y)\} = \begin{cases} 1 & \text{if } h(x) == h(y) \\ 0 & \text{otherwise} \end{cases} \implies E[X_{xy}] = \Pr(h(x) == h(y))$$
2.  The total length of the chain at index $T[h(x)]$, denoted by $Y_x$, is the sum of these indicators over all keys $y$ in the table:
    $$Y_x = \sum_{y \in \text{table}} X_{xy}$$
3.  Taking the expectation and applying Linearity of Expectation:
    $$E[Y_x] = E\left[ \sum_{y \in \text{table}} X_{xy} \right] = \sum_{y \in \text{table}} E[X_{xy}] = \sum_{y \in \text{table}} \Pr(h(x) == h(y))$$
4.  We evaluate the sum by splitting it into cases:
    *   **Case 1: $x$ is not stored in the table.**
        For all keys $y$ in the table, $y \neq x$. By the definition of universality:
        $$\Pr(h(x) == h(y)) \le \frac{1}{m}$$
        $$\implies E[Y_x] \le \sum_{y \in \text{table}} \frac{1}{m} = \frac{n}{m} = \alpha$$
    *   **Case 2: $x$ is stored in the table.**
        One of the keys $y$ is $x$ itself. For this item, $\Pr(h(x) == h(x)) = 1$. For the remaining $n - 1$ distinct keys $y \neq x$, the universality bound applies:
        $$\implies E[Y_x] = \Pr(h(x) == h(x)) + \sum_{y \in \text{table} \setminus \{x\}} \Pr(h(x) == h(y)) \le 1 + \sum_{y \in \text{table} \setminus \{x\}} \frac{1}{m} = 1 + \frac{n-1}{m} \le 1 + \alpha$$
5.  In either case, the expected chain length is bounded by $1 + \alpha$. If $m = \Theta(n)$, then $\alpha = O(1)$, rendering the expected runtime of searches, insertions, and deletions **$O(1)$**. $\blacksquare$


### 3. Linear-Time Sorting Foundations

While comparison-based sorting algorithms (such as Merge Sort or Heapsort) are bounded by the $\Omega(n \log n)$ information-theoretic lower bound, non-comparison algorithms exploit input distribution properties to sort in linear time.

#### 3.1 Counting Sort & Radix Sort

- **Counting Sort (מיון מנייה):** Requires keys to be integers in a known, bounded range $[0, k]$. It builds an array of frequencies to determine the exact output rank of each element.
    
    - _Complexity Matrix:_ Time: $\Theta(n + k)$, Space: $\Theta(n + k)$. It is strictly **Stable**.

```plaintext
COUNTING-SORT(A, B, k):
    let C[0...k] be a new array
    for i = 0 to k:
        C[i] = 0
    for j = 1 to len(A):
        C[A[j]] = C[A[j]] + 1
    for i = 1 to k:
        C[i] = C[i] + C[i-1]
    for j = len(A) down to 1:
        B[C[A[j]]] = A[j]
        C[A[j]] = C[A[j]] - 1
```

- **Radix Sort (מיון בסיס):** Breaks keys down into $d$ discrete digits, sorting from the **Least Significant Digit (LSD)** to the Most Significant Digit using a stable sorting sub-primitive (like Counting Sort).
    
    - _Complexity Matrix:_ Sorting $n$ keys where each key is an integer bounded by $n^c$ takes $\mathbf{\Theta(n)}$ time by representing them in base $n$ ($d = c$ digits, each digit in range $[0, n-1]$).

```plaintext
RADIX-SORT(A, d):
    for i = 1 to d:
        use a stable sort (like COUNTING-SORT) to sort array A on digit i
```

#### 3.2 Bucket Sort (מיון דליים)

- **Core Assumption:** Assumes the input elements are distributed uniformly and independently over a continuous interval, typically $[0, 1)$.
    
- **Operational Strategy:** Divides the range $[0, 1)$ into $n$ equal-sized buckets, distributes the elements into these buckets, sorts each bucket individually using Insertion Sort, and concatenates the results.

```plaintext
BUCKET-SORT(A):
    n = len(A)
    let B[0...n-1] be a new array of empty lists
    for i = 1 to n:
        insert A[i] into bucket B[floor(n * A[i])]
    for i = 0 to n - 1:
        sort bucket B[i] with insertion sort
    concatenate the lists B[0], B[1], ..., B[n-1] in order
```

##### Mathematical Proof of Expected Linear Time

Let $n_i$ be the random variable tracking the number of elements falling into bucket $i$. The total time to run Insertion Sort across all buckets is $\sum_{i=0}^{n-1} O(n_i^2)$. Taking the expectation:

$$E\left[ \sum_{i=0}^{n-1} O(n_i^2) \right] = \sum_{i=0}^{n-1} O(E[n_i^2])$$

Since each element falls into bucket $i$ with probability $p = 1/n$, $n_i$ follows a Binomial Distribution $\text{Bin}(n, 1/n)$. Using the variance identity $E[X^2] = \text{Var}(X) + E[X]^2$:

$$E[n_i^2] = n \cdot \frac{1}{n}\left(1 - \frac{1}{n}\right) + \left(n \cdot \frac{1}{n}\right)^2 = 1 - \frac{1}{n} + 1 = 2 - \frac{1}{n} = \Theta(1)$$

Substituting this back yields $\sum_{i=0}^{n-1} O(1) = \mathbf{O(n)}$.

#### 3.3 Randomized Sort & Selection (מיון ובחירה אקראיים)

##### Randomized QuickSort (מיון מהיר אקראי)
Avoids the $O(n^2)$ worst-case performance of standard QuickSort on pre-sorted arrays by selecting a pivot uniformly at random, guaranteeing an expected runtime of $O(n \log n)$.

```plaintext
RANDOMIZED-QUICKSORT(A, p, r):
    if p < r:
        q = RANDOMIZED-PARTITION(A, p, r)
        RANDOMIZED-QUICKSORT(A, p, q - 1)
        RANDOMIZED-QUICKSORT(A, q + 1, r)

RANDOMIZED-PARTITION(A, p, r):
    i = RANDOM(p, r)
    swap A[r] with A[i]
    return PARTITION(A, p, r)

PARTITION(A, p, r):
    x = A[r]
    i = p - 1
    for j = p to r - 1:
        if A[j] <= x:
            i = i + 1
            swap A[i] with A[j]
    swap A[i+1] with A[r]
    return i + 1
```

##### Mathematical Proof of Expected $O(n \log n)$ Comparisons:
1.  Let $z_1, z_2, \dots, z_n$ be the sorted order of the elements in the input array.
2.  Let $X_{ij}$ be a binary indicator random variable that is 1 if elements $z_i$ and $z_j$ are compared at any point during execution, and 0 otherwise. The total number of comparisons is $X = \sum_{i=1}^{n-1} \sum_{j=i+1}^n X_{ij}$.
3.  Two elements $z_i, z_j$ are compared if and only if one of them is chosen as a pivot before any other element in the range $\{z_i, \dots, z_j\}$ is chosen. The size of this range is $j - i + 1$. Because the pivot is selected uniformly at random at each recursive step, every element in this range has an equal probability of being chosen first. Thus:
    $$E[X_{ij}] = \Pr(z_i \text{ and } z_j \text{ are compared}) = \frac{2}{j - i + 1}$$
4.  By Linearity of Expectation:
    $$E[X] = \sum_{i=1}^{n-1} \sum_{j=i+1}^n \frac{2}{j-i+1}$$
    Let $k = j - i$. The sum becomes:
    $$E[X] = \sum_{i=1}^{n-1} \sum_{k=1}^{n-i} \frac{2}{k+1} < 2 \sum_{i=1}^n \sum_{k=1}^n \frac{1}{k} = 2 n H_n$$
    where $H_n = \sum_{k=1}^n 1/k \approx \ln n + O(1)$ is the $n$-th Harmonic number. Thus, $E[X] = \mathbf{O(n \log n)}$ comparisons. $\blacksquare$

##### Mathematical Proof of Paranoid QuickSort Expected Runtime:
1.  At each step of size $m$, we choose a pivot uniformly at random and partition. The partition is accepted if and only if both subproblems are of size at least $m/4$ (a "good pivot").
2.  An element is a good pivot if it lies between the 25th and 75th percentiles of the sorted array. Exactly half ($50\%$) of the elements satisfy this condition, so the probability of choosing a good pivot in a single trial is $p = 1/2$.
3.  The number of trials to find a good pivot is modeled as a geometric random variable $K \sim \text{Geom}(1/2)$. The expected number of attempts is:
    $$E[K] = \frac{1}{p} = 2$$
4.  Each partition check takes $O(m)$ time. The expected time to find a good pivot is $2 \cdot O(m) = O(m)$.
5.  Since the split splits the array such that the larger partition is at most $3m/4$, the recursion tree depth is strictly bounded by $\log_{4/3} n = O(\log n)$. Since the expected work per level is $O(n)$, the total expected runtime is **$\mathbf{O(n \log n)}$**. $\blacksquare$


##### Randomized QuickSelect (בחירה מהירה אקראית)
Finds the $i$-th smallest element in an array of size $n$ in $O(n)$ expected time (solving selection and median-finding problems).

```plaintext
RANDOMIZED-SELECT(A, p, r, i):
    if p == r:
        return A[p]
    q = RANDOMIZED-PARTITION(A, p, r)
    k = q - p + 1
    if i == k:
        return A[q]
    elif i < k:
        return RANDOMIZED-SELECT(A, p, q - 1, i)
    else:
        return RANDOMIZED-SELECT(A, q + 1, r, i - k)
```

### 4. Exam-Style Applications & Problem Solving Techniques

#### 4.1 Paradigm A: Asymmetric Bucket Allocations (2026 Moed B)

> [!IMPORTANT]
> 
> **Exam Target:** Standard Bucket Sort allocates exactly $n$ buckets for $n$ elements to achieve $O(n)$ average-case performance. If a question forces you to use an asymmetric number of buckets $m = o(n)$ or $m = \omega(n)$, you must recalculate the binomial variance dependencies to find the precise expected runtime bounds as a function of both parameters.

#### 4.2 Worked Example: Expected Analysis of a Compressed Bucket Space

Suppose you are given an array of $n$ floating-point numbers distributed uniformly and independently in the open interval $(0, 1)$. Instead of allocating the standard $n$ buckets, you are forced to distribute the elements across a compressed space of $m = \lceil \frac{n}{\log n} \rceil$ equal-width buckets. Each bucket is sorted internally using an $O(k^2)$ comparison mechanism. Provide a rigorous expected runtime analysis showing how this restriction changes the expected runtime relative to standard Bucket Sort.

##### Step-by-Step Problem Resolution:

1. **Characterize the Random Variable Distributions:**
    
    Let $n_i$ be the random variable representing the number of elements that fall into bucket $i$, where $0 \le i \le m-1$. Because the elements are distributed uniformly over $(0, 1)$ and each bucket has a uniform width of $1/m$, the probability of an element landing in any specific bucket is exactly $p = \frac{1}{m}$.
    
    Therefore, each variable $n_i$ follows a Binomial Distribution:
    
    $$n_i \sim \text{Bin}\left(n, \ \frac{1}{m}\right)$$
    
2. **Evaluate the Second Moment Invariant ($E[n_i^2]$):**
    
    To find the expected cost of sorting the buckets, we compute the explicit variance properties of our Binomial distribution:
    
    $$E[n_i^2] = \text{Var}(n_i) + \big(E[n_i]\big)^2 = n \cdot p \cdot (1 - p) + (n \cdot p)^2$$
    
    Substitute $p = \frac{1}{m}$ into the equation:
    
    $$E[n_i^2] = \frac{n}{m}\left(1 - \frac{1}{m}\right) + \left(\frac{n}{m}\right)^2 = \frac{n}{m} - \frac{n}{m^2} + \frac{n^2}{m^2}$$
    
3. **Incorporate the Compressed Bucket Constraints:**
    
    We are given that $m \approx \frac{n}{\log n} \implies \frac{n}{m} = \log n$. Substitute this ratio directly into our second moment equation:
    
    $$E[n_i^2] = \log n - \frac{\log n}{m} + (\log n)^2 = \Theta(\log^2 n)$$
    
4. **Aggregate Total Expected Insertion Sort Runtimes:**
    
    The total processing time of the algorithm is dominated by sorting each bucket individually. Using Linearity of Expectation:
    
    $$E[T_{\text{sort}}] = E\left[ \sum_{i=0}^{m-1} O(n_i^2) \right] = \sum_{i=0}^{m-1} O(E[n_i^2])$$
    
    Since all $m$ buckets share identical statistical dependencies, collapse the summation:
    
    $$E[T_{\text{sort}}] = m \cdot O(\Theta(\log^2 n)) = \left( \frac{n}{\log n} \right) \cdot O(\log^2 n) = \mathbf{O(n \log n)}$$
    
5. **Formulate the Comparative Conclusion:**
    
    Compressing the bucket space to $m = o(n)$ increases the expected number of elements per bucket, creating larger collision clusters. This causes the expected runtime to degrade from the standard $\mathbf{O(n)}$ linear profile down to **$\mathbf{O(n \log n)}$**.
    

#### 4.3 Paradigm B: Expected Pivot Analysis in Non-Uniform Selection

> [!NOTE]
> 
> **Exam Target:** Randomized Quick-Select finds the $k$-th smallest element in $O(n)$ expected time assuming a perfectly uniform pivot pick. If a question introduces a biased selection mechanism (e.g., picking from a restricted subset or running multi-stage sampling), solve it by setting up an indicator-based conditional expectation recurrence over the successful splitting options.

#### 4.4 Worked Example: Selection Under "Max-of-Two" Pivot Biasing

Given an array $A$ of $n$ distinct numbers. To find the median element, you implement a modified Quick-Select algorithm. In each step, instead of choosing a uniform random pivot, the algorithm selects two elements uniformly at random from the remaining array, compares them, and chooses the **larger of the two** to serve as the partition pivot. Formulate the exact probability of achieving a "Good Split" (defined as a split where both partitioned sides contain at least $n/4$ elements) under this biased selection strategy.

##### Step-by-Step Problem Resolution:

1. **Map the Geometric Bounds of a Good Split:**
    
    For a pivot to produce a good split, its value must rank between the 25th percentile and the 75th percentile of the sorted array. Let the ranks of the elements be indexed from $1$ to $n$. A good split occurs if the chosen pivot's rank $R$ falls within the following range:
    
    $$\frac{n}{4} \le R \le \frac{3n}{4}$$
    
2. **Analyze the Biased Pivot Selection Rule:**
    
    Let $X_1$ and $X_2$ be the ranks of the two elements chosen uniformly at random from the array (with replacement for simplified asymptotic analysis, since $n \to \infty$). The actual partition pivot chosen is $R = \max(X_1, X_2)$.
    
3. **Formulate the Cumulative Distribution Property:**
    
    The cumulative probability that the maximum of two independent choices is less than or equal to a target rank $k$ is given by:
    
    $$\Pr(R \le k) = \Pr(\max(X_1, X_2) \le k) = \Pr(X_1 \le k) \cdot \Pr(X_2 \le k) = \left( \frac{k}{n} \right) \cdot \left( \frac{k}{n} \right) = \left(\frac{k}{n}\right)^2$$
    
4. **Compute the Good Split Probability Matrix:**
    
    To find the probability of a good split, calculate the probability that the pivot's rank falls within the acceptable range $\Pr\left(\frac{n}{4} \le R \le \frac{3n}{4}\right)$:
    
    $$\Pr\left(\text{Good Split}\right) = \Pr\left(R \le \frac{3n}{4}\right) - \Pr\left(R < \frac{n}{4}\right)$$
    
    Substitute our cumulative distribution function into the equation:
    
    $$\Pr\left(R \le \frac{3n}{4}\right) = \left( \frac{\frac{3n}{4}}{n} \right)^2 = \left( \frac{3}{4} \right)^2 = \frac{9}{16}$$
    
    $$\Pr\left(R < \frac{n}{4}\right) = \left( \frac{\frac{n}{4}}{n} \right)^2 = \left( \frac{1}{4} \right)^2 = \frac{1}{16}$$
    
    Subtract the two values:
    
    $$\Pr\left(\text{Good Split}\right) = \frac{9}{16} - \frac{1}{16} = \frac{8}{16} = \mathbf{\frac{1}{2}}$$
    
5. **Formulate the Complexity Conclusion:**
    
    Even though picking the larger element shifts the pivot selection toward higher ranks, the probability of hitting a good split remains exactly **$1/2$** (identical to the uniform selection baseline). This ensures that the algorithm still runs in **$O(n)$ expected time**, as the expected number of iterations required to achieve a successful partition is bounded by $\frac{1}{1/2} = 2$.
    

### 5. Summary Matrix of Randomized & Linear Sort Complexities

This matrix tracks the absolute performance profiles and constraints of randomized and linear sorting frameworks.

|**Algorithm Primitive**|**Input/Distribution Constraints**|**Expected Time Complexity**|**Worst-Case Time Complexity**|**Space Complexity**|
|---|---|---|---|---|
|**Universal Hash Dictionary**|Uniform random selection from universal family $\mathcal{H}$|$O(1)$|$\Theta(n)$|$\Theta(n)$|
|**Counting Sort**|Keys must be integers in range $[0, k]$|$\Theta(n + k)$|$\Theta(n + k)$|$\Theta(n + k)$|
|**Radix Sort**|Keys must be bounded by $n^c$|$\Theta(n)$|$\Theta(n)$|$\Theta(n)$|
|**Standard Bucket Sort**|Elements distributed uniformly over $[0, 1)$|$\Theta(n)$|$\Theta(n^2)$|$\Theta(n)$|
|**Compressed Bucket Sort**|$m = \frac{n}{\log n}$ buckets, uniform data input|$\mathbf{O(n \log n)}$|$\Theta(n^2)$|$O(n)$|
|**Randomized Quick-Select**|Distinct element coordinates, uniform selection|$\Theta(n)$|$\Theta(n^2)$|$O(\log n)$ expected|

---
## 🔗 Navigation
**Previous:** [[ ]] | **Next:** [[ ]]