The theory behind retrieval says that the **closer the vectors** are in the N-dimensional space, the **more similar** the items they represent are.
**query** vector is **compared** to **all of the document** vectors
# Cosine Similarity: Why?
Principle: the **smaller the angle** between two vectors, the **more similar** they are.

Therefore, the **cosine** function gives a value **between 0** (for very different documents) and **1** (for identical documents).
This is **ideal for a measure of similarity**.

![](assets/3c%20Cosine%20Similarity/file-20260422160036222.png)
(dj ) ⃗ is a vector representing a document dj
q ⃗  is a vector representing the query q
T is the number of **distinct terms** in the entire document collection
w(i,j) is the weight of term i in document j
w(i,q) is the weight of term i in the query q
