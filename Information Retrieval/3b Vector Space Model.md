
# Boolean Model Disadvantages
The model predicts that each document is **either relevant or non-relevant** to the query.
There is **no** notion of a “**partial**” match to the query conditions
**no ranking** occurs.

# Vector Space Model
This **models** both **documents** and **queries** in an N-dimensional space.
Basic **vector algebra** is used to calculate the most **similar** documents to a query.

- A collection contains a set of **documents** each of which is **composed of** a “bag of words” (also known as **terms**).
- **Each unique term** in the collection is represented by **a dimension** in the vector space
- A **document** is then **represented** by giving a positive non-zero **value** to the **dimension** representing a term it contains.
	- This number is known as a **term weight**.
- Other dimensions get a value of **zero**. （not contain）
- A **similarity score** is calculated based on the **proximity of two vectors**
## Example
假设我们有 4 个文档，词典为 {cat, dog, fish, bird}：

|           | cat | dog | fish | bird |
| --------- | --- | --- | ---- | ---- |
| **Doc1**  | 2   | 1   | 0    | 0    |
| **Doc2**  | 0   | 1   | 3    | 0    |
| **Doc3**  | 1   | 0   | 0    | 2    |
| **Query** | 1   | 0   | 0    | 0    |

- **权重值** = 该 term 在文档中出现的**次数**（或 TF-IDF 等更复杂的值）
- **0** 表示该 term 不在文档中

**向量表示：**
- Doc1 = (2, 1, 0, 0) — “cat” 出现2次，”dog” 出现1次
- Doc3 = (1, 0, 0, 2) — “cat” 出现1次，”bird” 出现2次
- Query = (1, 0, 0, 0) — 找关于猫的文档

→ Doc1 和 Doc3 都与 Query 相关（都包含 “cat”），但 Doc1 的权重更高（2 vs 1）