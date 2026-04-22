we must first calculate the **score for each query**, and then get the **average** over all the queries.
# Why so many metrics
Each of them tells us **different things**.
# Precision at n
This is particularly suitable for **web search systems** where people typically look no further than the **first few results**.

interested in the precision after 10 documents have been retrieved => Known as “**Precision at 10**”, or “**P@10**”

This is a **measure of the quality** of the results that a user is **likely to look at**.

$$P@n=\frac{\text{Relevant in top n results}}{n}$$
## Example
![](assets/6a%20Single%20Value%20Metrics/file-20260422214348037.png)
P@3 = 2/3; P@10 = 4/10

## Problems
One problem with using P@n is that its **performance is affected** by the **number of relevant documents available**.

For a query that has **only 10 relevant documents**, a good **P@10** score is difficult.
If there are **1,000 relevant** documents, a good P@10 score is **easy**.
# R-precision
A similar metric is R-precision, where we are interested in the precision after the **top R documents** are returned (where R is the **number of relevant documents** for that query).

This is similar to P@n except that **n is not fixed for all queries** (because the number of relevant documents will be different for each query).

$$R -Precision=\frac{\text{Relevant in top r results}}{r}$$
## Example
