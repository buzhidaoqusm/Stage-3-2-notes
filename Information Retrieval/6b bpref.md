# Cranfield Paradigm的问题
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
For the evaluation metrics we have seen so far, they are simplified by **assuming** that **unjudged** documents are **non-relevant**.

It was noticed that **returning** many **unjudged** documents had the effect of **lowering evaluation score**.
- This does **not mean** that the **retrieval was worse**: whether a document is relevant or not is not affected by whether somebody has judged it.
![](assets/6b%20bpref/file-20260423133505176.png)
