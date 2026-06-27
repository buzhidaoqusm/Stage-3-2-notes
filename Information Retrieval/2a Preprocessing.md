# Preprocessing: Tokenization

## Learning Objectives
- Explain what a token is and how tokenization works
- Describe different methods of tokenization
- Apply normalization to your tokens

## Tokenisation

> **Input:** "Friends, Romans and Countrymen"
> **Output:** Tokens: Friends, Romans, Countrymen

- A **token** is an **instance** of a sequence of characters
- Each token is a **candidate** for storing as an **index entry**, after further processing (preprocessing)
- When we store a token in an index, we call it a **term**

## Problems with Tokenisation
#exam When tokenising text, the natural language that the documents are written in can influence the strategy being used. Discuss three examples of issues that can arise when tokenising languages other than English.
- Finland’s capital → Finland AND s? Finlands? Finland’s?
- Mercedes-Benz → Mercedes and Benz as two tokens?
- San Francisco: one token or two?
- Numbers
	- 3/20/91 Mar. 12, 1991 20/3/91
## Language
- French 
	- L'ensemble → one token or two?
- German(not segmented)
	- Lebensversicherungsgesellschaftsangestellter
- Chinese and Japanese have **no spaces between words**
- Arabic
	- written right to left
# Normalisation
common approaches:
- Changing all tokens to lowercase
- Delete full stops to form terms: USA, U.S.A. → usa
- Delete hyphens: anti-discrimininatory, antidiscriminatory → antidiscriminatory
# Thesauri and soundex
- hand-constructed equivalence classes
	- car = automobile color = colour
- rewrite to form equivalence-class terms
	- When the document contains automobile, index it under car-automobile
- expand a query
	- When the query contains **automobile**, look under **car** as well
