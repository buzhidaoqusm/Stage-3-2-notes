Based on the belief that good term weighting comes from **three principles**: 
- **Inverse document frequency**: (terms that are rare across a collection should carry more weight). 
- **Term frequency**: (terms that are common within a document should carry more weight). 
- Document length **normalisation**: (so that longer documents do not get an unfair advantage if they contain query terms often simply because of their length).
# TF in BM25
BM25 treats TF slightly differently – a **term** that is **common** in a document will reach **saturation** and should **not get a big advantage** from being even more common.
![](assets/5a%20BM25%20Model/file-20260422174150453.png)
