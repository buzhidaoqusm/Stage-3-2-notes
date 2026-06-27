# Information Retrieval Exam Knowledge Points 2023-2024

Sources:
- `COMP3009J-2023-Exam-Paper.pdf`
- `COMP3009J-2024-Exam-Paper.pdf`
- Course slides and existing Obsidian notes in `Information Retrieval`

Use this note as an exam-oriented index. Each section is grouped by knowledge point rather than by lecture order.

# 1. Information Need and Modern IR Pipeline

## Course knowledge

Related notes:
- [[0 Introduction#information need]]
- [[0 Introduction#Information Need: 4 Stages]]
- [[0 Introduction#Query]]
- [[7a IR Pipelines and Modern IR#Modern IR]]
- [[7a IR Pipelines and Modern IR#Learning to Rank (LTR)]]

An **information need** is the underlying information that a user wants to find. A query is only the expression of that need, and it may be imperfect because the user has to translate their real need into words accepted by the IR system.

**information need** 是用户真正想获得的信息；**query** 只是这个需求在系统里的表达。考试里要强调：query 可能只是 compromised need，不一定完整表达用户真正的需求。

Taylor's four stages of information need:
1. **Visceral need**: the unexpressed need for information.
2. **Conscious need**: the user's internal, possibly ambiguous, description.
3. **Formalised need**: a formal statement of the question.
4. **Compromised need**: the version presented to the information system.

四阶段记法：先是说不清的需求，再到脑中模糊描述，再到正式问题，最后变成提交给系统的 query。考试如果问 "4 stages"，按这四个英文名写。

A modern IR pipeline often uses multiple stages: Boolean retrieval or candidate generation, simple ranking such as BM25, and machine-learning reranking. Early stages must be fast and high-recall; later stages can use richer features to improve precision at the top of the ranking.

现代 IR pipeline 的核心是 speed-quality trade-off：前面用 Boolean/BM25 快速找 candidate set，尽量别漏掉相关文档；后面用 LTR/ML reranking 结合 TF, IDF, PageRank, document length, user behaviour 等特征优化最终排序。

## Exam example - 2023 Q1.a

**Question:** Describe what is meant by "information need" in the context of Information Retrieval. What are the different types of information need?

**Answer:** An information need is the information that a user is interested in finding. In IR, the system does not directly answer or change the user's knowledge; instead, it tells the user about the existence and location of documents that may satisfy the need. The need may be expressed to the system as different query types: keyword-based queries, context or phrase queries, Boolean queries, and natural language queries.

**解析:** 这题容易把 **information need** 和 **query** 混在一起。information need 是用户真正想知道的内容；query 是 compromised need，是提交给系统的表达形式。types 可以按课件中的 query expression types 写：keyword, context, Boolean, natural language。

## Exam example - 2024 Q1.c

**Question:** The information need of a user is said to have 4 stages. Describe these stages.

**Answer:** The four stages are: visceral need, which is the actual but unexpressed need for information; conscious need, which is the user's internal and possibly ambiguous description of the need; formalised need, which is a formal statement of the question that could be explained to another person; and compromised need, which is the version of the question presented to the IR system according to the inputs it supports.

**解析:** 四个英文名要写准：visceral, conscious, formalised, compromised。最后一个 compromised need 基本就是 query。

## Exam example - 2023 Q1.c / 2024 Q1.a

**Question:** A modern Information Retrieval pipeline may include Boolean searches, simple ranking using BM25, and reranking based on machine learning. Explain why each of these are useful to make an effective Information Retrieval system.

**Answer:** Boolean retrieval is useful for quickly filtering a very large collection and generating a high-recall candidate set. BM25 or another simple ranking model provides an efficient first ranking based on term matching, term frequency, inverse document frequency and document length normalisation. Machine-learning reranking is then applied to the initial ranked list, combining textual and non-textual features such as BM25 score, PageRank, URL depth, document quality and user behaviour to improve precision among the top results.

**解析:** 这题要答出三层各自的作用。Boolean = 快速过滤 / candidate generation；BM25 = scalable initial ranking；ML reranking = 用更多信号提升 top results。重点是如果第一阶段漏掉 relevant document，后面 reranker 永远找不回来，所以 early stage 更重 recall。

# 2. Indexing, Boolean Retrieval and Phrase Queries

## Course knowledge

Related notes:
- [[1b Indexing#Inverted Index]]
- [[1b Indexing#Construction of an Inverted Index]]
- [[1b Indexing#Merging postings lists]]
- [[1b Indexing#Boolean Operators]]
- [[1c Query Optimisation#Query Optimisation]]
- [[1c Query Optimisation#skip pointer]]
- [[3a Phrase Queries]]

An **inverted index** maps each term to a postings list of documents that contain that term. Each posting usually stores a document ID, and postings lists are sorted by docID so that Boolean operations can be implemented efficiently.

**倒排索引**是 term -> postings list。它比 term-document incidence matrix 更适合大语料，因为矩阵非常稀疏，而 postings list 只存出现过的 term-document pairs，空间更小，查询时也更快。

Construction steps:
1. Tokenise each document into `(token, docID)` pairs.
2. Sort by token and then by docID.
3. Merge repeated terms within the same document.
4. Build a dictionary of terms and a postings list for each term.
5. Store document frequency and optionally positions or term frequencies.

构建过程常考：token sequence -> sorting -> dictionary and postings。注意 term 在同一个 document 里通常只在 Boolean postings 中记录一次；如果要 phrase query，需要 positional index 额外存 position。

Boolean operators are set operations:
- `A AND B` narrows the search: intersection `A ∩ B`.
- `A OR B` broadens the search: union `A ∪ B`.
- `A NOT B` excludes documents: difference `A \ B`.

AND 返回更少，OR 返回更多，NOT 排除某类文档。因为 postings list 已排序，AND/OR/AND NOT 都可以用 two-pointer merge，复杂度通常是 `O(x + y)`。

Phrase queries require a **positional index**. A phrase can match only if all terms occur in the required relative positions in the same document.

短语查询不是只看 term 是否出现在同一文档，还要看 position。例如 phrase `"the same day at the same time"` 中两个 `same` 的位置差是 4，因为中间有 `day at the` 三个词。

## Exam example 1 - 2024 Q2.a(i)

**Question:** Explain why an inverted index is more suitable than a term-document incidence matrix.

**Answer:** A term-document incidence matrix has one cell for every term-document pair, so it is extremely sparse for large document collections. An inverted index stores only the documents in which a term actually occurs, making it much more space-efficient. Since postings lists are sorted, Boolean operations can also be processed efficiently by merging postings lists.

**解析:** 关键词：sparse, space-efficient, variable-sized postings lists, sorted docID, efficient merge。

## Exam example 2 - 2024 Q2.a(ii)

**Question:** Describe in detail how a set of postings lists can be created to represent a document corpus. Your answer should include details of the data structures that are used during this process.

**Answer:** First, each document is tokenised into a sequence of `(token, docID)` pairs, usually after normalisation such as lowercasing and punctuation removal. Second, these pairs are sorted alphabetically by token and then by docID. Third, repeated occurrences of the same term in the same document are merged for a Boolean postings list. The final data structure contains a dictionary of terms, and each dictionary entry points to a postings list containing the sorted docIDs of documents containing that term. The dictionary may also store document frequency, and a positional or ranked index may store positions or term frequencies as extra information.

**解析:** 这题要写 data structure：dictionary + postings lists。过程要写 token sequence -> sort -> merge -> split into dictionary and postings。

## Exam example 3 - 2023 Q2.a

**Question:** The Boolean Model makes use of the query operators AND, OR and NOT. Explain how these work and how they affect the number of documents returned by an IR system. Also show how each can be implemented using operations from Set Theory.

**Answer:** `AND` narrows a search by requiring all query terms to appear; it is implemented as set intersection, `A ∩ B`. `OR` broadens a search by returning documents containing any of the terms; it is implemented as set union, `A ∪ B`. `NOT` excludes documents containing a term; it is implemented as set difference, `A \ B`. Adding more `AND` or `NOT` constraints generally reduces the number of returned documents, while using `OR` generally increases it.

**解析:** 答案必须同时包含三件事：operator meaning、对结果数量的影响、set theory notation。

## Exam example 4 - 2023 Q2.b

**Question:** Briefly describe two ways in which the process of running Boolean queries can be optimised so that they can be processed more efficiently.

**Answer:** One optimisation is to process postings lists in increasing order of document frequency. For an `AND` query, starting with the shortest postings list reduces intermediate result sizes and can stop comparisons earlier. For queries involving `OR` groups, the system can estimate the size of each `OR` result and process smaller estimated sets first. A second optimisation is to use skip pointers in long sorted postings lists. If the current docID in one list is far behind the other list, a skip pointer can jump over several postings that cannot match, reducing the number of comparisons.

**解析:** 两个最稳的点：按 document frequency 从小到大处理；skip pointers。说明 skip pointer 依赖 sorted postings list。

## Exam example 5 - 2023 Q1.b

**Question:** The positional index for term `same` is:

```text
1: 120, 124, 167
2: 9, 10, 13
3: 121, 162
4: 4, 101, 105, 106
5: 1, 5, 88, 888, 889
```

Which documents could contain the phrase `"the same day at the same time"`?

**Answer:** The phrase contains two occurrences of `same`, and their positions must differ by 4. The only clear matching candidate is `d1`: positions 120 and 124 differ by 4, so the document could contain `the same day at the same time` with `the` at 119 and `time` at 125.

**解析:** `same` 第一次在 phrase 第 2 个词，第二次在第 6 个词，所以差值是 `6 - 2 = 4`。`d2` 的 9 和 13 差 4，但 position 10 已经是 `same`，而短语这里应该是 `day`，所以排除。`d4` 的 101 和 105 差 4，但 position 106 已经是 `same`，而短语这里应该是 `time`，所以排除。`d5` 的 1 和 5 差 4，但如果第一个 `same` 在 position 1，则前面的 `the` 会在 position 0，题目说 postings 从 1 开始，所以排除。

# 3. Preprocessing: Tokenisation, Stopwords, Stemming and Lemmatisation

## Course knowledge

Related notes:
- [[2a Preprocessing#Tokenisation]]
- [[2a Preprocessing#Problems with Tokenisation]]
- [[2a Preprocessing#Language]]
- [[2b Stopwords, Stemming and Lemmatisation#Stopword Removal]]
- [[2b Stopwords, Stemming and Lemmatisation#Zipf’s Law]]
- [[2b Stopwords, Stemming and Lemmatisation#Stemming]]
- [[2b Stopwords, Stemming and Lemmatisation#Lemmatisation]]

Tokenisation converts raw text into tokens. A **token** is an occurrence of a character sequence; after preprocessing and indexing, it becomes a **term**.

Tokenisation 是把原始文本拆成 token。token 是文本里的实例，term 是进入 index 后的规范化索引项。

Language affects tokenisation. English uses spaces, but other languages create extra issues: Chinese and Japanese do not normally put spaces between words; Arabic is written right-to-left; French contractions such as `L'ensemble` may be split in different ways; German can create long compound words.

非英语 tokenisation 常考三个例子：中文/日文无空格，阿拉伯语从右到左，法语缩写/德语复合词。也可以提日期、hyphen、proper nouns。

A **stopword** is a very common term that appears in many documents and usually contributes little to distinguishing documents. Stopword removal can reduce index size and processing cost, but may make some queries impossible or less accurate.

Stopword removal 会删除 `the`, `and`, `of` 这类高频低区分度词。优点是省空间和计算；缺点是短语、歌名、关系查询可能需要这些词，比如 `"King of Denmark"` 或 `"to be or not to be"`。

Zipf's Law says that if terms are ranked by frequency, the second most frequent term appears about half as often as the first, the third about one third as often, and so on. This explains why a small number of terms are extremely frequent and often become stopwords.

Zipf's Law 和 stopwords 的关系：频率最高的一小部分词占据大量出现次数，但通常区分度低，因此常被当作 stopwords。

**Stemming** maps related words to a common stem, which may not be a real word. **Lemmatisation** maps words to dictionary base forms, so the output is a real word and often requires part-of-speech analysis.

Stemming 是 suffix stripping，快但粗糙，可能 overstemming；lemmatisation 更慢但更准确，能处理 irregular forms，例如 `went -> go`。

## Exam example - 2023 Q1.e / 2024 Q2.b(iii)

**Question:** Compare and contrast stemming and lemmatisation. What are the advantages and disadvantages of each?

**Answer:** Stemming removes suffixes or applies simple rules to map words to a common stem. It is fast, simple and can improve recall by matching variants such as `compute`, `computer` and `computing`. However, stems may not be real words and overstemming can merge unrelated words. Lemmatisation converts words to dictionary lemmas using linguistic analysis, often including part-of-speech information. It is usually more accurate and produces real words, but it is slower and more complex to implement.

**解析:** 对比题要成对写：stemming = fast/simple/less accurate；lemmatisation = real lemma/more effective/slower/requires linguistic knowledge。

## Exam example - 2024 Q1.d

**Question:** When tokenising text, the natural language that the documents are written in can influence the strategy being used. Discuss three examples of issues that can arise when tokenising languages other than English.

**Answer:** Chinese and Japanese do not normally use spaces between words, so the tokeniser must perform word segmentation rather than simply splitting on whitespace. Arabic is written from right to left, which affects text processing and token ordering. French contractions such as `L'ensemble` create ambiguity about whether the text should be treated as one token or split into multiple tokens. German compound words can also be difficult because a long compound may contain several meaningful components.

**解析:** 题目要求 three examples，写三个就够；可以多写一个 German compound word 作为保险。关键是每个例子都要说明为什么影响 tokenisation strategy。

## Exam example - 2024 Q2.b(i)

**Question:** What is meant by stopword removal, and how can this help to improve the Information Retrieval process?

**Answer:** Stopword removal is the preprocessing step of removing very common terms, such as `the`, `and` and `of`, that occur in many documents and usually contribute little to distinguishing one document from another. It can improve IR by reducing index size and reducing the processing cost of handling very long postings lists for high-frequency terms. However, removing stopwords can harm phrase queries and queries where stopwords carry meaning.

**解析:** 优点写 space + processing；缺点可以顺手写，体现 trade-off。

## Exam example - 2024 Q2.b(ii)

**Question:** In what way is Zipf's Law related to stopword removal?

**Answer:** Zipf's Law states that if terms are ranked by frequency, the second most frequent term appears about half as often as the first, the third appears about one third as often, and so on. This means a small number of terms occur extremely frequently in a collection. These high-frequency terms are often poor at distinguishing documents and are therefore candidates for stopword removal.

**解析:** Zipf's Law 不是 stopword removal 的算法，而是解释为什么会存在一小批特别高频、低区分度的词。

# 4. Vector Space Model, TF-IDF and Cosine Similarity

## Course knowledge

Related notes:
- [[3b Vector Space Model]]
- [[3c Cosine Similarity#Cosine Similarity: Why?]]
- [[4a Term Weighting#Term Weights]]
- [[4a Term Weighting#TF-IDF:]]
- [[4a Term Weighting#TF]]
- [[4a Term Weighting#IDF]]

The Vector Space Model represents each document and query as a vector over the vocabulary. Similarity is measured by the angle between vectors, usually using cosine similarity.

VSM 把 document 和 query 都表示成 term-weight vector。角度越小越相似，cosine 越接近 1。

Course TF-IDF formula:

$$
tf_{i,j} = \frac{freq_{i,j}}{maxfreq_j}
$$

$$
idf_i = \log_2\left(\frac{N}{n_i}\right)
$$

$$
w_{i,j}=tf_{i,j}\times idf_i
$$

这里 `maxfreq_j` 是文档 `j` 中最高词频。`N` 是总文档数，`n_i` 是包含 term `i` 的文档数。考试算数要保留三位小数。

Cosine similarity:

$$
cos(d_j,q)=\frac{\vec d_j \cdot \vec q}{|\vec d_j||\vec q|}
$$

如果 query 中有 term 不在 corpus vocabulary 里，通常无法从 index 中匹配，计算时忽略该维度。

## Exam example - 2024 Q2.c(i-ii)

**Question:** Use TF-IDF to represent the documents and calculate cosine similarity for query `"What is her news?"`.

Stopwords: `a, an, is, some, the`

Documents:
1. `Her doctor gave her some very, very bad news.`
2. `A no news day is a good news day.`
3. `An apple a day keeps the doctor away.`

**Answer:**

After stopword removal:

```text
D1: her doctor gave her very very bad news
D2: no news day good news day
D3: apple day keeps doctor away
Query: what her news -> her news (what is not in the corpus vocabulary)
```

Important IDF values:

```text
idf(her)=log2(3/1)=1.585
idf(news)=log2(3/2)=0.585
idf(day)=log2(3/2)=0.585
idf(doctor)=log2(3/2)=0.585
```

Non-zero document weights:

```text
D1: her=1.585, very=1.585, bad=0.792, gave=0.792, doctor=0.292, news=0.292
D2: news=0.585, day=0.585, no=0.792, good=0.792
D3: apple=1.585, away=1.585, keeps=1.585, doctor=0.585, day=0.585
Q:  her=1.585, news=0.585
```

Cosine similarity:

```text
cos(D1,Q)=0.625
cos(D2,Q)=0.145
cos(D3,Q)=0.000
```

Final ranking: `D1 > D2 > D3`.

**解析:** D1 同时匹配 `her` 和 `news`，且 `her` 只出现在 D1，IDF 高；D2 只匹配 `news`；D3 不匹配 query term，所以分数为 0。

## Exam example - 2023 Q2.d(i-iv)

**Question:** Use TF-IDF and cosine similarity for query `"going to play football"` with stopwords `and, be, is, it, to, will`, without stemming.

**Answer:**

After stopword removal:

```text
D1: going rain rain rain today
D2: today i playing sport
D3: i am going watch the play
Query: going play football -> going play (football is not in the corpus)
```

Cosine similarity:

```text
cos(D1,Q)=0.042
cos(D2,Q)=0.000
cos(D3,Q)=0.516
```

Final ranking: `D3 > D1 > D2`.

**解析:** 因为题目要求不要 stemming，所以 `playing` 不等于 `play`，D2 不匹配 query。若使用 stemming，`playing` 和 `play` 会合并，D2 会获得非零相似度，ranking 会更有利于 D2。

# 5. BM25

## Course knowledge

Related notes:
- [[5a BM25 Model#TF in BM25]]
- [[5a BM25 Model#Document length in BM25]]
- [[5a BM25 Model#Document Frequency in BM25]]
- [[5a BM25 Model#Full Formula]]

BM25 is based on three principles: inverse document frequency, term frequency, and document length normalisation.

BM25 三原则必背：rare terms matter more；terms repeated in a document matter more but saturate；long documents need length normalisation。

BM25 uses term-frequency saturation. A term that appears many times should help, but each additional occurrence should help less than the previous one.

BM25 的 TF 不是线性增加，而是会 saturation。这样可以避免一个文档仅靠重复某个词获得不合理高分。

Document length normalisation prevents long documents from receiving unfair advantage simply because they contain more words. Parameter `b` controls the amount of length normalisation, and `k` controls TF saturation.

`b=0` 表示不考虑文档长度，`b=1` 表示完整长度归一化；常见设置是 `k=1, b=0.75`。

## Exam example - 2023 Q1.d

**Question:** The BM25 method is based on the belief that a good term weighting scheme is based on three principles. Briefly describe each.

**Answer:** First, inverse document frequency gives more weight to terms that occur in fewer documents, because rare terms are more useful for distinguishing relevant documents. Second, term frequency gives more weight when a query term occurs more often in a document, but BM25 applies saturation so repeated occurrences have diminishing returns. Third, document length normalisation adjusts scores so that long documents do not receive an unfair advantage simply because they contain more terms.

**解析:** 这题是 6 分短答，按 IDF / TF saturation / document length normalisation 三点分开写就够。

# 6. Evaluation: Cranfield, Precision, Recall, MAP, bpref and NDCG

## Course knowledge

Related notes:
- [[5b Evaluation#The Cranfield Paradigm]]
- [[5b Evaluation#Document sets]]
- [[5c Precision & Recall]]
- [[6a Single Value Metrics#Precision at n]]
- [[6a Single Value Metrics#R-precision]]
- [[6a Single Value Metrics#Mean Average Precision (MAP)]]
- [[6b bpref#bpref: Binary Preference]]
- [[6c Normalised Discounted Cumulated Gain#Normalised DCG]]
- [[6c Normalised Discounted Cumulated Gain#Which metric should I use?]]

The **Cranfield Paradigm** evaluates IR systems using standard test collections: a fixed document corpus, standard queries, and relevance judgments made by human experts.

Cranfield Paradigm 的三件套：corpus, queries, relevance judgments。目标是比较系统返回的 `Ret` 和专家标注的 `Rel`。

Precision and recall:

$$
Precision=\frac{|Ret \cap Rel|}{|Ret|}
$$

$$
Recall=\frac{|Ret \cap Rel|}{|Rel|}
$$

Precision 看返回结果中有多少是 relevant；Recall 看所有 relevant documents 中找回了多少。

**P@n** measures precision in the top `n` results. It is suitable for web search because users often inspect only the first page.

P@10 适合 web search/top results，但缺点是不考虑前 10 之后，也受 relevant documents 总量影响。

**MAP** rewards systems that rank relevant documents early. For one query, Average Precision is the average of precision values at ranks where a relevant document is retrieved, divided by the total number of relevant documents.

MAP 重点：只有遇到 relevant document 的 rank 才计算 precision，然后除以 `|Rel|`。如果有 relevant document 没被返回，也会通过分母惩罚。

**bpref** is designed for incomplete relevance judgments. It ignores unjudged documents and considers how many judged non-relevant documents are ranked above judged relevant documents.

bpref 用于 incomplete judgments。不要把 unjudged documents 当成 non-relevant，这是它和 MAP/Precision 的关键区别。

**NDCG** supports graded relevance. It rewards highly relevant documents and discounts documents that appear later in the ranking.

NDCG 适合 graded relevance，例如 0-3 分相关性。它通过 DCG 折扣后面位置，再除以 ideal DCG 得到 0 到 1 的分数。

## Metric comparison example - 2024 Q3.b(i)

**Question:** Compare and contrast P@10, MAP and NDCG. Suggest situations where each is appropriate.

**Answer:** P@10 measures the proportion of relevant documents in the first ten results. It is simple and appropriate for web search, where users often inspect only the top results, but it ignores all results after rank 10 and does not consider the total number of relevant documents. MAP averages precision at every rank where a relevant document is found, so it rewards systems that rank relevant documents early and is useful when binary relevance judgments are complete. However, it treats all relevant documents equally. NDCG uses graded relevance judgments and discounts lower-ranked documents, so it is appropriate when relevance has different levels, but it requires graded judgments and an ideal ranking for normalisation.

**解析:** 比较题建议按 metric -> advantage -> disadvantage -> best situation 写，避免只背公式。

## Worked example - 2023 Q3.c(i-iii)

**Question:** Retrieved = `d13, d21, d19, d12, d6, d24, d11, d1, d3, d17, d9, d23, d10, d14`; Relevant = `{d2, d3, d7, d9, d12, d15, d17, d23}`. Calculate MAP, Recall and R-Precision.

**Answer:**

Relevant documents retrieved at ranks:

```text
rank 4:  d12 -> P@4  = 1/4  = 0.250
rank 9:  d3  -> P@9  = 2/9  = 0.222
rank 10: d17 -> P@10 = 3/10 = 0.300
rank 11: d9  -> P@11 = 4/11 = 0.364
rank 12: d23 -> P@12 = 5/12 = 0.417
```

```text
AP = (0.250 + 0.222 + 0.300 + 0.364 + 0.417) / 8 = 0.194
Recall = 5 / 8 = 0.625
R-Precision = relevant in top 8 / 8 = 1 / 8 = 0.125
```

**解析:** 只有 5 个 relevant 被 retrieved，但 `|Rel|=8`，所以 AP 分母是 8，不是 5。

## Worked example - 2024 Q3.c(i-iv)

**Question:** Retrieved = `d7 d1 d10 d14 d8 d2 d18 d17 d4 d20 d21 d3 d11 d12 d5 d24`; Judged Relevant = `{d1, d3, d6, d7, d8, d13, d17, d19, d21, d24}`; Judged Non-relevant = `{d9, d10, d11, d12, d18, d25}`. Calculate Precision, Recall, MAP and bpref.

**Answer:**

Relevant documents retrieved at ranks:

```text
rank 1:  d7  -> P@1  = 1.000
rank 2:  d1  -> P@2  = 1.000
rank 5:  d8  -> P@5  = 0.600
rank 8:  d17 -> P@8  = 0.500
rank 11: d21 -> P@11 = 0.455
rank 12: d3  -> P@12 = 0.500
rank 16: d24 -> P@16 = 0.438
```

```text
Precision = 7 / 16 = 0.438
Recall = 7 / 10 = 0.700
MAP = (1.000 + 1.000 + 0.600 + 0.500 + 0.455 + 0.500 + 0.438) / 10 = 0.449
```

For bpref, count judged non-relevant documents above each retrieved relevant document:

```text
d7:  0 -> 1 - 0/10 = 1.000
d1:  0 -> 1.000
d8:  1 -> 0.900
d17: 2 -> 0.800
d21: 2 -> 0.800
d3:  2 -> 0.800
d24: 4 -> 0.600
bpref = (1.000 + 1.000 + 0.900 + 0.800 + 0.800 + 0.800 + 0.600) / 10 = 0.590
```

**解析:** bpref 只看 judged relevant 和 judged non-relevant。`d14, d2, d4, d20, d5` 这类 unjudged documents 不应该当作 non-relevant。

# 7. PageRank

## Course knowledge

Related notes:
- [[11a Page Rank#Document Importance]]
- [[11a Page Rank#PageRank]]
- [[11a Page Rank#Link Structure]]
- [[11a Page Rank#Simplified Version]]
- [[11a Page Rank#rank sink]]
- [[11a Page Rank#Combating Rank Sinks]]

PageRank measures the importance of a web page based on the link structure of the web. A page is important if many pages link to it, or if important pages link to it.

PageRank 的核心直觉：被很多网页链接，或者被高 PageRank 网页链接，就更重要。

Simplified PageRank:

$$
R(u)=\sum_{v \in B_u}\frac{R(v)}{N_v}
$$

With damping factor:

$$
R(u)=(1-d)+d\times \sum_{v \in B_u}\frac{R(v)}{N_v}
$$

`B_u` 是指向 `u` 的 backlinks，`N_v` 是页面 `v` 的 outlinks 数量。`d` 是 damping factor，用来避免 rank sink，并表示不是所有 PageRank 都沿着链接传递。

## Exam example - 2024 Q3.a(i)

**Question:** In PageRank, what is a damping factor and why is it important?

**Answer:** The damping factor controls how much PageRank is passed through outgoing links. In the course formula, only a proportion `d` of the link-based score is passed on, while `(1-d)` is added as a base score. This prevents rank sinks, where a closed group of pages keeps accumulating PageRank without linking outside the group, and also prevents pages with no backlinks from necessarily having zero score.

**解析:** damping factor 不只是公式符号，要解释 rank sink / random jump / base score 的作用。

## Worked PageRank example - 2024 Q3.a(ii)

Assumption from the exam diagram:

```text
d1 -> d3, d5
d2 -> d3
d3 -> d4, d5
d4 -> d2, d3
d5 -> d1, d4
```

Use `d=0.85` and initial PageRank `R(d1)=...=R(d5)=1.000`.

**Answer:**

```text
Iteration 1
d1 = 0.150 + 0.850*(d5/2)              = 0.575
d2 = 0.150 + 0.850*(d4/2)              = 0.575
d3 = 0.150 + 0.850*(d1/2 + d2/1 + d4/2)= 1.850
d4 = 0.150 + 0.850*(d3/2 + d5/2)       = 1.000
d5 = 0.150 + 0.850*(d1/2 + d3/2)       = 1.000

Iteration 2
d1 = 0.575
d2 = 0.575
d3 = 1.308
d4 = 1.361
d5 = 1.181

Iteration 3
d1 = 0.652
d2 = 0.729
d3 = 1.462
d4 = 1.208
d5 = 0.950
```

**解析:** 每轮必须用上一轮的 PageRank 统一更新，不能一边算一边覆盖。考试写三轮即可，不要求收敛。

## Worked PageRank example - 2023 Q3.a

Assumption from the exam diagram:

```text
d1 -> d6
d2 -> d6
d3 -> no outlinks
d4 -> d3, d6
d5 -> d1
d6 -> d5
```

Use course formula `R(u)=(1-d)+d*sum(...)`, `d=0.8`, and initial PageRank `1.000`.

**Answer:**

```text
Iteration 1
d1=1.000, d2=0.200, d3=0.600, d4=0.200, d5=1.000, d6=2.200

Iteration 2
d1=1.000, d2=0.200, d3=0.280, d4=0.200, d5=1.960, d6=1.240

Iteration 3
d1=1.768, d2=0.200, d3=0.280, d4=0.200, d5=1.192, d6=1.240
```

**解析:** `d3` 是 dangling page（没有 outlinks）。这里按课堂简化公式处理，即 dangling page 不向其他页面传递分数；如果考试要求 redistribution，需要先说明假设。

# 8. Relevance Feedback, Rocchio and Query Expansion

## Course knowledge

Related notes:
- [[8b Relevance Feedback#Rocchio Algorithm]]
- [[8b Relevance Feedback#Centroids]]
- [[8b Relevance Feedback#Assumptions]]
- [[8b Relevance Feedback#Pseudo Relevance Feedback]]
- [[8a Query Expansion#Resources for Query Expansion]]
- [[8a Query Expansion#Thesaurus-based Query Expansion]]
- [[8a Query Expansion#Word Embeddings for Query Expansion]]
- [[8a Query Expansion#Search Engine Query Expansion(Query Log Mining)]]

Relevance feedback uses user judgments to improve the representation of the user's information need. The system retrieves results, the user marks some documents as relevant or non-relevant, and the query representation is modified before retrieval is repeated.

Relevance feedback 的流程：初始 query -> 返回结果 -> 用户标 relevant/non-relevant -> 系统修改 query vector -> 重新检索。

Rocchio modifies the query vector by moving it closer to the centroid of relevant documents and away from the centroid of non-relevant documents:

$$
\vec{q}_m = \alpha \vec{q}_0 + \beta \frac{1}{|D_r|}\sum_{\vec d_j \in D_r}\vec d_j - \gamma \frac{1}{|D_{nr}|}\sum_{\vec d_j \in D_{nr}}\vec d_j
$$

Rocchio 的直觉：query vector 靠近 relevant cluster，远离 non-relevant cluster。通常 `gamma < beta`，因为 positive feedback 更有用。

Query expansion adds related terms to the query to reduce vocabulary mismatch and improve recall. Sources include manual thesauri, automatically generated thesauri, word embeddings, query logs and pseudo-relevance feedback.

Query expansion 的主要目标是解决 vocabulary mismatch。常考 three sources of synonyms：manual thesaurus, automatically derived thesaurus, word embeddings。也可以写 query log mining。

## Exam example 1 - 2023 Q4.a

**Question:** The Rocchio Algorithm uses a modified query vector to achieve relevance feedback. Explain why this is effective and how the modified query vector can be calculated.

**Answer:** Rocchio is effective because relevant documents are assumed to be close to each other in vector space and non-relevant documents should be further away. The original query may not be the best vector representation of the information need, so Rocchio moves the query towards the centroid of judged relevant documents and away from the centroid of judged non-relevant documents. The modified query is calculated as:

$$
\vec{q}_m = \alpha \vec{q}_0 + \beta \frac{1}{|D_r|}\sum_{\vec d_j \in D_r}\vec d_j - \gamma \frac{1}{|D_{nr}|}\sum_{\vec d_j \in D_{nr}}\vec d_j
$$

where `q0` is the original query, `Dr` is the judged relevant set, `Dnr` is the judged non-relevant set, and `alpha`, `beta`, `gamma` control the relative influence of the original query and feedback.

**解析:** 公式必须配解释。不要只写公式；要写 why effective：relevant cluster assumption。

## Exam example 2 - 2023 Q4.d / 2024 Q4.d

**Question:** Briefly describe three sources of synonyms for use in query expansion.

**Answer:** First, a manually created thesaurus such as WordNet can provide curated synonyms and near-synonyms. Second, an automatically derived thesaurus can be built by analysing word co-occurrence or grammatical relations in a large corpus. Third, word embeddings can identify semantically similar words by comparing vector representations. Query logs can also provide expansion terms based on previous user behaviour.

**解析:** 三个来源最好各写一句优缺点：manual 高质量但贵；automatic 可扩展但 noisy；embeddings 能捕捉语义相似但可能引入 query drift。

# 9. Fusion: Corpus Overlap, Effects, CombSUM, CombMNZ and Borda

## Course knowledge

Related notes:
- [[9a Fusion Intro#Applications of Fusion]]
- [[9a Fusion Intro#Corpus Overlap]]
- [[9a Fusion Intro#Three Fusion “Effects”]]
- [[9b Fusion Rank-Based#Borda-Fuse]]
- [[10a Fusion Score Based#CombSUM]]
- [[10a Fusion Score Based#Score Normalisation]]
- [[10a Fusion Score Based#CombMNZ]]

Fusion combines the ranked outputs of multiple IR systems into one final ranked result set. It can be used for metasearch, distributed IR, internal metasearch and data fusion.

Fusion 是把多个系统/算法的 ranked results 合并成一个结果。考试经常把它和 corpus overlap 以及 three effects 放在一起问。

Corpus overlap matters:
- **Disjoint databases**: no document appears in more than one corpus; chorus effect cannot be used.
- **Identical databases**: same document collection; appearing in multiple result sets is strong evidence of relevance.
- **Partially overlapping databases**: difficult to know whether absence means non-relevance or simply that the system did not contain the document.

Corpus overlap 决定 fusion algorithm 能不能可靠使用 chorus effect。Data fusion 通常是 identical databases；collection fusion 通常是 disjoint databases。

Three fusion effects:
- **Skimming Effect**: top-ranked documents from each system are likely to be useful.
- **Chorus Effect**: documents returned by multiple systems are more likely to be relevant.
- **Dark Horse Effect**: one system may be unusually good or bad for a particular query.

Skimming 几乎所有 fusion 都用；Chorus 在 data fusion 中很重要；Dark Horse 难识别，所以一般不常直接用。

Score-based fusion should normalise scores first if different systems use different score ranges:

$$
normalised = \frac{score - min}{max - min}
$$

CombSUM:

$$
CombSUM(d)=\sum_i score_i(d)
$$

CombMNZ:

$$
CombMNZ(d)=CombSUM(d)\times \text{number of systems returning }d
$$

Borda Fuse is rank-based. If there are `c` unique candidate documents, the top-ranked document receives `c` points, the second receives `c-1`, and so on. If a document is unranked by a system, the remaining points are divided evenly among unranked documents.

Borda 不看原始 score，只看 rank；CombSUM/CombMNZ 看 score，所以不同系统 score range 不同时必须先 normalise。

## Worked fusion example - 2024 Q4.b(i-iii)

**Question:** Calculate the score for `d16` using CombMNZ, `d18` using Borda Fuse, and `d10` using CombSum.

**Answer:**

After min-max normalisation per engine:

```text
d16:
Engine A = 0.403
Engine B = 0.250
Engine C = 0.000 (not returned)
Engine D = 0.150
CombMNZ(d16) = (0.403 + 0.250 + 0 + 0.150) * 3 = 2.406
```

```text
d10:
Engine A = 0.718
Engine B = 0.860
Engine C = 1.000
Engine D = 0.383
CombSUM(d10) = 2.961
```

For Borda Fuse, there are `22` unique documents. `d18` gets:

```text
Engine A: rank 6 -> 22 - 6 + 1 = 17
Engine B: rank 3 -> 20
Engine C: unranked. C returns 7 documents, so 15 are unranked: (15+1)/2 = 8
Engine D: rank 10 -> 13
Borda(d18)=17+20+8+13=58
```

**解析:** CombMNZ 的 multiplier 是返回该文档的系统数，不是所有系统数。Borda 的 `c` 是所有系统结果中的 unique documents。

## Worked fusion example - 2023 Q4.b(i-iii)

**Question:** Calculate `D6` using CombSUM, `D10` using CombMNZ, and `D5` using Borda Fuse.

**Answer:**

```text
D6 normalised scores:
Engine A = 0.171
Engine B = 0.231
Engine C = 0.546
CombSUM(D6)=0.948
```

```text
D10 normalised scores:
Engine A = 1.000
Engine B = 0.000
Engine C = 0.000 (not returned)
CombMNZ(D10)=(1.000+0.000+0.000)*1=1.000
```

For Borda Fuse, there are `11` unique documents:

```text
D5 in Engine A: unranked, A has 3 unranked docs -> (3+1)/2 = 2
D5 in Engine B: rank 1 -> 11
D5 in Engine C: rank 7 -> 5
Borda(D5)=2+11+5=18
```

**解析:** Engine B 里 `D10` 是最低分，因此 min-max normalisation 后是 0。CombMNZ 只乘 non-zero scores 的系统数；如果采用 "returned systems count" 的变体，需要在答案里说明，但课件定义是 Multiply by Non Zero。

# 10. Web Crawling and Adversarial IR

## Course knowledge

Related notes:
- [[12a Web Crawling#Polite Crawlers]]
- [[12a Web Crawling#The Robots Exclusion Standard]]
- [[12a Web Crawling#robots.txt]]
- [[12a Web Crawling#noindex and nofollow]]
- [[12a Web Crawling#Challenge]]
- [[12b Adversarial Information Retrieval#Adversarial IR]]
- [[12b Adversarial Information Retrieval#Hidden Text]]
- [[12b Adversarial Information Retrieval#Cloaking]]
- [[12b Adversarial Information Retrieval#Exploiting PageRank]]

A web crawler automatically finds pages to include in an IR index by downloading pages, extracting hyperlinks, and adding new URLs to a queue. A breadth-first crawling strategy starts from seed URLs and repeatedly visits queued links.

Web crawler 的基本流程：seed URLs -> queue -> download page -> extract links -> add unseen URLs -> repeat。实际系统会并行运行。

A polite crawler should obey `robots.txt`, respect crawl-delay, avoid overloading servers, avoid excessive bandwidth use, and identify itself with a user-agent string.

Polite crawler 必写 robots.txt、crawl-delay、不压垮网站、user-agent。`robots.txt` 是 voluntary standard，不是强制访问控制。

Challenges include duplicate URLs for the same page, dynamic pages, constantly changing content, deciding revisit policies, JavaScript-generated content and CAPTCHA.

爬虫难点：同一页面多个 URL 会影响去重和 PageRank；动态内容会变；CAPTCHA 和 JS 页面可能难以抓取。

Adversarial IR happens when content providers deliberately try to manipulate search engine results against the goals or guidelines of the IR system.

Adversarial IR 常见于 black-hat SEO：网页作者试图欺骗搜索引擎，让页面在不该高排的时候高排。

Examples include keyword stuffing in meta tags, hidden text, cloaking, sneaky JavaScript, link farms, comment spam and doorway pages.

考试问两个例子时，选最容易解释的两个：hidden text / cloaking / link farms。

## Exam example 1 - 2024 Q4.a

**Question:** Describe how you would design a web crawler to find information for an IR index. Include standards it should follow and situations where accessing information is difficult.

**Answer:** I would start with a queue of seed URLs and use a breadth-first crawling strategy. For each URL, the crawler downloads the page, extracts hyperlinks, normalises and deduplicates URLs, and adds unseen URLs to the queue. It should store downloaded content for indexing and schedule revisits for pages likely to change. The crawler should be polite: obey `robots.txt`, respect crawl-delay, avoid overloading a server, limit bandwidth use and identify itself using a user-agent string. Difficult situations include duplicate URLs for the same page, dynamic content, JavaScript-generated links, CAPTCHA, pages requiring login and constantly changing pages.

**解析:** 这题是设计题，按 workflow + politeness + challenges 三段答。

## Exam example 2 - 2024 Q1.e

**Question:** Explain what is meant by Adversarial Information Retrieval. In web search, show two examples of where this can occur.

**Answer:** Adversarial Information Retrieval occurs when some participants try to manipulate the retrieval system so that results are ranked in a way that benefits them but does not reflect true relevance. In web search, one example is hidden text or keyword stuffing, where extra query terms are added to a page, sometimes in text invisible to users, to increase term frequency. Another example is cloaking, where a server detects crawlers and serves them different, keyword-targeted content from what normal users see. Link farms and comment spam are also examples because they attempt to manipulate PageRank.

**解析:** 定义要写 "conflicting goals / manipulation"。例子要说明它怎么影响 ranking，不要只列名词。

# Quick Exam Checklist

- Information need: definition, 4 stages, query types.
- Modern pipeline: Boolean candidate generation, BM25 initial ranking, ML reranking.
- Inverted index: construction, postings list, Boolean set operations, phrase positional query.
- Preprocessing: tokenisation issues, stopwords, Zipf's Law, stemming vs lemmatisation.
- VSM: `tf=freq/maxfreq`, `idf=log2(N/ni)`, cosine similarity.
- BM25: IDF, TF saturation, document length normalisation.
- Evaluation: Cranfield, Precision, Recall, P@10, R-Precision, MAP, bpref, NDCG.
- PageRank: backlinks, outlinks, damping factor, iterative calculation.
- Feedback/QE: Rocchio, probabilistic feedback idea, synonym sources.
- Fusion: corpus overlap, skimming/chorus/dark horse, CombSUM, CombMNZ, Borda.
- Web: crawler design, robots.txt, crawl-delay, duplicate/dynamic pages, adversarial IR.
