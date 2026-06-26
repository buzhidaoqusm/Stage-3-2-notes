# Boolean Model Disadvantages
没有部分匹配的概念
The model predicts that each document is **either relevant or non-relevant** to the query.
There is **no** notion of a “**partial**” match to the query conditions
**no ranking** occurs.

# Vector Space Model
This **models** both **documents** and **queries** in an N-dimensional space.
Basic **vector algebra** is used to calculate the most **similar** documents to a query.

- A collection contains a set of **documents** each of which is **composed of** a “bag of words” (also known as **terms**).
- **Each unique term** in the collection is represented by **a dimension** in the vector space
- A **document** is then **represented** by giving a positive non-zero **value** to the **dimension** representing a term it contains.
	- This number is known as a **term weight**.
- Other dimensions get a value of **zero**. （not contain）
- A **similarity score** is calculated based on the **proximity of two vectors**

the Vector Space Model facilitates **partial matching** by using **non-binary term weights**
similarity score：**sim(q,d)** (i.e. the similarity between a query **q** and a document **d**)
These models return a **ranked list of documents**, where the documents with the **highest similarity scores** are at the **top** of the list.
## Example
**documents**:  
1. Information Retrieval is an exciting subject  
2. Mathematics is important in Information Retrieval
**6 distinct terms**: information; retrieval; exciting; subject; mathematics; important.

vector space will have **six dimensions**
wi,j refers to the weight of **term i** in **document j**
0≤w(i,j)≤1

query: important information  =>  (1, 0, 0, 0, 0, 1)
the **order** of the **terms** in the vector is the same as for the **corpus**
![](/assets/3b%20Vector%20Space%20Model/file-20260422152835629.png)