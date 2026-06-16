# Term Weights
We can also take the number of times a **term occurs** in a document into account (i.e. the **frequency of the term**)

A weight wi,j > 0 is associated with every **index term ki** of **document dj**

**Index terms** are usually assumed to be **independent**, though this is a simplification.
# TF-IDF: 
- TF: Term Frequency 
	- Every **term** has a separate **term frequency** for every document in the IR system.
	- appears frequently->higher TF
- IDF: Inverse Document Frequency
	- terms that **only occur in very few documents** are more important
	- Each term has **only one** inverse document **frequency** within an entire document collection
	- appears infrequently->higher IDF
### TF
![](/assets/4a%20Term%20Weighting/file-20260422165729592.png)
𝑡𝑓𝑖,𝑗 is the term frequency of term i in document j

where **𝑓𝑟𝑒𝑞𝑖,𝑗** is the number of times **term i occurs** in document j and **𝑚𝑎𝑥𝑓𝑟𝑒𝑞𝑗** is the **maximum** number of times any term **occurs** in document j (i.e. the frequency of the most common term in document j).

normalised frequency->TF scores will range **bewtee 0 and 1**

The reason we divide by the maximum frequency is to ensure that **terms in long documents** do not get an **unfair advantage**.
### IDF
each term only has **one IDF score** within an entire **document collection**
![](/assets/4a%20Term%20Weighting/file-20260422170722042.png)
N is the total **number of documents** in the collection 
ni is the number of **documents** in the collection that **contain term i**.
### TF-IDF
term i in a document j
𝑤𝑖,𝑗 = 𝑡𝑓𝑖,𝑗 × 𝑖𝑑𝑓i
