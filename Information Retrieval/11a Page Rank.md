The ranking score is calculated based entirely on the **content of the document**
Learning to Rank **combines** the ranking **score** with **other features** that might help to indicate the relevance of a document
One feature: importance of a document
# Document Importance
> Example

in academic writing: citation counting
论文被引用越多，越重要

> Citation Counts and the Web

Because of the **presence of hypertext** (i.e. links) in documents on the web, there is an **interconnection** between these **documents**

notable differences:
- **Circular references** （学术中，不能两篇论文互相引用，但是网页可以）
	- in academic publishing, a paper can only cite a paper that has already been published. Later papers are not cited by earlier ones. On the web, two pages can link to one another.
- Quality control（学术发表需要评审，网页谁都可以）
	- in academic publishing, a paper is peerreviewed before it is approved for publication. Thus some quality is maintained in the papers that include citations. On the web, anybody can publish material and link to other documents. It would be a trivial task to write a program that would generate hundreds or thousands of pages containing links to somewhere else.
# PageRank
measure the importance of web pages
based on the **premise** that **documents** that are **linked to** by many other documents are important

A document will tend to have a high PageRank score if: 
- It is linked to by **many documents** and/or 
- It is linked to by **documents** that themselves have a **high PageRank**.
## Link Structure
![](assets/11a%20Page%20Rank/file-20260601162940252.png)
A and B are **backlinks** of C
A and B **have outlinks** to C

high PageRank：
- Many backlinks 
- Backlinks with high PageRank
## Simplified Version
$$
R(u) = \sum_{v \in B_u} \frac{R(v)}{N_v}
$$
- R(u) is the PageRank score for document u. 
- Bu is the set of **all backlinks** of document u. 
- R(v) is the PageRank score for document v. 
- Nv is the number of **outlinks** in document v.

> Example

![](assets/11a%20Page%20Rank/file-20260601164230376.png)

> Question: where does the initial PageRank come from?

we can give an **arbitrary score** to every document

These **continue to be recalculated** until the **scores converge** (i.e. calculating again **does not change** the scores, or changes them **very little**)

## rank sink

which refers to a group of pages that have at least one backlink and link to one another, but do **not link** to anywhere else **outside** the group
![](assets/11a%20Page%20Rank/file-20260601165413442.png)

### Combating Rank Sinks
$$
R(u) = (1-d) + d \times \sum_{v \in B_u} \frac{R(v)}{N_v}
$$
damping factor (d), which ensures that **not all of a document**’s PageRank is passed on **via its outlinks**
## No Backlinks
a document with no backlinks would have a PageRank of **zero**

With the **modified formula**, a document with no backlinks has a PageRank of **(1-d)** to contribute to the documents it links to.

![](assets/11a%20Page%20Rank/file-20260601171723839.jpg)

Brin and Page were using a database of 322 million links and believed that the convergence had reached a reasonable level after 52 iterations.
Doubling the size of the document collection does not double the time taken to converge.