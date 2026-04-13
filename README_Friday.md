# Week 07 · Friday — NLP Evaluation & Production Readiness

**PG Diploma · AI-ML & Agentic AI Engineering · IIT Gandhinagar**

---

## Assignment Overview

| Field | Details |
|---|---|
| **Week / Day** | Week 07 · Friday — Synthesis Day |
| **Topics** | NLP Evaluation · ML Pitfalls · Model Selection · Production Readiness |
| **Dataset** | ShopSense Reviews (`product_reviews.csv` — provided on LMS) |
| **Full Dataset** | [kaggle.com/datasets/olistbr/brazilian-ecommerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) |
| **Duration** | 60–90 minutes (Sub-steps 1–5 in-session · Sub-steps 6–7 optional take-home) |
| **Submission** | Push notebook to `week-07/friday/` in your GitHub repo · Paste link in LMS |
| **Deadline** | Saturday · 09:15 AM |

---

## How to Run

1. Clone your repo and navigate to `week-07/friday/`
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Place `product_reviews.csv` in the working directory (download from LMS checkpoint)
4. Open and run the notebook top to bottom:
   ```bash
   jupyter notebook W7_Friday_DailyAssignment.ipynb
   ```

> **Note:** Avoid hardcoded file paths (e.g., `C:/Users/.../data.csv`). Use relative paths — the TA runs your notebook from a clean environment.

---

## Preconditions

Before starting, ensure the following are ready:

- 📂 ShopSense Reviews dataset loaded (`product_reviews.csv` — available on LMS checkpoint)
- 🤖 At least one trained sentiment classifier ready to evaluate
- 🧹 Review text preprocessed — HTML stripped, categories standardised, encoding artefacts removed
- 📓 A working notebook environment with standard NLP libraries

---

## The Scenario

ShopSense is preparing to greenlight a **review intelligence feature**. A prior sentiment classifier was deployed last week with 94% test accuracy and declared production-ready — but the customer support team has raised a critical alert: it is producing unexpected results, predicting "positive" for nearly every review, including confirmed 1-star complaints.

**Your task:** Diagnose what failed and determine whether your team's approach carries the same risk, then deliver a defensible production recommendation.

---

## Sub-steps

### 🟢 Easy — Required (Sub-steps 1 & 2)

**1. Class Distribution Analysis**
- Load the ShopSense corpus and examine the sentiment label distribution
- Document the class breakdown (count + percentage per category)
- Explain why simple accuracy is an unreliable metric for this distribution

**2. Model Evaluation**
- Evaluate your trained classifier using a metric appropriate for the class imbalance found in Sub-step 1
- Produce a plain-language performance summary suitable for a non-technical stakeholder (Priya Menon, Head of Product)
- This summary is the evidence base for Sub-steps 3, 4, and 5

---

### 🟡 Medium — Required (Sub-steps 3, 4 & 5)

**3. Constraint-Based Model Comparison**

Evaluate at least two sentiment classification approaches against ShopSense's three hard engineering constraints:

| Constraint | Detail |
|---|---|
| Domain shift | New product categories added next quarter with no retraining |
| Code-mixing | ~15% of reviews are in Hindi-English mixed language |
| Latency | Inference must complete in under 20 ms per review |

> ⚠️ Each constraint must be **quantitatively measured** — qualitative claims ("handles it better") will not pass.

**4. Cost Model & Production Recommendation**
- Define a cost model: business cost of a false negative vs. false positive in the ShopSense context
- Justify cost estimates in business terms
- Apply the cost model to Sub-step 2 metrics for each approach from Sub-step 3
- Calculate projected daily misclassification cost at 100,000 reviews/day
- Make and defend your production recommendation based on cost analysis, not model performance alone

**5. Technical Brief & Monitoring Specification**

Package your recommendation as a one-page technical brief with two parts:
- **Recommendation** — what ships, why, and what it cannot guarantee
- **Production monitoring spec** — which metric to track weekly, the retraining threshold, and how to detect model degradation before it surfaces as a customer complaint

---

### 🔴 Hard — Optional (Sub-steps 6 & 7)

**6. Failure Mode Reproduction & Fix**
- Reproduce the broken pipeline (94% test accuracy → predicts "positive" for everything in production)
- Identify every decision that contributed to the failure
- Fix each issue and show before-and-after performance with metrics that would have detected the problem pre-deployment

**7. Cost Impact of the Failure**
- Apply your Sub-step 4 cost model to the broken pipeline
- Calculate the projected daily business cost of the failure at 100,000 reviews/day
- Audit your Sub-step 4 recommendation against the same failure modes — does it share any vulnerabilities? Show your evidence

> At least one Hard attempt anywhere across the week is required to reach **Band 4** on Delivery.

---

## Scoring Rubric

**Total: /25** — Delivery + Correctness (/10) · Engineering Quality (/8) · Git / Documentation (/7)

| Band | Delivery + Correctness (/10) | Engineering Quality (/8) | Git / Docs (/7) | Description |
|---|---|---|---|---|
| 4 | 10 | 8 | 7 | All mandatory sub-steps pass + ≥1 Hard attempted. Modular code, readable names, edge cases handled. Clear folder structure, README, 5+ meaningful commits, no junk files. |
| 3 | 8 | 6 | 5 | 70–89% of mandatory sub-steps pass. Code is followable. Basic README, ≥3 commits. |
| 2 | 6 | 4 | 4 | 40–69% pass. README exists, ≥2 commits. |
| 1 | 3 | 2 | 2 | <40% pass. Single dump commit or late-night push. |
| 0 | 0 | 0 | 0 | Not submitted, code doesn't run, no commits, or raw AI output without critique. |

### Engineering Quality Indicators

| Indicator | What the TA Checks |
|---|---|
| ① Readable naming | Variable/function names describe intent (e.g., `clean_review_text()`, not `x`) |
| ② Modular structure | At least 2 functions per sub-step — no single giant notebook cell |
| ③ No magic numbers | Constants (thresholds, costs, window sizes) named or commented |
| ④ Defensive handling | `try/except` around I/O or external calls; edge cases considered |

---

## GitHub Submission Requirements

| # | Requirement |
|---|---|
| ① | Notebook in `week-07/friday/` folder — not dumped in the root |
| ② | This README with: how to run, Python version, and packages needed |
| ③ | At least 3 commits with descriptive messages — not `"final"` or `"done"` |
| ④ | No `.env` files, API keys, `__pycache__`, or `.ipynb_checkpoints` committed |

---

## AI Usage Policy

AI tools are permitted, with the following guardrails:

- Every AI-assisted sub-step must include the **exact prompt used** (appended to your notebook or in a `prompts.md` file)
- Include a brief **critique** for each: *"Was the AI output correct? What did I change and why?"*
- Submitting raw AI output without modification or critique **zeros your Engineering Quality score** for the entire week (Dimension B = 0 pts)
- Sub-steps 3 and 7 require testing on your specific classifiers and applying your cost model — AI cannot do this for you

---

## Common Mistakes That Cost Marks

- ❌ **Hardcoded file paths** — breaks when the TA runs the notebook
- ❌ **Using accuracy as the primary metric** on an imbalanced corpus — Sub-steps 1 & 2 will not pass
- ❌ **Qualitative-only assessment** in Sub-step 3 — "handles it better" without numbers does not pass
- ❌ **No defined cost model** in Sub-step 4 — recommendation must be grounded in business impact
- ❌ **Sub-steps 3–5 completed without using Sub-step 2's evaluation** — the chain breaks
- ❌ **Single end-of-week commit** — caps Git score at Band 1
- ❌ **All code in one flat cell** with no functions — caps Engineering at Band 1
- ❌ **AI output submitted without prompt or critique** — Dimension B zeroed entirely
