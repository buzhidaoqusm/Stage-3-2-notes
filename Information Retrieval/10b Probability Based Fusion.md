# ProbFuse
## Motivation
Using a **single weight** for each system **only reflects** that the **overall average performance** of one system is better (or worse) than another
may miss some characteristics:
- An input system may tend to return **a few relevant documents** at the start of its results, so **precision and recall** may both be **poor**.
- Another system could have good recall, but be **poor at ranking** its results so as to place relevant documents first.
## Overview
Each input result set is divided into x **equal-sized segments**.
The score is based on the **probability** that the **document is relevant**
**probabilities** are calculated by examining **historical data** from each input system

two phases:
- **Training Phase**: Probabilities for each input system estimated, based on **historic queries** for which **relevance judgments** are **available**.
	- x: how many **segments** to divide the result set into（分成几段）
	- k: segment number (k=1 is the first segment, k=2 the second, etc.) （第几段）
	- ![](assets/10b%20Probability%20Based%20Fusion/file-20260518220234663.png)
- **Fusion Phase**: Probabilities used to **calculate ranking scores** for each document. Used to rank final output result set.
### Training
A **set of probabilities** must be calculated for **every input system**.
For this, a set of **training queries is required**, where relevance judgments are available.

Calculate the **probability** that **document d** returned in **segment k** is relevant.
1. Count all of the relevant documents in segment k 
2. Divide by total number of documents in segment k 
3. Average over all training queries
![](assets/10b%20Probability%20Based%20Fusion/file-20260518220747515.png)
### Fusion
The score assigned to a document by each underlying input system is the **probability of relevance** associated with the **segment it is returned in**, divided by the **segment number**.
- **Dividing by the segment number** gives **highly-ranked** documents an additional **boost**, so as to exploit the **Skimming Effect**.
- The document's final ranking score is found by **adding each individual score**. 
	- This exploits the **Chorus Effect**

only suitable for data fusion
# SegFuse
## Motivation
ProbFuse 分成 25 segments 时效果优于CombMNZ

The input result sets used were up to **1000 documents in length**, meaning that **each segment** contained **40 documents.**
This means that **document 40** would be treated the **same as document 1**.

Two major **variations** to ProbFuse proposed: 
- Instead of constant segment sizes, the **size** of the segments **increase exponentially** as we go down the result set. 
- **Normalised scores** used to boost the **Skimming Effect**, rather than dividing by the segment number.

## Training
probability is calculated in exactly the **same way** as for **ProbFuse**
**only difference** is that the **size of the segment**

$$
Size_k = (10 \times 2^{k-1}) - 5
$$
![](assets/10b%20Probability%20Based%20Fusion/file-20260518222835133.png)
# SlideFuse
## Motivation
In particular, **adjacent documents** could be **treated very differently**, for instance under SegFuse: 
- Document 20 would be grouped with documents 6-20 
- Document 21 would be grouped with documents 21-55
## Overview
**Aside**: Why not just **use the probability** at **each rank** in the result set? 
- Expecially for large-scale tasks, there may be **few judged relevant documents** available, but we **don't want to** give a probability of **zero** to a rank just because **no relevant document was returned** in that exact rank during training

we instead use **each rank's neighbours** as evidence
We describe this as using "**sliding windows**"
![](assets/10b%20Probability%20Based%20Fusion/file-20260518230151188.png)
### Distribution of Probabilities
![](assets/10b%20Probability%20Based%20Fusion/file-20260518225434507.png)


## Training
calculate the probability of relevance at each rank r
**Number of relevant documents** returned at rank r **divided by** number of **training queries**.
## Fusion
final score is the sum of the scores a document receives from the input systems

![](assets/10b%20Probability%20Based%20Fusion/file-20260518230945531.png)