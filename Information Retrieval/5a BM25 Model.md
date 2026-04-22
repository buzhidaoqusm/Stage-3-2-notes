Based on the belief that good term weighting comes from **three principles**: 
- **Inverse document frequency**: (terms that are rare across a collection should carry more weight). 
- **Term frequency**: (terms that are common within a document should carry more weight). 
- Document length **normalisation**: (so that longer documents do not get an unfair advantage if they contain query terms often simply because of their length).
# TF in BM25
BM25 treats TF slightly differently – a **term** that is **common** in a document will reach **saturation** and should **not get a big advantage** from being even more common.
![](assets/5a%20BM25%20Model/file-20260422174150453.png)
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
