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
