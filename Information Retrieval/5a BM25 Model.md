#exam The BM25 method of Information Retrieval is based on the belief that a good term weighting scheme is based on three principles. Briefly describe each of these principles.

Based on the belief that good term weighting comes from **three principles**: 
- **Inverse document frequency**: (terms that are rare across a collection should carry more weight). 
- **Term frequency**: (terms that are common within a document should carry more weight). 
- Document length **normalisation**: (so that longer documents do not get an unfair advantage if they contain query terms often simply because of their length).
# TF in BM25
BM25 treats TF slightly differently – a **term** that is **common** in a document will reach **saturation** and should **not get a big advantage** from being even more common.
![](/assets/5a%20BM25%20Model/file-20260422174150453.png)
𝑓i,j is the frequency of **term i** in **document j** and k is some constant value
This version of TF **rises quickly** for **low** frequencies, but more **slowly** for **high** frequencies after saturation occurs.

It has another **nice feature** – it **rewards complete matches** over partial ones.
¤ 假设我们搜索“cat"和"dog"
¤ 如果一篇文档同时包含“cat”和“dog”一次，则每个文档的排名分数都会增加1/2。 
¤ 如果另一个文档两次包含“cat”但不包含“dog”，则“cat”的排名分数将增加2/3，而“dog”的排名分数将增加0。
1/2+1/2 > 2/3
**完全匹配**查询的文档将**优先于**仅**部分匹配**但查询词出现频率相同的文档

# Document length in BM25
It is also important to **normalise for document lengths**
¤ 如果一个非常短的文档提到“大象”，那么它可能是关于大象的。 
¤ 如果一份很长的文件只提到“大象”一次，那么它可能不是关于大象的。

BM25 achieves this by **adjusting k** depending on the **length of the document**.
¤ 对于超过平均长度的文档，k 将增加。对于低于平均长度的文档，k 将减少。

not every document collection is the same
- tunable parameter called b
	- b=0: document length is not considered
	- b=1: full document length adjustment from the **previous slide**
	- Any value 0 ≤ b ≤ 1 can be chosen
# Document Frequency in BM25
## IDF
![](/assets/5a%20BM25%20Model/file-20260422181458760.png)
where N is the total **number of documents** in the corpus, and 𝑛i is the number of documents **containing term** i.

Effect
- It results in a **negative weight** for documents that **appear in > 50%** of the documents in the corpus.
- 2 options:
	- Treat all these terms as **stopwords** and do **not index** them
	- ![](/assets/5a%20BM25%20Model/file-20260422183911363.png)
# Full Formula

$$
sim_{BM25}(d_j, q)\sim \sum_{k_i \in d_j \land k_i \in q} \frac{f_{i,j}\times(1+k)}{f_{i,j}+k\left((1-b)+\frac{b\times len(d_j)}{avg\_doclen}\right)} \times \log\left(\frac{N-n_i+0.5}{n_i+0.5}\right)
$$
- ki: term
- 𝑓ij is the frequency of ki in dj
- k and b are constants that can be set to **suit the document collection** and the desired behaviour
	- k=1, b=0.75
- k controls how much the **overall TF increases** for terms that are common within the document.
# Example
Query: “President Lincoln” 
N = 500,000 documents 
“president” occurs in 40,000 documents (𝑛i = 40,000) 
“lincoln” occurs in 300 documents (𝑛i = 300) 
“president” occurs 15 times in this document (𝑓i,j = 15) 
“lincoln” occurs 25 times in this document (𝑓i,j = 25) 
The document is 90% of the length of the average
𝑘 = 1, 𝑏 = 0.75

![](/assets/5a%20BM25%20Model/file-20260422194856490.png)
# Variations
BM25F: Allows **different fields** to be given **different importance** in the document (e.g. document **title**, **headlines**, main text, etc.). 
BM25+: addresses an issue with BM25 whereby **very short documents** would be given scores that are **too high**.