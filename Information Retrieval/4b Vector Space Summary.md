# TF-IDF Advantages
Using TF gives more influence to a document that contains many occurrences of a particular term.
Using IDF lessens the impact of terms that appear very often in the collection. These terms are regarded as being **less informative** (even when they are not frequent enough to be stopwords).
# Vector Space Model: Advantages and Disadvantages
- Advantages
	- The term weighting schemes can **improve retrieval performance**. 
	- Partial matching strategy allows for retrieval of documents that match part of the query. 
	- The cosine similarity can be used to return a **ranked list** of documents .
- Disadvantage
	- assumption that **terms** are **independent**, which can sometimes harm performance
## Implementation
1. Read in the documents. 
2. Divide each document into its constituent terms. 
3. Remove stopwords. 
4. Stem terms. 
5. Add documents to an index. 
6. Calculate TF-IDF. 
	1. Each term has one IDF score. 
	2. Each term has a separate TF score in each document. 
7. Receive a query. 
8. Retrieve query results.