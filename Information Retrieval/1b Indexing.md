# Aim
effective Boolean Model
Inverted Index

# Indexing
We frequently refer to this **data structure** as an **index**, and the process of creating this data structure is called **indexing**.

incidence matrix is one possibility
# Inverted Index
term->文档id->文档id

An **inverted index** is a type of data structure where each **term t** is mapped to **a list of documents** that **contain t**.
- term: term as being a **word**

We need **variable-sized list** for each term, called a **postings list**（linked list）
Each **docID** in the list is a **posting**.
**ordered** by the docID
> friend ->  2->3->7

# Construction of an Inverted Index
![](/assets/1b%20Indexing/file-20260420195733455.png)
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
| ![](/assets/1b%20Indexing/file-20260420200428967.png) | ![](/assets/1b%20Indexing/file-20260420200449683.png) | ![](/assets/1b%20Indexing/file-20260420200501081.png) |
## How do we process a query
> Brutus AND Caesar

1. Locate Brutus in the dictionary and retrieve its postings. 
	- What is the time complexity for finding Brutus in our dictionary?
	- logn，用二分查找（因为是排好序的）
2. Locate Caesar in the dictionary and retrieve its postings. 
3. Merge the two postings (find the intersection) of the document sets. 
	- Each of our boolean operators (AND, OR, NOT) is equivalent to a set operation.

## Merging postings lists

Merging two sorted postings lists (**AND** operation):

```
INTERSECT(p1, p2)
  result = {}
  while p1 != NIL and p2 != NIL
    if p1.docID == p2.docID
      ADD(result, p1.docID)
      p1 = p1.next
      p2 = p2.next
    else if p1.docID < p2.docID
      p1 = p1.next
    else
      p2 = p2.next
  return result
```

- Time complexity: **O(x + y)** where x, y are the lengths of the two lists
- Worst case (two distinct lists with **nothing in common**)

# Boolean Operators
#exam The Boolean Model makes use of the query operators AND, OR and NOT. Explain how these work and how they affect the number of documents returned by an Information Retrieval system. Also show how each of these can be implemented by using operations from Set Theory.
## And
AND is used to **narrow a search** by ensuring that all the search terms should appear in the results
It is commonly used to **search** for **relationships** between two concepts or terms.
more terms in search, fewer records
`P ∩ C`
## Or
OR is used to **broaden a search** by retrieving any, some or all of the keywords used in the search statement.
It is commonly used to search for **synonymous terms** or concepts.
The effect of using OR is that documents containing **any number of the terms** specified will be returned.
`C ∪ U`
## NOT
NOT is used to **specifically exclude** a term from your search.
more terms in search, fewer records
`P \ C`

# Question
## Adapting the Merge Algorithm
### Brutus OR Caesar (Union)

```
UNION(p1, p2)
  result = {}
  while p1 != NIL and p2 != NIL
    if p1.docID == p2.docID
      ADD(result, p1.docID)
      p1 = p1.next
      p2 = p2.next
    else if p1.docID < p2.docID
      ADD(result, p1.docID)
      p1 = p1.next
    else
      ADD(result, p2.docID)
      p2 = p2.next
  while p1 != NIL
    ADD(result, p1.docID)
    p1 = p1.next
  while p2 != NIL
    ADD(result, p2.docID)
    p2 = p2.next
  return result
```

- When docIDs differ, add the smaller one to result
- Append remaining documents at the end
- Time complexity: **O(x + y)**

### Brutus AND NOT Caesar (Difference)

```
DIFFERENCE(p1, p2)
  result = {}
  while p1 != NIL and p2 != NIL
    if p1.docID == p2.docID
      p1 = p1.next  // Skip: appears in both
      p2 = p2.next
    else if p1.docID < p2.docID
      ADD(result, p1.docID)  // Keep: only in p1
      p1 = p1.next
    else
      p2 = p2.next  // Advance p2 until it catches up
  while p1 != NIL
    ADD(result, p1.docID)
    p1 = p1.next
  return result
```

- Skip when docIDs match (appears in both)
- Only keep documents from p1
- Time complexity: **O(x + y)**

### Brutus OR NOT Caesar

**Cannot be done with two-pointer merge alone!**

"NOT Caesar" means returning **all documents** except those containing Caesar.

**Required additional information:**
- Total number of documents **N**
- Or a list of **all document IDs**

```
OR_NOT_CAESAR(Brutus, N, Caesar)
  result = {}
  all_docs_containing_Caesar = get_postings(Caesar)
  
  for docID in Brutus.postings
    ADD(result, docID)
  
  for docID in 1 to N
    if docID not in all_docs_containing_Caesar
      ADD(result, docID)
  
  return result
```

- Time complexity: **O(N)** — need to traverse all N documents

## Summary

| Query Type | Algorithm | Complexity |
|------------|-----------|------------|
| AND | Intersection | O(x + y) |
| OR | Union | O(x + y) |
| AND NOT | Difference | O(x + y) |
| OR NOT | Needs N or all docs list | O(N) |
