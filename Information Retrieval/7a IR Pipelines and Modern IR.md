# Classical IR Pipeline
1. Documents are **preprocessed** to prepare them for use in the IR system. (Tokenisation...)
2. The **indexer** creates a suitable data structure so that retrieval can happen.
3. A **ranking** method matches queries against the index to **produce a ranked list** of results for the user (or to an **evaluation step**).
this is evaluation in terms of the system’s **effectiveness**
## Pipeline Design Pattern
We call this design pattern a **pipeline** because the **output** of one process **becomes** the **input** of the next stage in the process. 
This is similar to a **linear pipeline**, where each stage happens sequentially.
An alternative is a **non-linear pipeline**, where some **stages** can happen in **parallel**.

## Issues
- Vocabulary mismatch
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
![](assets/7a%20IR%20Pipelines%20and%20Modern%20IR/file-20260423145941528.png)
# Increasing Recall
However, in modern architectures, the **output** of the **basic approaches** (e.g. Boolean retrieval, BM25) will not be shown directly to the users – they are **inputs** to **further processing** to improve the final ranking.

Therefore, **recall** is more important at this stage
- If the early retrieval **does not find** some relevant document, that can **never appear** in the final result. 
- If the early retrieval **finds some non-relevant** documents then these can be **moved down** the rankings (or **filtered out** of the results list) by the more sophisticated **techniques later** in the process, before they reach the user.

**Two classical techniques** that are used to increase recall: **query expansion** and **pseudo-relevance feedback**.
## Query Expansion
**Related terms** are **added** to the **query** to increase the chance of matching relevant documents.

Many sources of related terms: 
- A **manually-created thesaurus**, such as WordNet1 . 
- An **automatically-created thesaurus** from some external corpus like a web crawl, or Wikipedia2  
- **Word embeddings**, where words are represented by vectors (e.g. word2vec, GloVe, ELMo and BERT-based embeddings).
![](assets/7a%20IR%20Pipelines%20and%20Modern%20IR/file-20260423153655027.png)
