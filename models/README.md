# Models and Detection Methods

This folder documents the models and detection methods used in the improved energy-consumption anomaly detection workflow. The notebook contains the executable preprocessing, training, and evaluation code.

## 1. Isolation Forest

Isolation Forest is used as an unsupervised point-anomaly detector. It identifies observations that are easier to isolate from the rest of the data.

The notebook includes an Isolation Forest run trained using a sample of **150 observations per meter** with:
- 300 trees (`n_estimators=300`)
- Contamination setting: 0.05
- Random state: 42

A separate three-model benchmark also uses an Isolation Forest with 200 trees. Its results are reported alongside LOF and Robust Covariance in the Results section.

## 2. Local Outlier Factor (LOF)

Local Outlier Factor compares an observation's local density with the density of its neighbours. The benchmark uses novelty detection so the fitted model can score held-out observations.

Benchmark settings:
- 35 neighbours
- Contamination setting: 0.05
- `novelty=True`

## 3. Robust Covariance (Elliptic Envelope)

Elliptic Envelope estimates a robust covariance structure and flags observations that fall unusually far from the estimated distribution.

Benchmark settings:
- Contamination setting: 0.05
- `support_fraction=0.8`
- Random state: 42

## 4. Majority-voting ensemble

The benchmark combines the predictions of Isolation Forest, LOF, and Elliptic Envelope. An observation is flagged by the ensemble when **at least two of the three models** classify it as anomalous.

On the full scored test set of 4,452,047 observations, the majority-voting ensemble flagged 168,957 observations (3.80%). Individual benchmark-model rates were:
- Isolation Forest: 6.47%
- LOF: 4.98%
- Robust Covariance: 4.28%

See [the Results README](../results/README.md) for the vote distribution and visual summaries.

## 5. Persistent change detection

In addition to point-anomaly models, the workflow uses a rolling 168-hour median compared with each meter's training-period median, scaled by a robust MAD-based scale estimate. Scores above +3.5 or below -3.5 are flagged as upward or downward shifts. Contiguous flagged periods are grouped into episodes, and episodes lasting at least 24 hours are retained for analysis.

This is a statistical change-detection method, not one of the three machine-learning models in the voting ensemble.

## Training and evaluation notes

- The time series is split chronologically: the first 80% of timestamps for training and the final 20% for testing.
- Model features are prepared per meter, and training-derived statistics are used when scaling test observations.
- The three-model benchmark estimators were fitted on a limited sample of **20 training observations per meter** (25,560 rows across 1,278 meters). This is an important limitation: benchmark rates are exploratory and should not be treated as validated fault rates.
- The dedicated Isolation Forest run used a different training sample size (150 observations per meter). Do not confuse its training setup with the three-model benchmark.
- The dataset has no ground-truth anomaly labels. Precision, recall, F1-score, and confirmed fault-detection performance cannot be calculated from this analysis.
- An anomaly flag indicates an unusual pattern relative to the fitted model; it does not establish the physical cause or prove that a meter or building is faulty.

## Reproducing the workflow

Refer to the notebook in [`notebooks/`](../notebooks/) for data preparation, feature engineering, chronological splitting, model fitting, scoring, ensemble voting, and visual analysis. Large fitted model files and intermediate Parquet batches are not included in this repository; they were stored in Google Drive during the Colab workflow.
