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

it is difficult to draw reliable conclusions about these documents that appear in multiple result sets