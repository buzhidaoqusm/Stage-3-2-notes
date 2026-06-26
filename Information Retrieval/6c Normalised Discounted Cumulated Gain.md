- The metrics so far make **use of binary** relevance judgments: 
	- Documents are judged to be **relevant or non-relevant**. 
	- Any document that **helps** to satisfy an **information need** in any way is considered to be **relevant**.

NDCG supports **graded relevance judgments**
## Motivation
rewards systems that: 
**Position relevant** documents in **early** positions in the ranking (MAP also rewards this)
Rank **highly-relevant** documents **ahead** of **mildly** relevant ones (MAP treats all relevant documents equally).
排名越靠前分数越高
## Graded Relevance
The first thing that is required is a **set of graded relevance judgments**.
**assume** that a 0-3 scale where **3** is a **highly relevant** document and **0** is a **non-relevant** document
## Example
![](/assets/6c%20Normalised%20Discounted%20Cumulated%20Gain/file-20260423140512804.png)
We create a **gain vector** that records the relevance level at each rank
G = (1, 0, 1, 0, 0, 3, 0, 0, 0, 2, 0, 0, 0, 0, 3)
The gain vector can be thought of as the **amount of information** that a user gains by reading each document(they gain more information from highly-relevant documents)
### Cumulated Gain
$$
CG[i] =
\begin{cases}
G[1], & i = 1 \\
G[i] + CG[i-1], & i > 1
\end{cases}
$$
累加
- e.g. at rank 10:  
	- CG[10] = G[10] + CG[9] = 2 + 5 = 7

- The idea is that **each relevant document** we find should **add to the gain** (i.e. the overall usefulness of the list).
	- It can be thought of as the **total amount of information** that the user has gained to **this point**.
- **More highly relevant** documents **add more to the gain** than less relevant documents.
- BUT: documents **late** in the list can **add as much** to the gain as documents **early** in the list
	- We would **prefer to reward** systems for returning the relevant documents towards the **top of the ranking**.
### Discounted Cumulated Gain
**later documents** add **less** to the gain than earlier ones
It is calculated in the same way as CG, except that the gain at each rank is **discounted** by dividing it by the **log of the rank** (except for the first rank, which is unaffected).
$$
DCG[i] =
\begin{cases}
G[1], & i = 1 \\
\frac{G[i]}{\log_2 i} + DCG[i-1], & i > 1
\end{cases}
$$
![](/assets/6c%20Normalised%20Discounted%20Cumulated%20Gain/file-20260423141619903.png)
#### Analysis
This shows how finding **relevant documents** increases the **quality** of the results to that point. 
- **More relevant** documents contribute **more** to gain. 
- Relevant documents found **earlier** in the ranked list also **contribute more** to gain.

BUT: By itself, this is **not easy to compare** with others. 
- Is 4.2 a good score for this query?
Most evaluation metrics give a single value that is in the range **between 0 and 1**.
### Normalised DCG
is calculated by **comparing** the **DCG vector** against an **Ideal** DCG vector

The Ideal DCG vector is the DCG vector that we would see if the IR system had perfect retrieval. 
- i.e. it **begins with** all the documents of relevance **level 3**. 
- then it includes all the documents of relevance level 2. 
- then it includes all the documents of relevance level 1.

We calculate an Ideal DCG vector to be the **same length** as the **DCG vector** (with **0 relevance values inserted** at the end if there are not enough relevant documents).
#### IDCG
R = {[d3 , 3], [d5 , 3], [d9 , 3], [d25, 2], [d39, 2], [d44, 2], [d56,1], [d71, 1], [d89, 1], [d123, 1]}
Ideal Gain vector:
IG = (3, 3, 3, 2, 2, 2, 1, 1, 1, 1, 0, 0, 0, 0, 0)
计算方式同DCG
IDCG=(3.0, 6.0, 7.9, 8.9, 9.8, 10.5, 10.9, 11.2, 11.5, 11.8, 11.8, 11.8, 11.8, 11.8, 11.8)
#### NDCG
We can **normalise** the DCG by **dividing the score** that was actually achieved at each rank by the ideal score, to yield a score between 0 and 1.
NDCG=DCG(实际的)/IDCG
![](/assets/6c%20Normalised%20Discounted%20Cumulated%20Gain/file-20260423142338169.png)

We still have the problem that we have **15 different scores** for the evaluation.
similar way to Precision@n
NDCG@10 is a very commonly used metric: here it is 0.29
## Features
- Combines document **ranks** and **graded relevance** judgments. 
- Single measure of quality at any rank, **without** needing to know **recall**. 
- NDCG@n only considers relevant documents found **to that point**: not affected by many relevant documents being found very late. 
- Gives **less weight** to relevant documents found **late** in the ranking.
# Which metric should I use?
if **complete judgments** are available **any metric** can be used

**MAP** gives a good indication of performance within a **single metric** as it is averaging the results over multiple queries. 
For collections that do **not** have **complete judgments**, **bPref** is a more suitable metric. 
For tasks such as **web search** (due to **user behaviour**), metrics like **P@10** might be used. 
If **graded relevance judgments** are available, **NDCG** is preferred: this has continually gained in popularity.
# Performing Evaluation
A document collection for IR evaluation consists of: 
- Documents. 
- Standard queries. 
- Relevance judgments for the standard queries.