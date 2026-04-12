# ShopSense Reviews — TF-IDF Analysis

A from-scratch implementation of TF-IDF (Term Frequency–Inverse Document Frequency) on 10,000 e-commerce product reviews, with cosine similarity–based search and a comparison against scikit-learn's `TfidfVectorizer`.

---

## Overview

This script processes a dataset of product reviews (`shopsense_reviews.csv`) and performs four core tasks:

| Task | Description |
|------|-------------|
| **(a) TF-IDF Matrix** | Builds a full sparse TF-IDF matrix for all 10,000 reviews from scratch |
| **(b) Query Ranking** | Ranks the top-5 most relevant reviews for a given query using cosine similarity |
| **(c) sklearn Comparison** | Compares the custom TF-IDF implementation against scikit-learn's `TfidfVectorizer` using average L2 difference |
| **(d) Category Analysis** | Finds the word with the highest average TF-IDF score within the Electronics category |

---

## Requirements

Install the required Python libraries before running:

```bash
pip install pandas numpy scipy scikit-learn nltk
```

You will also need to download NLTK stopwords (handled automatically in the script):

```python
import nltk
nltk.download('stopwords')
```

---

## Dataset

The script expects a CSV file named `shopsense_reviews.csv` with at least the following columns:

| Column | Description |
|--------|-------------|
| `review_text` | Raw text of each product review |
| `category` | Product category (e.g., `Electronics`) |

Update the file path in the script if running locally:

```python
df_full = pd.read_csv("/path/to/shopsense_reviews.csv")
```

---

## How It Works

### Text Preprocessing

Each review goes through the following pipeline before vectorization:

1. **HTML tag removal** — strips any `<tag>` elements
2. **Lowercasing** — converts all text to lowercase
3. **Alphabetic filtering** — keeps only `[a-z]` characters
4. **Stopword removal** — removes common English stopwords using NLTK

### TF-IDF (From Scratch)

- **Term Frequency (TF):** `count(word) / total_words_in_doc`
- **Inverse Document Frequency (IDF):** `log(N / (df + 1))` where `N` = total documents and `df` = number of documents containing the word
- **Storage:** Stored as a `scipy` sparse matrix (`csr_matrix`) for memory efficiency

### Query Ranking (Task b)

The query `"wireless earbuds battery life poor"` is preprocessed and converted into a TF-IDF vector using the same vocabulary. Cosine similarity is then computed against every document vector to return the top-5 most relevant reviews.

### sklearn Comparison (Task c)

Vocabularies from the custom implementation and `TfidfVectorizer` are aligned on common words. The average L2 (Euclidean) distance per document is computed to quantify the difference between the two methods.

> Note: Differences arise because sklearn applies L2 normalization and uses `log((N+1)/(df+1)) + 1` for IDF by default.

### Electronics Category Analysis (Task d)

Reviews belonging to the `Electronics` category are isolated, and the word with the highest **mean TF-IDF score** across all Electronics documents is identified. The result is the word `"camera"` — frequent and specific enough to the category to score highly.

---

## Output

The script prints the following to the console:

- Shape and number of non-zero values in the TF-IDF sparse matrix
- Top-5 review scores and text for the query in Task (b)
- Average L2 difference between custom and sklearn TF-IDF in Task (c)
- The top word by average TF-IDF in the Electronics category in Task (d)

---

## Key Finding

> **"camera"** has the highest average TF-IDF score in the Electronics category because it is both frequently used in Electronics reviews and relatively rare across other categories — a high TF × high IDF combination.

---

## File Structure

```
.
├── day_37_monday_submission.py   # Main script
├── shopsense_reviews.csv         # Input dataset (not included)
└── README.md                     # This file
```
