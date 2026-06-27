# Classical IR Pipeline
预处理-创建索引-排序
1. Documents are **preprocessed** to prepare them for use in the IR system. (Tokenisation...)
2. The **indexer** creates a suitable data structure so that retrieval can happen.
3. A **ranking** method matches queries against the index to **produce a ranked list** of results for the user (or to an **evaluation step**).
this is evaluation in terms of the system’s **effectiveness**
## Pipeline Design Pattern
We call this design pattern a **pipeline** because the **output** of one process **becomes** the **input** of the next stage in the process. 
This is similar to a **linear pipeline**, where each stage happens sequentially.
An alternative is a **non-linear pipeline**, where some **stages** can happen in **parallel**.

## Issues
- **Vocabulary mismatch**
	- The words chosen by the user may **not match** the words used in a relevant document.
	- **polysemy**: the same word can have different meanings (e.g. “bank”). 
	- **synonymy**: two different words can have the same meaning (e.g. plane/airplane/aeroplane/aircraft).
- How can we be **confident** that our choice of retrieval model is the **best for each query**?
- Only the **document contents** are taken into account.
	- Particularly on the web, there is a lot more information available. 
		- The link structure of the web: 
			- Where does this page link to?
		- For a large search engines, they have **query logs** to record previous **user’s behaviour** when they give the same query.
- Doesn’t make use of recent advances in **machine learning** (especially deep learning) and **Natural Language Processing** (NLP) and **Natural Language Understanding** (NLU).
	- But, **Languages models** are **slooooooow**
## Speed is of the essence
A typical modern IR system will use **multiple different process** to create the final ranking.
![](/assets/7a%20IR%20Pipelines%20and%20Modern%20IR/file-20260423145941528.png)
# Increasing Recall
bm25会作为后续步骤的输入，而不是直接输出
However, in modern architectures, the **output** of the **basic approaches** (e.g. Boolean retrieval, BM25) will not be shown directly to the users – they are **inputs** to **further processing** to improve the final ranking.

recall很重要，因为一开始没有找到相关文档，后面永远不会找到
Therefore, **recall** is more important at this stage
- If the early retrieval **does not find** some relevant document, that can **never appear** in the final result. 
- If the early retrieval **finds some non-relevant** documents then these can be **moved down** the rankings (or **filtered out** of the results list) by the more sophisticated **techniques later** in the process, before they reach the user.

提高recall的方式：查询扩展，伪相关反馈
**Two classical techniques** that are used to increase recall: **query expansion** and **pseudo-relevance feedback**.
## Query Expansion
**Related terms** are **added** to the **query** to increase the chance of matching relevant documents.

Many sources of related terms: 
- A **manually-created thesaurus**, such as WordNet1 . 
- An **automatically-created thesaurus** from some external corpus like a web crawl, or Wikipedia2  
- **Word embeddings**, where words are represented by vectors (e.g. word2vec, GloVe, ELMo and BERT-based embeddings).
![](/assets/7a%20IR%20Pipelines%20and%20Modern%20IR/file-20260423153655027.png)
using “OR”
## Pseudo-Relevance Feedback
IR输出文档-用户给反馈-重新输出结果
可能会有query drift的问题
Relevance Feedback is when users are **shown** a set of **documents** in response to a query, and tell the system **which ones are relevant**.
- The system can then **use** this **information** to **re-run the search** (with different term weights) and hopefully find more relevant documents that are similar to the ones the user has said are relevant.
- However, **users don’t like** giving relevance feedback!
Pseudo-Relevance Feedback simulates relevance feedback by **assuming** that the **top k ranked documents are relevant**, and then **re-running** the search in the **same way.** 
- Why does this work? “**Terms** that **occur consistently** in relevant documents also tend to **appear consistently** in **top ranked** documents.”
- But it’s not perfect: danger of **query drift** towards the topic of the top-ranked documents.
# Modern IR
#exam A modern Information Retrieval pipeline may include Boolean searches, simple ranking and reranking based on machine learning. Explain why these are all useful to make an effective Information Retrieval system.
1. Boolean search（候选集生成 / 高召回）
	- 用于快速过滤文档
	- 生成一个“可能相关”的文档集合（candidate set）
	- 优点：计算快、覆盖广（high recall）
2. Simple ranking（如 BM25 / VSM）
	- 对候选文档进行初步排序
	- 利用 TF-IDF / BM25 等方法计算相关性
	- 优点：
	    - 高效
	    - 可扩展到大规模语料
	    - 提供初始 ranking
3. Machine Learning reranking（Learning to Rank）
	- 在初始排序结果基础上进行优化排序
	- 使用更多特征（TF, IDF, PageRank, 文档长度等）
	- 优点：
	    - 提升 precision（尤其 top results）
	    - 融合多种信号（text + link + user behavior）
> 答案：Modern IR systems use a pipeline approach where Boolean retrieval is used to generate a high-recall candidate set, simple ranking models such as BM25 provide an efficient initial ranking, and machine learning reranking is applied on top to improve final precision by combining multiple relevance signals; this multi-stage design is necessary because of the trade-off between scalability, speed, and retrieval quality.


![](/assets/7a%20IR%20Pipelines%20and%20Modern%20IR/file-20260423155331348.png)
- QL: Query Likelihood 
- DocLen: Document Length 
- LTR: Learning To Rank 
- RF: Random Forest 
- CEDR: Contextualized Embeddings for Document Ranking
## Fusion
根据多个输入产生一个新的输出
Input: a set of **results** (for one query) from several Ranked Retrieval systems. 
Output: a **single, combined set** of results that are (hopefully) **better than the input** results in isolation.

Can be based on: 
- the **scores** calculated by the Ranked Retrieval systems; 
- the **rank of the documents** in each results list; 
- the **probability** of a document **being relevant**, based on past performance.
## Learning to Rank (LTR)
用机器学习给文档重排
**Machine Learning** used to **re-rank** the documents that have been retrieved.
- Aim: improve the result by putting the **most-relevant** documents at the **top** of the ranked list of results.
**Different** to a lot of **classical machine learning** where problems are either **classification** or **regression**
In **LTR**, the actual scores calculated for each document are not important – the **relative ordering** of the documents is the **key challenge**.
### Features
Each **piece of information** used as an input to the LTR models is known as a **feature** in Machine Learning.
- Textual Features: 
	- Term occurrence/non-occurrence  
	- Term frequency 
	- Inverse Document Frequency 
	- Document Length 
	- Term Proximity 
- Non-Textual Features: 
	- PageRank (see next slide) 
	- URL Depth 
	- Document Quality 
	- Readability 
	- Sentiment 
	- Query Clarity
#### PageRank: Document Importance
Measures the **importance** of a document based on the **link structure of the web**.
A **document** is considered to be **important** if: 
- **Many** pages **link to it**; or 
- Other **important** pages **link to it**.
# Neural Language Models
Bidirectional Encoder Representations from Transformers (**BERT**) is a deep-learning based Large Language Model (LLM) for Natural Language Processing.
- In simple terms, it learns the **relationships between words** to aid document understanding, representing them as **dense vectors**.
## ChatGPT
LLMs are made by **learning patterns** and **relationships** between words/phrases from enormous quantities of text.
- Challenges:
	- It is not designed to provide knowledge and so it is **often confidently wrong**. The output can be plausible and believable, but might still be incorrect. 
		- Often described as “**hallucinating**”.
	- It **cannot cite its sources**, so you cannot find where it got its information from. 
## Retrieval Augmented Generation (RAG)
When a user asks a question, an IR system is **first** used to **find documents** that contain the information required to satisfy the information need.
Important for: 
-  More **reliable generation** of facts. 
- Allowing users to **check original sources**, in case of hallucinations. 
- Events that have **occurred** since the model was **trained**.
![](/assets/7a%20IR%20Pipelines%20and%20Modern%20IR/file-20260423164726711.png)
In some architectures, the **LLM** will be **involved** in the **retrieval process** also, for example by performing **query expansion**