# Preprocessing: Tokenization

## Learning Objectives
- Explain what a token is and how tokenization works
- Describe different methods of tokenization
- Apply normalization to your tokens

## Tokenisation

**Input:** "Friends, Romans and Countrymen"

**Output:** Tokens: Friends, Romans, Countrymen

- A **token** is an **instance** of a sequence of characters
- Each token is a **candidate** for storing as an **index entry**, after further processing (preprocessing)
- When we store a token in an index, we call it a **term**

## Problems with Tokenisation

- Hyphens, apostrophes, word boundaries cause problems
- Examples:
  - "state-of-the-art"
  - "don't"
  - "U.S.A."
  - "横切"

> A token is delimited by **whitespace**, **punctuation**, and **other special characters**

## Goal of Tokenization

Create an index that corresponds to the **vocabulary** the user is likely to use when querying.

**Example:**
> "Are you, by any chance, having one of those days in which everything is status-of-the-art and you aren't?"

**Tokens:** are, you, by, any, chance, having, one, of, those, days, in, which, everything, is, status, of, the, art, and, you, aren, t

## Tokenization in Practice

Don't create tokens that are:
- Too long
- Too short
- Contain just digits

| Good Tokens | Bad Tokens |
|-------------|------------|
| "1974" | "1,000,000" |
| "MK15" | "a1234567890b" |
| "MK5a" | |

## Case Folding

Reduce all letters to **lowercase**.

**Example:** "Porcupine" → "porcupine"

> However, "US" vs "us" — need to be careful with **acronyms**!

## Stop Words

**Stop words** are words that are filtered out before or after processing.

### Common Stop Words
a, about, above, after, again, against, all, am, an, and, any, are, as, at, be, because, been, before, being, below, between, both, but, by

### Characteristics
- Very **common words**
- Have **little meaning** on their own
- Appear in **almost every document**
- Provide little **discriminatory power**

### Problems with Stop Words
- **Phrase search** becomes impossible:
  - "to be or not to be"
  - "King of Denmark"
- Often crucial to the **meaning** of the query

## Lemmatization

Reduce words to their **dictionary form** (base form) using morphological analysis.

**Examples:**
| Word | Lemma |
|------|-------|
| am, are, is | be |
| car, car's, cars', cars | car |

## Stemming

Reduce words to their **root form** using **heuristic rules**.

**Example:** automate, automatic, automation → automat

> Stemming is **cruder** than lemmatization and doesn't handle vocabulary variations well.

## Morphological Analysis

The **study of word forms**:

### Inflectional Changes
- boy → boys
- walk → walked

### Derivational Changes
- regular → regulate
- national → nation

### Morphological Lexicon
Maps words to their **base forms**.

## Summary

| Technique | Description |
|-----------|-------------|
| Tokenization | Split text into tokens |
| Case Folding | Convert to lowercase |
| Stop Words | Filter common words |
| Lemmatization | Dictionary form (morphological analysis) |
| Stemming | Root form (heuristic rules) |
