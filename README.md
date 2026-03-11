# Mental Health App Review Topic Mining
## Reviewing Mental Health App Reviews: An Unsupervised Machine Learning Exploratory Study
**Unsupervised NLP pipeline for topic discovery across 63,474 Google Play Store reviews**

UCL MSc Health Informatics Dissertation · Distinction Award · 2018  
📄 *Dissertation available upon request*

---

## What This Does

Mental health apps are increasingly used as frontline healthcare tools, yet there was no systematic method to analyze what users say about them at scale. This project scrapes, cleans, and clusters 63,474 reviews from 157 mental health apps to surface distinct user concern topics — without any labeled training data.

**Topics surfaced include:**
- App crashes and technical failures
- Subscription and payment complaints
- Chatbot and listener quality issues
- Meditation and relaxation feature satisfaction
- Healthcare professional engagement gaps

---

## Technical Stack

| Component | Approach |
|---|---|
| Data collection | Custom Selenium + BeautifulSoup web scraper |
| Text preprocessing | NLTK tokenization, Snowball stemming, custom stopword removal |
| Feature extraction | TF-IDF vectorization (scikit-learn) |
| Clustering | K-means with elbow + silhouette analysis for k selection |
| Dimensionality reduction | MDS (Multi-Dimensional Scaling) for 2D visualization |
| Evaluation | Internal (silhouette coefficient) + External (manual cluster inspection) |

---

## Key Results

**63,474** reviews · **157 apps** · **10 experimental iterations** · scraped June–July 2018

| Review Subset | Optimal k | Avg Silhouette Score |
|---|---|---|
| All reviews (thumbs-up filtered) | 7 | 0.0137 |
| Negative reviews (thumbs-up filtered) | 7 | 0.0133 |
| Positive reviews (thumbs-up filtered) | 12 | 0.0177 |

**Negative review topics:** Account/login issues · App crashes/bugs · Subscription complaints · Chatbot/listener problems · Ad disruptions · Lack of professional support

**Positive review topics:** Daily life management · Emotion management · Meditation aid · Relaxation tools · Healthcare professional engagement · Heart rate tracking · Listener/chatbot support · App design · Helpful functions

**Core finding:** Chatbots/listeners, subscriptions/payments, and healthcare professional involvement appeared in both positive and negative clusters — identifying them as the most salient user concerns across all sentiment groups.

---

## Repo Structure

```
HI-Dissertation/
├── data/
│   ├── raw/                        # Scraped raw CSVs (per search query, zipped)
│   └── processed/                  # Filtered iteration output files (IT3, IT5–IT10)
├── notebooks/                      # iPython notebooks (zipped)
├── outputs/                        # HTML cluster visualizations (zipped)
├── src/
│   ├── scraping/                   # Selenium + BeautifulSoup scrapers (per query)
│   └── clustering/                 # K-means iteration scripts (IT1–IT10)
├── requirements.txt
└── LICENSE
```

---

## Experimental Methodology

The 10 iterations systematically evaluate two axes: **filtering strategy** (word count threshold, helpfulness votes, sentiment segment) and **optimal k selection** per subset.

| Iterations | Variable Tested | Finding |
|---|---|---|
| IT1 vs IT2 | >5 words vs >10 words | >10 word threshold produces more semantically coherent clusters |
| IT2 vs IT3 | No thumbs-up filter vs thumbsup >1.0 | Thumbs-up filter improves cluster separation |
| IT3 vs IT4 | Thumbsup >1.0 vs >3.0 | Diminishing returns above >1.0; corpus too small at >3.0 |
| IT5/IT6/IT7 | Sentiment segmentation (no helpfulness filter) | Baseline — confirms negative reviews cluster more distinctly |
| IT8/IT9/IT10 | Sentiment segmentation + thumbsup >1.0 | Best results overall; positive reviews most silhouette-separable |

---

## Quickstart

```bash
pip install -r requirements.txt
```

Run any single iteration directly:
```bash
python iterations/DSA_Negative_Iteration8.py
```

> **Note:** Raw data path is hard-coded to a local CSV in each script. Update the path variable at the top of the script before running.

---

## Limitations & Future Work

- **Algorithm:** K-means requires pre-specifying k and assumes spherical clusters. HDBSCAN or LDA would be natural next steps. TF-IDF doesn't capture semantic similarity — replacing with sentence-transformers (`all-MiniLM-L6-v2`) would likely improve cluster quality substantially.
- **Evaluation:** No ground truth labels — silhouette scores are internal metrics only. A human annotation task on a sample would provide a real precision estimate.
- **Engineering:** The 10 iteration scripts share near-identical logic with varying filter parameters. A natural next step is consolidating into a single configurable pipeline with CLI arguments for reproducibility and easier experimentation.
- **Data:** English-only, Google Play only, single search query. Star rating as sentiment proxy is noisy.

---

## Context

Data scraped: June 30 – July 1, 2018 · Reviews dated September 2010 – June 2018 · Google Play Store (UK, English)  
Search string used: *"depression stress anxiety"*  
Scrape duration: 1 hour 36 minutes across 157 apps