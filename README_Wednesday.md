# Day 39 – Wednesday: Sentiment Feature Engineering for Edge Cases

## Overview

This script addresses **five common failure cases** in baseline sentiment analysis models (e.g., TF-IDF + classifier). For each failure, a custom **preprocessing function** and **feature engineering strategy** is designed to recover the lost sentiment signal.

---

## Dataset

| Field | Details |
|---|---|
| File | `preprocessed_reviews.csv` |
| Column Used | `review_text` |
| Format | Raw text strings, tokenized on whitespace |

---

## Dependencies

```bash
pip install pandas
```

| Library | Purpose |
|---|---|
| `pandas` | Loading the review dataset |

---

## Edge Cases & Solutions

### (a) Negation Handling

**Problem:** Phrases like `"not bad at all"` are misclassified as negative because TF-IDF treats `"bad"` as a standalone negative signal.

**Fix — `handle_negation(tokens)`**

Prefixes the word immediately following a negation trigger (`not`, `no`, `never`) with `NOT_`.

```
Input:  ["not", "bad", "at", "all"]
Output: ["NOT_bad", "at", "all"]
```

**Effect:** The model learns `bad` = negative and `NOT_bad` = positive as separate, distinct tokens.

---

### (b) Sarcasm Detection

**Problem:** Reviews like `"Wow great! Broke on day 1"` contain strong positive words (`wow`, `great`) that override the actual negative sentiment.

**Fix — `sarcasm_feature(tokens)`**

Returns a binary flag `1` if the sentence contains **both** positive and negative sentiment words simultaneously.

```
sarcasm_flag = 1  →  contradictory sentiment detected
```

**Effect:** Adds a domain-aware signal that prompts the model to treat the sentiment as conflicted or negative.

---

### (c) Code-Mixing (Hinglish Normalization)

**Problem:** Mixed Hindi-English reviews like `"Product bahut accha hai lekin delivery late thi"` lose meaning because Hindi words are treated as noise by TF-IDF.

**Fix — `normalize_code_mix(tokens)`**

Uses a bilingual dictionary to translate known Hindi words into their English equivalents before feature extraction.

```python
hinglish_dict = {
    "accha": "good",
    "bahut": "very",
    "lekin": "but",
    "thi":   "was"
}
```

```
Input:  ["bahut", "accha"]
Output: ["very", "good"]
```

**Effect:** Sentiment-bearing Hindi words are preserved and contribute meaningfully to TF-IDF features.

---

### (d) Implicit Sentiment

**Problem:** Reviews like `"Returned it within 2 hours"` contain no explicit sentiment words, causing the model to predict neutral.

**Fix — `implicit_features(text)`**

Adds binary flags for domain-specific action words that strongly imply negative sentiment.

| Feature | Trigger | Implied Sentiment |
|---|---|---|
| `returned_flag` | `"return"` in text | Negative |
| `refund_flag` | `"refund"` in text | Negative |

**Effect:** Turns implicit behavioral signals into explicit model features.

---

### (e) Comparative Statements

**Problem:** Reviews like `"Way better than my previous Samsung"` are ambiguous — the model sees `"Samsung"` + `"better"` but can't determine which product is being praised.

**Fix — `comparative_features(tokens)`**

Detects comparison patterns using a two-feature binary signal:

| Feature | Trigger | Implied Sentiment |
|---|---|---|
| `has_better` | `"better"` in tokens | Current product = Positive |
| `has_than` | `"than"` in tokens | Comparison present |
| `has_worse` *(optional)* | `"worse"` in tokens | Current product = Negative |

**Effect:** The `better + than` pattern is treated as a positive signal for the product under review.

---

## Feature Vector Construction

All features are assembled into a **single final vector** for each review:

```python
final_vector = tfidf_vector          # TF-IDF base features
             + [neg_feat]            # Negation count/flag
             + [sarcasm_feat]        # Sarcasm binary flag
             + [implicit_feat]       # Return/refund flags
             + [comp_feat]           # Comparative pattern flags
```

```
Feature vector length: 10 + 4 custom features = 14
```

> **Note:** `tfidf_sparse`, `negation_features`, and `sarcasm_flag` are stubbed with placeholder implementations in the current script and are expected to be replaced with full TF-IDF pipeline outputs in production.

---

## Summary Table

| Edge Case | Baseline Failure | Fix Applied |
|---|---|---|
| Negation | `"not bad"` → Negative | `NOT_` prefix tokens |
| Sarcasm | `"Wow great! Broke"` → Positive | Contradiction binary flag |
| Code-mixing | Hindi words ignored | Hinglish → English dictionary |
| Implicit sentiment | `"Returned it"` → Neutral | Action-word binary flags |
| Comparative | `"Better than X"` → Ambiguous | `better + than` pattern detection |

---

## Key Takeaway

> Baseline TF-IDF models fail on linguistically complex reviews. Domain-specific **preprocessing** and **hand-crafted features** for negation, sarcasm, code-mixing, implicit signals, and comparisons significantly improve sentiment classification on real-world noisy data.
