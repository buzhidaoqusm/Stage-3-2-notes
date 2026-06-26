# Cranfield Paradigm的问题
所有的文档都是相关/不相关，也就是说所有的文档都必须要被人工评判过，不存在没有评判的情况
**All of the evaluation** techniques we have mentioned so far are based on the **Cranfield Paradigm**
- **test collections** and **queries** are created that have a **known set of relevant documents** associated with them

The point is that for each query, **every document** in the collection is judged to be “**relevant**” or “**non-relevant**”.
Where this occurs, we say that we have “**complete relevance judgments**”.

With **smaller collections**, this **Cranfield Paradigm is perfect** (i.e. complete relevance judgments exist).
However, as **document** collections have become **larger**, **complete judgments** have become **less common** and we have **incomplete judgments**
## How do incomplete relevance judgments happen?
文本检索会议 (TREC) 每年都会设定 IR 任务（称为“track”）
发布了一个语料库和一些标准查询（称为topics）
邀请参与小组使用他们的系统搜索每个主题的语料库，并向组织者提交一组结果。 

组织者不会把整个 corpus 都人工判断一遍，因为太大了，不现实，所以只对pool中的judge。
The organisers use a system called **pooling** to decide which **documents to judge**.
the **top n documents** returned in each set of results for every query are **gathered into a pool**
judge the **relevance** of only the documents **in the pool** for each query

This is based on the **assumption** that **different groups** using a variety of state-of-the-art retrieval techniques should, collectively, **find all the relevant documents** that were available in the corpus.

In practice, this is **not guaranteed**
# bpref: Binary Preference
没有被judge过的Document不应该产生影响，也就是说不应该把它认为是non relevant
For the evaluation metrics we have seen so far, they are simplified by **assuming** that **unjudged** documents are **non-relevant**.

It was noticed that **returning** many **unjudged** documents had the effect of **lowering evaluation score**.
- This does **not mean** that the **retrieval was worse**: whether a document is relevant or not is not affected by whether somebody has judged it.
![](/assets/6b%20bpref/file-20260423133505176.png)

The idea behind bpref is that these **unjudged documents** should **not impact** so largely on the **evaluation score**.
The **only documents** considered are those that were **judged relevant** or **judged non-relevant**.

$$
B = \frac{1}{R}\sum_{r \in R}\left(1 - \frac{\left|\, n \text{ ranked higher than } r \,\right|}{R}\right)
$$
R relevant documents
n is a member of the first R judged non-relevant documents

- In other words: 
	- For each relevant document in that was retrieved in the answer set: 
		- Count the **number of non-relevant** documents **above** it in the result set (this is |𝑛 𝑟𝑎𝑛𝑘𝑒𝑑 ℎ𝑖𝑔ℎ𝑒𝑟 𝑡ℎ𝑎𝑛 𝑟|). This **cannot be greater** than the total number of relevant documents (**R**)
		- 排名在R上面的
		- The score for that document is $1 - \frac{\left|\, n \text{ ranked higher than } r \,\right|}{R}$
![](/assets/6b%20bpref/file-20260423134936842.png)
## Problems
when **R is very small** (i.e. there are only one or two relevant documents) it fails

**bpref-10**
$$
bpref10 = \frac{1}{R}\sum_{r \in R}\left(1 - \frac{\left|\, n \text{ ranked higher than } r \,\right|}{10+R}\right)
$$
where **n** is a member of the **first 10+R judged** nonrelevant documents.
## Comparison
As the judgments become less complete, bpref is more **stable** than the others.
![](/assets/6b%20bpref/file-20260423135609187.png)
y 轴：评估指标得分
x 轴：使用判断的百分比（他们不断删除判断文件并重新评估）