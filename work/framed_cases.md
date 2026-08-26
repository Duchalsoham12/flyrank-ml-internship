# ML-12: Content Decline Detection

## The Problem

Some content on a site loses search performance over time, but with thousands of content items, it's hard to know which ones actually need attention first. Checking each item manually doesn't scale, and relying on a single metric (like a traffic drop) misses cases where the real signal is spread across multiple factors — engagement, trend direction, search intent match, and so on.

The goal was to build a model that could pull in multiple performance signals at once and produce a prioritized list of content likely to be declining, instead of a flat metric or a manual review process.

## What I Did

This was built during my FlyRank internship using anonymized starter data: 30,000 content records across 32 clients. Each record included signals like search volume, competition, CPC, content type, search intent, word count, impressions, clicks, pageviews, sessions, engaged sessions, AI sessions, scroll events, content age, CTR, average position, engagement rate, scroll rate, and AI traffic percentage.

The dataset came with an existing `is_declining_label` (1 = declining, 0 = not) — I used this label as-is rather than defining my own decline threshold. Of the 30,000 records, 16,262 (54.2%) were labeled declining and 13,738 (45.8%) were not, so the classes were reasonably balanced.

Key decisions:

* **Model:** Random Forest classifier. It handles non-linear relationships across many tabular signals well, and I didn't run a broader model comparison — this was the model I used for the project.
* **Split:** 80/20 train-test split, grouped by client. This kept all records from a given client on one side of the split, so the model couldn't learn client-specific baselines and get credit for memorizing rather than generalizing.
* **Evaluation metric:** Precision@50. The practical goal wasn't overall classification accuracy — it was getting the most relevant declining content into the top of a review queue. Precision@50 measures that directly.
* **Output:** I built an actual recommendation queue from the model's predictions on the 6,163-row test set — each item ranked by decline score, with rule-based reason codes like `LOW_ENGAGEMENT` and `NEGATIVE_TREND` attached. These reason codes came from thresholds on specific signals, not from the model itself.

## What Came of It

On the 6,163-row held-out test set, the Random Forest reached a **Precision@50 of 1.00**, compared to a **Week-5 baseline Precision@50 of 0.24**.

I want to be direct about what that means and doesn't mean: **1.00 is the observed result on this specific test split, not a claim that the model will generalize at that level to new data.** A perfect score on one split is a reason to dig deeper, not a reason to declare the problem solved.

Two open questions remain:

* **No feature importance analysis.** I didn't check which signals the model actually relied on. Without that, I can't rule out that a feature closely tied to how the label was generated is inflating the result.
* **No multi-model comparison.** I used Random Forest directly and didn't benchmark it against other model types.

This was an internship exercise, not a deployed system — the recommendation queue was produced and evaluated, but not handed off to a live team. If I did this again, I'd test across additional client-based splits and check the label-generation process for leakage before treating the 1.00 result as robust.

Practically, this project also involved sorting out file-path errors and missing variables in the notebook environment, and rebuilding the workflow step by step until it ran cleanly.

## Bio

I'm a CSE AI & DS student building practical machine-learning projects for real-world problems.

## Contact

If you're a content or SEO manager dealing with a content library too large to review by hand, I'd like to talk about how a similar approach could work for your data. Reach out and let's discuss.

## Before / After

**Before:** "I leveraged machine learning to develop a robust solution that delivers actionable insights for optimizing content performance."

**After:** "I built a Random Forest model to identify declining content and turn the predictions into a prioritized review queue."
