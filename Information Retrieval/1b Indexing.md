# Aim
effective Boolean Model
Inverted Index

# Indexing
We frequently refer to this **data structure** as an **index**, and the process of creating this data structure is called **indexing**.

incidence matrix is one possibility
# Inverted Index
An **inverted index** is a type of data structure where each **term t** is mapped to **a list of documents** that **contain t**.
- term: term as being a **word**

We need **variable-sized list** for each term, called a **postings list**（linked list）
Each **docID** in the list is a **posting**.
**ordered** by the docID
> friend ->  2->3->7

# Construction of an Inverted Index
![](assets/1b%20Indexing/file-20260420195733455.png)
## 1. Token sequence
Sequence (list) of pairs: **(token, docID)**
punctuation removed
lowercase
## 2. Sort
Sort **alphabetically** then by the **docIDs**
## 3: Dictionary & Postings
a term is **recorded only once** for each document

Split into a **dictionary** (**alphabetical order** of terms) and **postings** (ordered list of **documents** each term appears in)

We also record the **document frequency** of each term (the **number** of documents it **appears** in)

| step1                                                | step2                                                | step3                                                |
| ---------------------------------------------------- | ---------------------------------------------------- | ---------------------------------------------------- |
| ![](assets/1b%20Indexing/file-20260420200428967.png) | ![](assets/1b%20Indexing/file-20260420200449683.png) | ![](assets/1b%20Indexing/file-20260420200501081.png) |
## How do we process a query
> Brutus AND Caesar

1. Locate Brutus in the dictionary and retrieve its postings. 
	- What is the time complexity for finding Brutus in our dictionary?
	- logn，用二分查找（因为是排好序的）
2. Locate Caesar in the dictionary and retrieve its postings. 
3. Merge the two postings (find the intersection) of the document sets. 
	- Each of our boolean operators (AND, OR, NOT) is equivalent to a set operation.

## Merging postings lists

# Boolean Operators
## And
AND is used to **narrow a search** by ensuring that all the search terms should appear in the results
It is commonly used to **search** for **relationships** between two concepts or terms.
more terms in search, fewer records
## Or
OR is used to **broaden a search** by retrieving any, some or all of the keywords used in the search statement.
It is commonly used to search for **synonymous terms** or concepts.
The effect of using OR is that documents containing **any number of the terms** specified will be returned.
## NOT
NOT is used to **specifically exclude** a term from your search.
more terms in search, fewer records
