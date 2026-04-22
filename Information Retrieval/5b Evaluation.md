There are many measureable quantities for this: 
1. Processing: How quickly does the user receive a response? How well are resources utilised?
2. User Experience: Does the user enjoy using the system? 
3. Search: How effective is the system in satisfying the user’s information need?

关注3
![](assets/5b%20Evaluation/file-20260422203740753.png)

A **relevant document** is one that (at least partially) **satisfies** a user’s information **need**.

As an alternative, we use **experts to judge** whether each document is relevant to each query (the Cranfield experiments used aeronautical engineers)

This results in **standard test collections**, that researchers use to test how effective their systems are. These consist of:
- A **standard corpus** used for all queries. 
- A set of **standard queries**. 
- A set of **relevance judgments** – i.e. for each query, the human judges provide a **list of all the documents** in the corpus that are relevant to that query.

3 sets of documents
- **C**: The collection or **corpus** is the set of all **available documents** (provided in the test collection)
- **Rel**: The relevant set is the **set of documents** that have been **judged** by the experts to be **relevant** for that **query** (provided in the test collection).
- **Ret**: The answer set or retrieved set is the **set of documents** that an **IR** system has **returned** in response to a query (returned by any system that is to be tested).

让rel和ret找到的Document相同
![](assets/5b%20Evaluation/file-20260422204435751.png)

The **purpose** of evaluating the effectiveness of an IR technique is to **evaluate** the quality of this **ranked list**.
