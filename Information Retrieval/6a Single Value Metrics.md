we must first calculate the **score for each query**, and then get the **average** over all the queries.
# Why so many metrics
Each of them tells us **different things**.
# Precision at n
This is particularly suitable for **web search systems** where people typically look no further than the **first few results**.

interested in the precision after 10 documents have been retrieved => Known as “**Precision at 10**”, or “**P@10**”

This is a **measure of the quality** of the results that a user is **likely to look at**.

$$P@n=\frac{\text{Relevant in top n results}}{n}$$
## Example
![](/assets/6a%20Single%20Value%20Metrics/file-20260422214348037.png)
P@3 = 2/3; P@10 = 4/10

## Problems
One problem with using P@n is that its **performance is affected** by the **number of relevant documents available**.
受相关文档的数量影响

For a query that has **only 10 relevant documents**, a good **P@10** score is difficult.
If there are **1,000 relevant** documents, a good P@10 score is **easy**.
# R-precision
A similar metric is R-precision, where we are interested in the precision after the **top R documents** are returned (where R is the **number of relevant documents** for that query).

This is similar to P@n except that **n is not fixed for all queries** (because the number of relevant documents will be different for each query).

$$R -Precision=\frac{\text{Relevant in top r results}}{r}$$
## Example
R是人工选出来的，也就是rel
就是取Ret的前rel个结果，看看有多少个正确的
# Mean Average Precision (MAP)
Unlike simple precision, which is set-based, it **rewards** systems that **rank relevant documents** at the beginning of the results returned.
Unlike P@10, it continues to **examine the later stages** of the ranked list, although with **lesser weight**.

three steps:
1. calculate the precision at each **recall point** (at each rank where a relevant document is found)
2. The **Average Precision** for this query is found by **dividing the sum** of these precision calculations by the total number of relevant documents **|Rel|**.
3. As with most metrics, for multiple queries the same procedure must be performed for each. We must calculate the **mean of the queries**’ average precision values, giving us Mean Average Precision.
## Example
![](/assets/6a%20Single%20Value%20Metrics/file-20260422220440917.png)
$$\text{Average Precision} = \frac{\sum_{k=1}^{n} P@k \times rel@k}{r}$$
n is the number of retrieved documents 
P@k is precision at rank k. 
rel@k is 1 if the document at rank k is relevant, or 0 if not. 
r is the number of relevant documents available for retrieval (i.e. |Rel|)

(1+0.67+0.5+0.4+0.33)/10