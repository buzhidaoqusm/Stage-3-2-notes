# Precision
Precision is the **fraction** of the set of **retrieved documents** (Ret) that are **relevant** (i.e. that are also in Rel

$$Precision=\frac{RelRet}{Ret}$$

# Recall
Recall is the fraction of the relevant documents (Rel) that have been retrieved (i.e. they are also in Ret).
$$Pecall=\frac{RelRet}{Rel}$$

# Precision vs. Recall
The two metrics of precision and recall are often **inversely related**: as **one increases** the other **decreases**.

- **Precision** will be **high** whenever a system is good at **avoiding non-relevant** documents. 
	- A system can achieve very high precision by retrieving very few documents.
- **Recall** will be **high** whenever a system finds **many relevant** documents. 
	- A system can achieve 100% recall simply by **retrieving all the documents** in the collection.

Which is most important?
- **task-dependent**
- **Example**:
	- Users searching the web want high precision(they want the returned documents to be relevant)
		- Because of the **size of the web**, there are very **many documents** that will help satisfy the information need, so the user **does not need to examine all of them** (i.e. recall does not need to be high)
	- On the other hand, a patent **lawyer** researching a patent must ensure that they **get all of the relevant documents** and hence they want high recall.
		- They will **tolerate lower precision** to facilitate this (i.e. they are more likely to be willing to read through some non-relevant documents).

Ideally every IR system would have **both recall and precision of 100%** (the answer set is equal to the relevant set).
# Problems with Precision
没有考虑排序，顶部的应该更重要
Basic Precision is a **set-based**, **unranked** retrieval metric (as is recall)
Most IR systems **return ranked lists**, not sets of documents, so **users** are most likely to look at the **top of the list first**.

![](/assets/5c%20Precision%20&%20Recall/file-20260422213443627.png)
R = Relevant
BUT, clearly **Engine B is better**, because its top-ranked documents are relevant, as a user would expect.