Basic definition: 
**Combining** the **outputs** of **multiple** different Information Retrieval systems/algorithms into a **single ranked result set** that can be shown to a user in response to a **query**

query给多个IR system，每个IR system会返回Documents，将所有Documents加权到一起返回给user作为结果
# Applications of Fusion
-  **Metasearch**: This involves fusion of result sets **returned by** autonomous, complete **search engines** (e.g. Google, Bing)
- **Distributed Information Retrieval**: Numerous IR systems are designed to **co-operate with one another**, each working on a **subset** of the document collection.
- **Internal Metasearch**: Numerous algorithms perform searches on the **same document collection** (data fusion)
# Corpus Overlap
## Disjoint Databases
完全不同
input systems search **separate, disjoint document** collections that have **no documents in common**
A document **cannot** be **returned** by **more than one input** system
**Distributed IR** is typically implemented using **disjoint databases**. 
Fusion of **disjoint corpora** is frequently known as `Collection Fusion`.
## Identical Databases
完全相同
same set of documents
Appearing in **multiple result sets** is frequently **interpreted** as further evidence of **relevance**
This has become known as `Data Fusion` and is our **main focus**.
**Internal Metasearch** is generally a Data Fusion task.
## Overlapping Databases
部分相同

it is **difficult to draw reliable conclusions** about these **documents** that appear in **multiple** result sets
当一个Document出现在一个result但不在另一个result中时，可能某个系统认为它是不相关也有可能它没有该文档，所以很难判断。
External Metasearch

# Three Fusion “Effects”
## The Skimming Effect
top documents = 相关的文档
The Skimming Effect argues that "**skimming**" the **top documents** from each result set and using these for **fusion** should give better performance.

**used for all** popular fusion algorithms
## The Chorus Effect
If **multiple input systems** agree that a **document is relevant**, the Chorus Effect argues that this evidence of relevance should be taken into account and **that document** should be **highly ranked** in the fused result set.

**Whether** this effect is **applicable** **depends** on the level of **overlap** between the corpora
- Data Fusion: the Chorus Effect tends to be an **important consideration**.
- Collection Fusion: not a factor at all
- Partially overlapping: Difficult to gauge
## The Dark Horse Effect
The Dark Horse Effect is where **one** input **system** returns **results** of a **much different** quality than the others.
either unusually accurate or unusually inaccurate

This seems to **contradict the Chorus Effect**, as it would favour identifying a "dark horse" and just using its results, **rather than fusing** it with others.

difficult to identify
generally **not used** in fusion algorithms
