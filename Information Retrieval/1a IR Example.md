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
