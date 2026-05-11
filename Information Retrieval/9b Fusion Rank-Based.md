# Interleaving
take **one document** from the **top of each** input set in a "round robin" fashion and **add** it to the **fused result set**

poor effectiveness

![](assets/9b%20Fusion%20Rank-Based/file-20260511202753177.png)

# Borda-Fuse
a few voters (input systems) vote for many candidates (documents)

- For each voter, the **top-ranked** candidate is given **c** points, the **second** ranked is given **c-1** points, etc
- If a candidate is **not ranked by a voter**, the voter’s **remaining points** are **divided evenly** among unranked candidates.
- If there are n candidates that were unranked by a voter, that voter gives each of them  (𝑛+1)/2 votes

> Example

![](assets/9b%20Fusion%20Rank-Based/file-20260511204722018.png)
14 unique documents: c = 14
System A gives 2.5 points to each of the 4 documents it didn’t choose: i.e. (4+1)/2
System B gives 3.5 points to each of the 5 documents it didn’t choose: i.e. (5+1)/2

![](assets/9b%20Fusion%20Rank-Based/file-20260511204800516.png)

# Reciprocal Rank Fusion
a simple rank-based method
the score for each document: $RRF_{score}(d \in D) = \sum_{r \in R} \frac{1}{k + r(d)}$
