# Day 38 – Tuesday: Word2Vec & Polysemy Exploration

## Overview

This script explores **Word2Vec embeddings** using the `gensim` library, focusing on how a single word can carry multiple meanings (polysemy) and how context-based disambiguation can be implemented using cosine similarity.

---

## Dataset

| Field | Details |
|---|---|
| File | `preprocessed_reviews.csv` |
| Column Used | `review_text` |
| Format | Tokenized sentences (list of word lists) |

---

## Dependencies

```bash
pip install gensim pandas numpy
```

| Library | Purpose |
|---|---|
| `pandas` | Loading and handling the CSV dataset |
| `gensim` | Training the Word2Vec model |
| `numpy` | Cosine similarity computation |

---

## Word2Vec Model Configuration

```python
Word2Vec(
    sentences=sentences,
    vector_size=100,
    window=5,
    min_count=2,
    workers=4
)
```

| Parameter | Value | Description |
|---|---|---|
| `vector_size` | 100 | Dimensionality of word embeddings |
| `window` | 5 | Context window size |
| `min_count` | 2 | Ignores words with frequency below this |
| `workers` | 4 | Parallel threads for training |

---

## Key Concepts Covered

### (a) Polysemy — One Word, One Vector

Word2Vec assigns a **single embedding** to each word, regardless of its context-dependent meaning.

The word `"cheap"` is used as a case study — it can mean:
- **Affordable** → associated with words like `"price"`
- **Low-quality** → associated with words like `"poor"`

Both senses are collapsed into a single vector, which is the core limitation demonstrated.

```python
print(model.wv['cheap'][:10])  # Inspect the embedding
model.wv.most_similar("cheap", topn=10)  # Nearest neighbors
```

---

### (b) Context-Based Disambiguation System

A lightweight disambiguation system is built using **anchor vectors** and **cosine similarity**.

**Steps:**

1. **Define Anchors** — Representative vectors for each sense:
   - `affordable_anchor` → vector of `"price"`
   - `quality_anchor` → vector of `"poor"`

2. **Compute Context Vector** — Average embedding of all non-target words in the sentence.

3. **Classify** — Compare context vector to each anchor; assign the meaning with higher cosine similarity.

```python
classify_cheap("this is cheap and worth the price")   # → Affordable
classify_cheap("this looks cheap and poor quality")   # → Low-quality
```

---

### (c) Window Size & Its Effect

| Window Size | Captures | Best For |
|---|---|---|
| Small (e.g., 2) | Syntactic relationships | Grammar, modifiers, POS patterns |
| Large (e.g., 10) | Semantic relationships | Topic-level associations, meaning |

---

## Outputs

- First 10 dimensions of the `"cheap"` word vector
- Vocabulary membership checks (`"affordable"`, `"flimsy"`)
- Top-10 most similar words to `"cheap"`
- Cosine similarity scores: `cheap vs price`, `cheap vs poor`
- Disambiguation classification for sample sentences

---

## Key Takeaway

> Word2Vec's single-vector-per-word design is both its strength and its limitation. It captures distributional semantics efficiently but **cannot separate multiple meanings** of a polysemous word. Context-aware models like **BERT** address this by producing dynamic, context-dependent embeddings.
