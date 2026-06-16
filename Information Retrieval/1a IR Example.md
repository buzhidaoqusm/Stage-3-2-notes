# Aim
using the **Boolean Model**(if contain particular words)
explore **why representation is important**
# Boolean Model
The Boolean **AND/OR/NOT** operators are used to indicate combinations of words that **if appear in the documents** the user is searching for
> (cat OR dog OR pet) AND hospital

output is **not ranked**

## Example

>contain the words **Brutus AND Caesar** but **NOT Calpurnia**

### Attempt 1
find those that contain Brutus and Caesar and then remove those that contain Calpurnia

Slow

A **corpus** is a **collection of documents**. The **plural is corpora**.

### Term-document incidence matrix
This is an example of a representation of a text document.
一些戏剧中包含的一些单词

![](/assets/1a%20IR%20Example/file-20260420174636022.png)
#### Incidence vectors
>Brutus: 110100

Our query is: **Brutus AND Caesar NOT Calpurnia**

NOT Calpurnia:
**complement** of the incidence vector
001000 → 110111

结果: bitwise AND
110100 AND 110111 AND 101111 = 100100

### Bigger collections
矩阵太大
We only record the 1 positions
