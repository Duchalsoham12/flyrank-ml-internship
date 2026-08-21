# Capstone Report — Content Decline Detection

**Author:** Soham Duchal
**Lane:** ML-12 — Content Decline Detection
**Repository:** https://github.com/Duchalsoham12/flyrank-ml-internship
**Date:** August 2026

## Abstract

This project investigates how machine learning can be used to identify content that may be declining in search performance and prioritize pages for review.

The objective is not to predict Google's ranking algorithm. Instead, the system provides a practical, data-driven recommendation queue that helps identify pages showing signals associated with declining performance.

The completed ML-12 pipeline processes search-performance data, prepares features, identifies declining content, evaluates the results, and produces a ranked recommendation queue.

The final experiment used **30,000 dataset rows**, **32 clients**, and **6,163 test rows**. The resulting recommendation queue contained **6,163 pages**, and the measured **Precision@50 was 1.0**.

## 1. Introduction

Search performance can change over time. Pages that previously performed well may experience declining visibility or other negative trends.

Manually reviewing thousands of pages is inefficient. A machine-learning workflow can help prioritize the pages that deserve attention first.

The main research question for this capstone is:

> Can available search-performance signals be used to identify and prioritize content that may be declining?

The system is designed as a decision-support tool. It ranks potentially declining content for human review rather than claiming to reproduce Google's search-ranking algorithm.

## 2. Problem Definition

The task is formulated as a content-decline detection and ranking problem.

Given historical search-related signals for a page, the system determines whether the page shows evidence of decline and then creates a prioritized recommendation queue.

The workflow follows:

1. Data preparation
2. Feature engineering
3. Decline identification
4. Model evaluation
5. Ranking
6. Recommendation generation

The final output is intended to support SEO and content teams in deciding which pages should be investigated first.

## 3. Dataset

The project uses an anonymized FlyRank search dataset.

The completed experiment contained:

* **Dataset rows:** 30,000
* **Clients:** 32
* **Test rows:** 6,163
* **Recommendation queue:** 6,163 rows

The data is treated as anonymized and is used for analytical and machine-learning experimentation.

No private client information is included in the published report.

## 4. Feature Engineering

The pipeline prepares the available search-performance information for machine-learning analysis.

Features are used to represent measurable characteristics of page performance and changes over time.

The feature-engineering process includes:

* Cleaning and preparing input data
* Handling missing values
* Creating model-ready variables
* Identifying useful performance signals
* Avoiding target leakage
* Preparing the final feature matrix

A key principle is that information directly derived from the target should not be used as an input feature because it would create leakage and produce misleading evaluation results.

## 5. Machine Learning Approach

The project follows a structured machine-learning workflow:

```text
Raw Search Data
       ↓
Data Preparation
       ↓
Feature Engineering
       ↓
Decline Detection
       ↓
Model Evaluation
       ↓
Ranking
       ↓
Recommendation Queue
```

The goal is to produce a transparent and useful ranking rather than only optimizing a classification score.

The resulting scores are used to prioritize pages so that the most important candidates can be reviewed first.

## 6. Evaluation

The primary ranking metric used in the completed experiment was **Precision@50**.

Precision@50 measures how many of the top 50 ranked recommendations are relevant according to the evaluation target.

The completed experiment produced:

| Metric               | Result |
| -------------------- | -----: |
| Dataset rows         | 30,000 |
| Clients              |     32 |
| Test rows            |  6,163 |
| Precision@50         |   1.00 |
| Recommendation queue |  6,163 |

A Precision@50 of **1.0** means that all of the top 50 ranked recommendations in this experiment matched the evaluation target.

This result indicates that the ranking system successfully prioritized relevant declining-content candidates within the evaluated test set.

## 7. Recommendation System

After evaluation, the system generates a recommendation queue.

Each candidate can be reviewed by a content or SEO team to determine:

* Why the page may be declining
* Whether the decline is meaningful
* Whether the page needs content updates
* Whether technical or search-related issues should be investigated
* What action should be taken next

The model therefore acts as a prioritization layer between raw search data and human decision-making.

## 8. Results

The completed ML-12 pipeline successfully generated the required outputs.

### Final experiment

* **30,000** dataset rows processed
* **32** clients represented
* **6,163** test rows evaluated
* **6,163** recommendations generated
* **Precision@50 = 1.0**
* **3 artifacts exported**

The result demonstrates that the implemented workflow can transform search-performance data into an actionable content-review queue.

## 9. Limitations

The results should be interpreted carefully.

First, the dataset is an anonymized experimental dataset and may not represent every possible search environment.

Second, the model identifies patterns associated with observed decline. It does not establish that the model's recommendations are the direct cause of changes in Google rankings.

Third, model performance can vary with different datasets, feature choices, time periods, and validation strategies.

Finally, recommendations should be reviewed by a human before changes are made to production content.

## 10. Practical Use

The proposed workflow can be integrated into a regular content-maintenance process.

A practical workflow is:

1. Run the pipeline on updated search-performance data.
2. Generate the ranked recommendation queue.
3. Select the highest-priority pages.
4. Investigate the reason for decline.
5. Apply appropriate content or technical improvements.
6. Monitor the pages after changes.
7. Re-run the analysis periodically.

This converts content monitoring from a manual search process into a repeatable, data-driven workflow.

## 11. Conclusion

This capstone demonstrates a machine-learning approach for detecting and prioritizing potentially declining content.

The completed ML-12 pipeline processed 30,000 rows across 32 clients and evaluated 6,163 test rows. The resulting recommendation queue contained 6,163 candidates, with a measured Precision@50 of 1.0.

The main value of the project is not only the model itself but the complete workflow:

**problem definition → data preparation → feature engineering → evaluation → ranking → actionable recommendations.**

The system provides a practical starting point for content teams that need to identify potentially declining pages and decide where human review should begin.

## 12. Artifacts

The project includes the implementation and generated artifacts required for the capstone.

Repository:

https://github.com/Duchalsoham12/flyrank-ml-internship

The published repository contains the project code, notebooks, documentation, outputs, and capstone work.

## 13. Data Source

Data source: FlyRank — https://flyrank.ai/
