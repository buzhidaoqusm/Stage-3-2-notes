The concept of phrase queries has proven **easily understood** by users
one of the few “**advanced search**” ideas that **works**
# Biword indexes
- Index every **consecutive pair** of terms in the text as a phrase
	- example “Friends, Romans, Countrymen”:
		- riends romans
		- romans countrymen
- Each of these **biwords** is now a **dictionary term**
- **Two-word phrase** query-processing is now **immediate**
## Longer phrase queries
- “stanford university palo alto” can be broken into the Boolean query on biwords:
	- stanford university AND university palo AND palo alto

**Without the documents** themselves, we **cannot verify** that the docs matching the above Boolean query do contain the exact phrase

**false positives**
## Issues
- False positives
- Index blowup due to bigger dictionary
- ¤Biword indexes are not the standard solution (for all biwords) but **can be** part of a **compound strategy**.
# Positional indexes
```
<term, number of docs containing term;
doc1: position1, position2 … ;
doc2: position1, position2 … ;
etc.>
```


For phrase queries, we use a **merge** algorithm recursively at the **document level**.
But we now need to deal with more than just equality: we **look for terms** that **follow each other**.
## Positional index size
positional index **expands** postings **storage** substantially
positional index is now **standardly used** because of the power and usefulness of phrase queries.

## Rules of thumb
