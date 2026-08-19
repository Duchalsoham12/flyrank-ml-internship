# Capstone Report — Content Decline Detection

- **Author:** Soham Duchal
- **Lane:** ML-12 — Content Decline Detection
-  **Repo:** https://github.com/Duchalsoham12/flyrank-ml-internship
- **Date:** 2026-08-19

## 0. Abstract

This project investigates how machine learning can identify content that may be declining and help editors prioritize content for review. The dataset contains 30,000 rows across 32 clients, with client information treated as grouping information rather than predictive features. A Random Forest model was trained to identify content with a downward trend using a grouped 80/20 evaluation split and random state 42. On the held-out test set of 6,163 rows, the model achieved Precision@50 of 1.00 compared with 0.24 for the transparent baseline. The resulting recommendation queue contains 6,163 test records and is intended to support editorial review and prioritization rather than make autonomous decisions.

## 1. Problem framing

The decision supported by this project is which content items should be reviewed first because their performance may be declining.

The unit of analysis is an individual content record. The model produces a ranking/score indicating which records are more likely to show a downward trend.

A FlyRank editor can use the ranked output to prioritize content for investigation, optimization, updating, or further review.

A wrong positive recommendation can waste editorial time on content that does not actually require attention. A wrong negative can cause genuinely declining content to be missed.

Machine learning is useful because the dataset contains multiple measurable signals that can be combined into a consistent ranking instead of relying only on a simple manual rule.

## 2. Data safety

The project uses the FlyRank ML Internship dataset containing 30,000 records across 32 clients.

Client identifiers were used only for grouping the train/test split and were not used as predictive features.

The following fields were deliberately excluded from model features:

- `content_id` — identifier, not a predictive feature.
- `client_id` — pseudonymous grouping identifier; excluded to prevent client-specific memorization.
- `trend_direction` — label-derived field and therefore leakage.
- `trend_pct` — directly derived from the observed trend and therefore a leakage risk.
- `is_declining_label` — label-derived field and therefore excluded from model features.

The target was based on `trend_direction == "down"`.

No client-identifying information is intentionally included in the model feature set or generated recommendation output.

## 3. Baseline

The baseline was a transparent rule-based ranking designed to provide a simple comparison before using machine learning.

The baseline was evaluated on the same held-out data and using the same Precision@50 metric as the Random Forest model.

The baseline achieved:

- **Baseline Precision@50:** 0.24
- **Random Forest Precision@50:** 1.00

This baseline is useful because it provides a simple, interpretable reference point for measuring whether the machine-learning approach provides additional ranking value.

## 4. Model / analysis

A Random Forest classifier was used because the task involves structured tabular data and potentially nonlinear relationships between the input signals and the declining-content target.

The target was defined as:

`trend_direction == "down"`

The model features were constructed from the available non-leaking content/performance signals.

The following fields were intentionally excluded:

- `content_id`
- `client_id`
- `trend_direction`
- `trend_pct`
- `is_declining_label`

The model therefore learns from available measurable content signals rather than directly receiving the answer through label-derived fields.

## 5. Evaluation

The dataset was divided using an **80/20 grouped split by client** so that client groups were not used as ordinary predictive features and the evaluation better represents performance on held-out client groups.

The split used:

- **Random state:** 42
- **Total records:** 30,000
- **Test records:** 6,163
- **Clients:** 32

The main ranking metric was Precision@50.

| Method | Precision@50 |
|---|---:|
| Baseline | 0.24 |
| Random Forest | 1.00 |

The Random Forest therefore substantially outperformed the baseline on the same evaluation split.

The test set contains 6,163 records, which form the evaluation/recommendation queue.

Error analysis should focus on the records ranked highly by the model that do not actually belong to the declining class, as well as declining records that receive lower ranks.

## 6. Interpretation

The model found that combinations of content/performance signals can distinguish records associated with a downward trend better than the transparent baseline.

The strongest practical finding is the improvement in top-ranked precision: the model achieved Precision@50 of 1.00, while the baseline achieved 0.24.

This means the model's highest-ranked 50 recommendations were all relevant according to the evaluation label.

The result should be interpreted as a measured ranking result on the held-out evaluation data, not as evidence that the model causes content performance to improve.

A limitation is that a high Precision@50 alone does not establish general performance across every future client or time period. Additional monitoring on new data is required.

## 7. Recommendation

The output is a ranked recommendation queue containing **6,163 test records**.

A FlyRank editor can use the queue as follows:

1. Start with the highest-ranked records.
2. Review the content and supporting signals.
3. Determine whether the content actually needs editorial attention.
4. Prioritize appropriate optimization or updating work.
5. Continue monitoring the content after the action.

The model provides decision support rather than an automatic editorial decision.

**Confidence:** High for the measured Precision@50 result on the evaluated test split.

**Limits:** The model should not be interpreted as predicting a search-engine algorithm or as proving that an editorial intervention will cause improved performance. Future data and monitoring are required to assess robustness.

## 8. Reproducibility

The experiment uses a fixed random state of **42**.

The evaluation uses an 80/20 grouped split by client.

The ML-12 pipeline exports three artifacts:

- `capstone_results.csv`
- `capstone_recommendations.csv`
- `capstone_monitoring.csv`

The completed run produced:

- **Dataset rows:** 30,000
- **Clients:** 32
- **Test rows:** 6,163
- **Precision@50:** 1.00
- **Recommendation queue:** 6,163
- **Artifacts exported:** 3

The notebook/script used to construct the evaluation data and produce the metrics should remain committed in the repository so that the result can be reproduced from a fresh clone.

## 9. Acknowledgments & data credit

Built on the FlyRank ML Internship dataset. Data source: [FlyRank](https://flyrank.ai).
