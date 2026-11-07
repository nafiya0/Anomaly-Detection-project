# Results

This folder contains exported visual summaries and analysis outputs from the improved implementation.

## Full-test ensemble summary

The full-test ensemble contains **4,452,047 scored observations**. Majority voting flagged **168,957 observations (3.80%)** as anomalies.

| Detection method | Anomaly rate |
|---|---:|
| Isolation Forest | 6.47% |
| Local Outlier Factor (LOF) | 4.98% |
| Robust Covariance (Elliptic Envelope) | 4.28% |
| Majority-voting ensemble | 3.80% |

The ensemble vote distribution was:
- 0 models flagged an observation: 4,000,994
- 1 model flagged an observation: 282,096
- 2 models flagged an observation: 89,082
- 3 models flagged an observation: 79,875

## Visualizations

The [visualizations folder](visualizations/) contains `energy_anomaly_visualizations.png`, which brings together four plots:

1. **Anomaly Detection Model Comparison** – compares the anomaly rates of the three individual models and the majority-voting ensemble.
2. **Majority Voting – Model Agreement** – shows how many models voted to flag each observation.
3. **Top 15 Meters by Ensemble Anomaly Rate** – highlights meters with the highest proportion of flagged observations.
4. **Monthly Median Electricity Reading Trends** – compares five selected meters and marks the start of the test period.

## Interpretation and limitations

Anomaly flags indicate unusual observations relative to the fitted models; they do not confirm equipment faults or operational causes. The dataset has no ground-truth anomaly labels, so precision, recall, and F1-score cannot be established from this analysis.

The three-model comparison estimators were fitted using a limited sample of 20 training observations per meter. Treat the resulting anomaly rates as exploratory model outputs, not validated fault rates.

Large intermediate Parquet batches should generally remain in Google Drive rather than being committed to Git.
