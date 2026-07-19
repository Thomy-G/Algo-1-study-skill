# 📝 Ester Practice Exam 2 - Questions

---

## Question 1 [20 Points]
A company needs to schedule $n$ tasks to be performed by $m$ workers. 
- Each task $i$ has a demand $d_i$ representing the exact number of hours needed to complete it.
- Each worker $j$ has a capacity $c_j$ representing the maximum number of hours they can work.
- Some tasks can only be performed by a subset of workers. Specifically, we are given a set of pairs $S \subseteq \{1,\dots,n\} \times \{1,\dots,m\}$, where $(i, j) \in S$ means task $i$ can be assigned to worker $j$.
- Each worker $j$ can spend at most $h_{ij}$ hours working on task $i$ (where $h_{ij}$ is given).

*   **Your Task:** Describe how to reduce the problem of finding a feasible task assignment (if one exists) to a standard circulation problem with demands and capacities. Prove the correctness of your reduction.

### 🇮🇱 תרגום לעברית:
חברה צריכה לתזמן $n$ משימות לביצוע על ידי $m$ עובדים.
- לכל משימה $i$ יש דרישה $d_i$ המייצגת את מספר השעות המדויק הדרוש להשלמתה.
- לכל עובד $j$ יש קיבולת $c_j$ המייצגת את מספר השעות המקסימלי שהוא יכול לעבוד.
- חלק מהמשימות יכולות להתבצע רק על ידי תת-קבוצה של עובדים. ספציפית, נתונה קבוצת זוגות $S \subseteq \{1,\dots,n\} \times \{1,\dots,m\}$, כאשר זוג $(i, j) \in S$ פירושו שמשימה $i$ יכולה להיות מוקצת לעובד $j$.
- כל עובד $j$ יכול להשקיע לכל היותר $h_{ij}$ שעות בעבודה על משימה $i$ (כאשר $h_{ij}$ נתון).

*   **המשימה:** תארו כיצד לבצע רדוקציה של הבעיה של מציאת הקצאת משימות אפשרית (אם קיימת כזו) לבעיית סירקולציה סטנדרטית עם דרישות וקיבולים. הוכיחו את נכונות הרדוקציה שלכם.

---

## Question 2 [20 Points]
Let $T[0 \dots n-1]$ be a text string and $P[0 \dots m-1]$ be a pattern string over an alphabet $\Sigma$. 
In addition to normal characters, the pattern $P$ can contain a special wildcard character `*` (joker), which matches any character in $\Sigma$.

*   **Your Task:** Describe an algorithm that finds all occurrences of the pattern $P$ in the text $T$ (where wildcards are matched correctly). Your algorithm must run in $O(n \log m)$ time. Explain its correctness and analyze its complexity.

### 🇮🇱 תרגום לעברית:
יהי $T[0 \dots n-1]$ מחרוזת טקסט ו-$P[0 \dots m-1]$ מחרוזת תבנית מעל אלפבית $\Sigma$.
בנוסף לתווים רגילים, התבנית $P$ יכולה להכיל תו כללי מיוחד `*` (ג'וקר), שמתאים לכל תו ב-$\Sigma$.

*   **המשימה:** תארו אלגוריתם המוצא את כל המופעים של התבנית $P$ בטקסט $T$ (כאשר תווים כלליים מותאמים כהלכה). על האלגוריתם לרוץ בזמן $O(n \log m)$. הסבירו את נכונותו ונתחו את סיבוכיותו.

---

## Question 3 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph where the weight of every edge $e \in E$ is either $1$ or $2$.

*   **Your Task:** Prove that Prim's algorithm for finding a Minimum Spanning Tree of $G$ can be implemented to run in $O(|V| + |E|)$ time. Describe the data structures and modification needed to achieve this runtime.

### 🇮🇱 תרגום לעברית:
יהי $G = (V, E)$ גרף קשיר ולא מכוון שבו המשקל של כל קשת $e \in E$ הוא 1 או 2.

*   **המשימה:** הוכיחו כי ניתן לממש את אלגוריתם פריים (Prim) למציאת עץ פריסה מינימלי ב-$G$ כך שירוץ בזמן $O(|V| + |E|)$. תארו את מבני הנתונים והשינויים הנדרשים כדי להשיג זמן ריצה זה.

---

## Question 4 [20 Points]
Let $G = (V, E)$ be a connected, undirected graph with $n$ vertices. We run Karger's randomized algorithm to find a global minimum cut of $G$.

*   **Your Task:**
    1. Prove that the probability that a single run of Karger's algorithm successfully finds a minimum cut of $G$ is at least $\frac{2}{n(n-1)}$.
    2. Analyze the number of independent runs of Karger's algorithm required to ensure that a minimum cut is found with probability at least $1 - \frac{1}{e}$.

### 🇮🇱 תרגום לעברית:
יהי $G = (V, E)$ גרף קשיר ולא מכוון עם $n$ צמתים. אנו מריצים את האלגוריתם האקראי של קארגר (Karger) למציאת חתך מינימלי גלובלי של $G$.

*   **המשימה:**
    1. הוכיחו כי ההסתברות שהרצה בודדת של האלגוריתם של קארגר תמצא בהצלחה חתך מינימלי של $G$ היא לפחות $\frac{2}{n(n-1)}$.
    2. נתחו את מספר ההרצות הבלתי תלויות של האלגוריתם של קארגר הנדרשות כדי להבטיח שחתך מינימלי יימצא בהסתברות של לפחות $1 - \frac{1}{e}$.

---

## Question 5 [20 Points]
Suppose we are given a Linear Program with $n$ constraints in $d$ dimensions:
$$\text{Maximize } c^T x \quad \text{subject to } a_i^T x \le b_i \quad \forall 1 \le i \le n$$
where $d$ is a very small constant (low-dimensional LP).

*   **Your Task:**
    1. Describe the randomized incremental algorithm (Seidel's LP) to solve this problem.
    2. Prove that the expected runtime of the algorithm is $O(d! \cdot n)$.
    3. Explain how the algorithm handles the base case when $d=1$.

### 🇮🇱 תרגום לעברית:
נניח שנתונה תוכנית ליניארית (LP) עם $n$ אילוצים ב-$d$ ממדים:
$$\text{Maximize } c^T x \quad \text{subject to } a_i^T x \le b_i \quad \forall 1 \le i \le n$$
כאשר $d$ הוא קבוע קטן מאוד (LP בממד נמוך).

*   **המשימה:**
    1. תארו את האלגוריתם האקראי האינקרמנטלי של סיידל (Seidel's LP) לפתרון בעיה זו.
    2. הוכיחו כי זמן הריצה המצופה של האלגוריתם הוא $O(d! \cdot n)$.
    3. הסבירו כיצד האלגוריתם מטפל במקרה הבסיס כאשר $d=1$.
