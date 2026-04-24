Query expansion (QE) is a process in Information Retrieval which consists of **selecting and adding terms** to the user’s query with the goal of **minimizing** query-document **mismatch** and thereby **improving retrieval performance**
- Generally works by **increasing recall** by matching **more documents** than the original query would match.
- Can be done **automatically or interactively** (with user involvement, similar to relevance feedback).
## Interactive Query Expansion
suggested some query expansion options for the user to choose
![](assets/8a%20Query%20Expansion/file-20260423165533258.png)
## Automatic Query Expansion
The original query “cancer” has been automatically expanded with several related search terms.
![](assets/8a%20Query%20Expansion/file-20260423165556625.png)
## Resources for Query Expansion
main information used is **synonymy**
A **thesaurus** is a type of database that **collects synonyms** (or near-synonyms) 
- **Manual** thesaurus (maintained by human editors) 
- **Automatically derived** thesaurus 
- Using **word embeddings** as a similar effect, without using a thesaurus.
**Query Log Mining**: based on queries **previously submitted** by other users.
## Thesaurus-based Query Expansion
For each term in the query, expand the query by **adding words** from the **thesaurus** that are similar to it. 
- e.g. ”cancer” à “cancer, cancerous, neoplasms” etc. 
Generally **increases recall**, but may significantly **reduce precision**. 
- e.g. ”interest rate” à “interest fascinate rate” 
Widely used in specialised search engines for **science and engineering**. 
Creating and maintaining a **manual** thesaurus is **expensive**, and is a lot of work!
### Automatic Thesaurus Creation
A thesaurus can be created automatically by **analysing the distribution** of words in documents. 
2 possible definitions: 
1. Two words are similar if they **co-occur** with **similar words** 
	- (e.g. “car” and “truck” may be similar because they both occur with words like “road”, “driver”, “license”, etc.) 
2. Two words are similar if they occur in particular **grammatical relations** with the same words 
	- (e.g. you can “harvest”, “peel”, “eat”, “prepare” apples and pears, so apples and pears may be similar).
## Word Embeddings for Query Expansion
**neural network** approaches to create **word embeddings**: a **representation of word** as **vectors** in an n-dimensional space.

Because words are represented as vectors, a **similarity between words** can easily be calculated.
余弦相似度
## Search Engine Query Expansion(Query Log Mining)
The query log stores details about **previous queries** and **user behaviour**. 
- Example: After searching for “palm”, many users next search for “palm oil”. 
- Example: Users searching for “football” and “soccer ball” often click on the same URL in the search results.