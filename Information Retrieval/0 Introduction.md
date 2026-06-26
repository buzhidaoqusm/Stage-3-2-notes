Information Retrieval (IR) deals with the **representation, storage, organization of, and access** to information items.

# Information Item
books, documents or other written material that contain **information in an unstructured form**

# Representation
Searching through the raw text-slow
**mathematical representation(e.g. vectors, sets, bit-strings)** of the information is typically used

# Storage and Organisation
to be quickly accessible

# Access
key attribute allow users to access the information
This should happen in a timely and efficient manner

# information need
#exam Describe what is meant by “information need” in the context of Information Retrieval. What are the different types of information need?
#exam The information need of a user is said to have 4 stages. Describe these stages.

Users only use IR systems when there is **some information that they are interested** in reading

# Information Need: 4 Stages
1. **visceral** need: **unexpressed** need for information
2. **conscious** need: conscious within-brain description of the need(**ambiguous**)
3. **formalised** need: formal statement of the question
	1. Who are the 10 best golfers in the world?
4. **compromised** need: question as presented to the information system

## Query
the **expression** of an information need (**compromised** **need**)

- **Keyword-Based** Querying: **A keyword** (or list of keywords) is supplied to the IR system.
- **Context** Queries: These specify **sequences of words** that should appear close together in documents that are retrieved. Physical proximity has semantic value.
- **Boolean** Queries: The oldest method of combining keywords allows a user to specify keywords that **should or should not be contained** in the documents that are desired
- **Natural Language** Queries: Queries are provided in their natural form.

# Role of an IR
An information retrieval system does **not inform** (i.e. change the knowledge of) the user on the subject of his inquery. 
It merely informs on the **existence** (or non-existence) and **whereabouts** of documents relating to his request.
- Existence: Tells the user that a **document exists** that **helps** to satisfy the information need.
- Whereabouts: Tells the user **where that document is.**

principal function: give users a **list of documents** that are relevant to the user’s information need

It makes **no** attempt to **take a subset of the information** contained **in a document** and present that to the user
## Relevance
A document is ”relevant” if its content **satisfies** (or helps to satisfy) the user’s **information need**.
a **subjective concept**, that is entirely **up to** the **user** to judge

# if the system is good
if the documents it returns **satisfy the information need**

Laboratory experiments use “**test collections**” for which **standard queries** have been created.
**Documents** in these collections have been **judged by human** judges

# Question Answering
the system is designed to **provide a full answer** to a **question**
>Question: What is the capital city of France? 
>Answer: Paris is the capital city of France
![](/assets/0%20Introduction/file-20260420163904433.png)

# Information Extraction
An Information Extraction system is designed to take **unstructured text** and try to **create structured data** from it.

> Input:  “Kate Hudson was born in Los Angeles, California, the daughter of Academy Award-winning actress Goldie Hawn and Bill Hudson, an actor, comedian, and musician. Her parents divorced eighteen months after her birth; she and her brother, actor Oliver Hudson, were raised in Colorado by her mother and her mother’s long-time boyfriend, actor Kurt Russell.” 
> Output (examples): mother(Kate Hudson, Goldie Hawn), brother(Kate Hudson, Oliver Hudson), gender(Kate Hudson, female)

# Note
Question Answering和Information Extraction不是Information Retrieval