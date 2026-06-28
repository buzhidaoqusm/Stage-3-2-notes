# Term Weights
We can also take the number of times a **term occurs** in a document into account (i.e. the **frequency of the term**)

A weight **wi,j** > 0 is associated with every **index term ki** of **document dj**

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

#exam Below is a small document collection, containing three documents. Answer the questions that follows. All calculations should be presented in decimal format, and be correct to three decimal places.

Stopwords: `a, an, is, some, the`

Documents:
1. `Her doctor gave her some very, very bad news.`
2. `A no news day is a good news day.`
3. `An apple a day keeps the doctor away.`

(i) Calculate a vector to represent each document, using the TF-IDF weighting system. You should use the stopword list provided, but do not perform stemming or lemmatisation.

(ii) Calculate the cosine similarity for each vector using the query "What is her news?" and show the final ranked list of documents for this query.

#exam Below is a small document collection, containing three documents. Answer the questions that follow.

Stopwords: `and, be, is, it, to, will`

Document 1: `It is going to rain and rain and rain today.`

Document 2: `Today I will be playing sport.`

Document 3: `I am going to watch the play.`

(i) Describe the preprocessing steps you would use when creating an index for these documents.

(ii) Calculate a vector to represent each document, using the TF-IDF weighting system. You should use the stopword list provided, but do not perform stemming.

(iii) Calculate the cosine similarity for each vector using the query "going to play football", and show the final ranked list of documents for this query.

(iv) What effect on the results would you see if you had used stemming for this corpus?