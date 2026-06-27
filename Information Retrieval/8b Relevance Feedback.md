#exam The probabilistic model of Information Retrieval makes use of two probabilities relating to query terms. These are 𝑃(𝑘!|𝑅) (the probability that a relevant document will contain the term ki) and 𝑃(𝑘!|𝑅') (the probability that a non-relevant document will contain the term ki). However, these probabilities cannot be calculated directly and must be estimated. 
(i) Briefly describe how initial values for these probabilities may be generated. (ii) Explain how these initial estimates can be improved with user feedback.

Typical process: 
1. User provides a **query** (usually short and simple) 
2. The system returns an **initial set of results**. 
3. The user **marks** some **relevant** and **nonrelevant** documents (in some situations the user only marks relevant results). 
4. The system computes a **better representation** of the information need based on this feedback. 
5. The system returns a **revised set** of results. 
6. Possibly the above process is repeated.

# Rocchio Algorithm
Basic Idea
- Based on the idea that **documents and queries** are represented as **vectors**
- Wants to find a **query vector** that **maximises similarity** with **relevant** documents and **minimises similarity** with **nonrelevant** documents.
![](/assets/8b%20Relevance%20Feedback/file-20260427101345630.png)

> example

![](/assets/8b%20Relevance%20Feedback/file-20260427101614012.png)
离relevant近

## Centroids
cluster的中心
$$
Centroid(C) = \frac{1}{|C|} \sum_{\vec{d}_j \in C} \vec{d}_j
$$
## Rocchio Algorithm

$$
\vec{q}_{opt} = \frac{1}{|C_r|} \sum_{\vec{d}_j \in C_r} \vec{d}_j - \frac{1}{|C_{nr}|} \sum_{\vec{d}_j \in C_{nr}} \vec{d}_j
$$
Informally, this is the vector that is obtained by **subtracting** the centroid of the **nonrelevant** set **from** the centroid of the **relevant** set.
But: In practice we **don’t yet know** the **relevant** and **nonrelevant** sets!

> In practice

we have access to a **subset** of the **relevant and nonrelevant** sets, which have been identified by the user
The Rocchio algorithm **takes this information** and **computes a modified query vector** (or “revised query vector”)

formula to calculate the modified query vector
$$
\vec{q}_m = \alpha \vec{q}_0 + \beta \frac{1}{|D_r|} \sum_{\vec{d}_j \in D_r} \vec{d}_j - \gamma \frac{1}{|D_{nr}|} \sum_{\vec{d}_j \in D_{nr}} \vec{d}_j
$$
`q0` is the original query provided by the user.
`𝐷𝑟` and `𝐷𝑛𝑟` are the documents that have been identified by the user
`𝛼`, `𝛽` and `𝛾` are weights
- They control the **balance** between **trusting** the judged sets of **relevant** and **nonrelevant** documents and trusting the **original query**.
- With **many judged** documents, **𝛽 and 𝛾 might be high.**
- qm靠近Dr的中心，远离Dnr

relevance feedback is very helpful to **increase recall**
Positive feedback (i.e. identifying **relevant** documents) is more **useful** than negative feedback (i.e. identifying **nonrelevant** documents), so in most systems **𝛾 < 𝛽**
Reasonable values might be 𝛼 = 1, 𝛽 = 0.75 and 𝛾 = 0.15.
## Assumptions
The user must have enough knowledge to make an **initial query** that is **close** to the **relevant** documents.

Some **problems** that relevance feedback cannot solve by itself
- Misspellings
- Vocabulary Mismatch
- Cross-language IR
	- where documents in different languages will not be close in the vector space

Relevant documents are assumed to be **close** to each other **in a cluster**.

But sometimes relevant documents can be in several clusters:
- Documents might use **different vocabulary** (e.g. astronaut vs. cosmonaut)
- A query that combines two **very different things**
- General concepts that combine **many more specific subjects** (e.g. ”felines” includes both wild animals and domestic pets)

## Pseudo Relevance Feedback
users generally do not wish to provide feedback (known as **explicit feedback**)
Instead, **pseudo relevance feedback** (also called blind relevance feedback) can be used: **assume the top k ranked documents are relevant** and then **apply** a relevance feedback **algorithm**

### Indirect Relevance Feedback
implicit feedback, For example: 
- Which documents does the user choose to click on? 
- How long did the user spend viewing a document? 
- How long did the user spend browsing the results?

