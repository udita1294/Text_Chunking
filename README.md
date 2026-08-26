# Chunking

A simple Python project demonstrating different **text splitting and chunking strategies** using LangChain.

Text splitting is an important step in **RAG (Retrieval-Augmented Generation)** applications because large documents need to be divided into smaller chunks before they can be embedded, stored, and retrieved efficiently.

This project demonstrates three approaches:

1. Fixed-size / character-based chunking
2. Paragraph-based chunking
3. Recursive character-based chunking

---

##  Project Overview

Large documents cannot always be directly passed to an LLM or embedding model. Therefore, the document is divided into smaller pieces called **chunks**.

For example:

```text
Large Document
      ↓
Text Splitting
      ↓
 ┌────────┬────────┬────────┐
 │ Chunk 1│ Chunk 2│ Chunk 3│
 └────────┴────────┴────────┘
      ↓
Embeddings / Retrieval / LLM
```

The goal of this project is to understand how different splitting strategies affect the resulting chunks.

---

##  Technologies Used

* Python
* LangChain
* `langchain-text-splitters`

---

##  Project Structure

```text
langchain-text-splitting/
│
├── main.py
├── README.md
└── requirements.txt
```

---


#  1. Fixed-Size / Character Chunking

The first approach uses `CharacterTextSplitter`.

```python
fixed = CharacterTextSplitter(
    separator="",
    chunk_size=100,
    chunk_overlap=0
)
```

Here:

* `separator=""` means the text is split without relying on a paragraph separator.
* `chunk_size=100` sets the target chunk size.
* `chunk_overlap=0` means there is no overlap between chunks.

This approach is useful for understanding basic character-based splitting.

---

#  2. Paragraph-Based Chunking

The second approach splits the document using paragraphs.

```python
paragraph = CharacterTextSplitter(
    separator="\n\n",
    chunk_size=100,
    chunk_overlap=0
)
```

The separator:

```text
\n\n
```

represents a blank line between paragraphs.

This allows the splitter to use paragraph boundaries when creating chunks.

LangChain's `CharacterTextSplitter` supports specifying a separator and measures chunk length using characters by default.

---

#  3. Recursive Character Splitting

The third approach uses:

```python
RecursiveCharacterTextSplitter
```

Example:

```python
recursive = RecursiveCharacterTextSplitter(
    chunk_size=100,
    chunk_overlap=20
)
```

Unlike simple character splitting, the recursive splitter attempts to preserve larger pieces of text before breaking them down further.

Its default separator hierarchy is approximately:

```text
Paragraph
    ↓
Line
    ↓
Word
    ↓
Character
```

This makes it better at preserving context within chunks.

LangChain recommends `RecursiveCharacterTextSplitter` as a general-purpose starting point because it attempts to keep semantically related text together while respecting the chunk size.

---

##  Chunk Overlap

The recursive splitter uses:

```python
chunk_overlap=20
```

This means consecutive chunks can share approximately 20 characters.

For example:

```text
Chunk 1:
The detective entered the abandoned house...

Chunk 2:
house just before midnight. The rooms were...
```

The overlapping content helps reduce the chance of losing important context when information falls near a chunk boundary.

---

##  Comparison

| Method              | Main Idea                        | Context Preservation | Complexity |
| ------------------- | -------------------------------- | -------------------- | ---------- |
| Character Splitting | Split using characters/separator | Low                  | Easy       |
| Paragraph Splitting | Split around paragraphs          | Medium               | Easy       |
| Recursive Splitting | Try multiple separators          | High                 | Medium     |

---

##  Why Text Splitting Matters in RAG

Text splitting is commonly used before creating embeddings in RAG systems.

A typical RAG pipeline looks like:

```text
Documents
    ↓
Text Splitting
    ↓
Chunks
    ↓
Embeddings
    ↓
Vector Database
    ↓
User Query
    ↓
Similarity Search
    ↓
Relevant Chunks
    ↓
LLM
    ↓
Answer
```

Good chunking can improve retrieval because the retrieved chunks are more likely to contain the relevant context.

---

##  Learning Objectives

By completing this project, you will understand:

* What text chunking is
* Why documents need to be split
* How `CharacterTextSplitter` works
* How paragraph-based splitting works
* How `RecursiveCharacterTextSplitter` works
* What `chunk_size` means
* What `chunk_overlap` means
* Why recursive splitting is useful in RAG
* How text splitting fits into a RAG pipeline

---

##  Useful LangChain Documentation

* [LangChain Text Splitters](https://docs.langchain.com/oss/python/integrations/splitters)
* [Character Text Splitter](https://docs.langchain.com/oss/python/integrations/splitters/character_text_splitter)
* [Recursive Character Text Splitter](https://docs.langchain.com/oss/python/integrations/splitters/recursive_text_splitter)

---
