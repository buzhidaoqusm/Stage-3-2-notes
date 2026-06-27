# Stopword Removal
A **stopword** is a commonly occurring term that appears in so many documents that it does **not add to the meaning** of the document
like `the, and, of`
没意义的词

of **little use** when **differentiating** between documents
The fact that they are **so common** also means that **more processing power is required** to deal with these terms than others that are less common.
处理时所需的算力高

## Zipf’s Law
If we **rank** terms from the most commonly used to the least commonly-used in a large text collection, the **second term** is approximately **½** as common as the **first**, the **third** is approximately **⅓** as common as the first, etc.

## Tradeoff
- away from removing them
	- Good compression means that space required is small. 
	- Good optimisation means processing them takes little time.
- **Some** queries will become **impossible**
	- Phrases: “King of Denmark” 
	- Song titles, etc. “Let it be”, “To be or not to be” 
	- Relational queries: “Flights to London”
- **solutions**
	- **Don’t remove stopwords**. Use all words as terms
	- **Recognise** when **combinations** of stopwords are **meaningful**, and include these as terms in the index.
	- 将停用词的组合作为索引项
# Stemming
#exam Compare and contrast the preprocessing steps of stemming and lemmatisation. In particular, what are the advantages and disadvantages of each?

将一些词转为共同的词干（不是真实的词）
- **Stemming** is the process that maps these **terms** to a **common root**. 
	- e.g. “computing”, “computer” → “comput”
	- Note: a stem does not need to be a real word.
- The **end** of a term is called a **suffix** (-er, -s, -ing, -ed, etc.)
- Hence stemming is also known as **suffix stripping**.

## Porter’s Stemming Algorithm
the most famous
## Example
- accept: FAI decides to accept rescue deal. 
- acceptable: FAI deems rescue deal acceptable. 
- acceptance: Owens pleased after rescue deal acceptance. 
- accepted: The FAI has accepted the government rescue deal. 
- accepts: FAI accepts rescue deal.

The **stem** for all of these is “**accept**”
## 优点
- **Fast and simple** (rule-based, no deep analysis needed)
- **Improves recall** by matching word variants  
    e.g. computing / computer → comput
- **Widely used** in IR systems (e.g. Porter stemmer)
## Problems
- **overstemming**
	- **suffixes** can be **removed** from words that are **not related**, but that end up being the same afterwards
	- **Example**: 
		- consider the words “general” and “generate”
		- Both stem to "**gener**", even though they are **not related**.
- **homographs**(含义不同但拼写相同)
	- bank: 银行/河岸
	- often needs to see the word in context
# Lemmatisation
将单词变成词元（是真实的词）
converting **words** into **lemmas** (i.e. **base words** found in **dictionaries**)
- Unlike a stem, a lemma is always a **real word**.
**slower** than stemming, but is usually more **effective**
a lemmatiser will often need to know **the part of speech** that the word is being used as (verb, noun, etc.)
**requires** **more text analysis** than stemming
![](/assets/2b%20Stopwords,%20Stemming%20and%20Lemmatisation/file-20260421213211943.png)
## 优点
real word
more **effective**
能处理复杂词形变化（irregular forms）e.g. go / went / gone → go
适合nlp任务

## 缺点
computationally slower
requires **more text analysis**
实现复杂，requires linguistic knowledge