# Building Energy Consumption Anomaly Detection

An unsupervised machine-learning project for identifying unusual electricity-meter behaviour and persistent changes in energy consumption patterns.

This repository contains two related implementations:

- **Original implementation:** the initial pipeline and files retained in the repository root.
- **Improved implementation:** a rebuilt, notebook-based analysis with expanded data-quality checks, chronological evaluation, feature engineering, model comparison, majority voting, and persistent change detection.

## Improved implementation

The rebuilt analysis is being organised under these folders:

- `notebooks/` — Google Colab/Jupyter notebook containing the end-to-end analysis.
- `data/` — dataset notes. The raw dataset is not included; see the data instructions.
- `models/` — optional saved model artifacts and notes.
- `results/` — prediction summaries and exported results.
- `results/visualizations/` — project charts.

### Dataset

The analysis uses hourly electricity meter readings from the Building Data Genome Project 2 dataset. The working table contains a timestamp and individual meter columns. The rebuilt workflow selected 1,278 eligible meters after data-quality filtering.

The raw dataset is not stored in this repository. Refer to [data/README.md](data/README.md) for setup notes and dataset provenance.

### Methodology

1. Inspect the time index, meter coverage, missingness, and constant readings.
2. Remove fully empty and unsuitable meter columns.
3. Interpolate short internal gaps while retaining original-missingness flags.
4. Select eligible meters and engineer lag, rolling-window, deviation, and calendar features.
5. Split the data chronologically into training and test periods.
6. Train and compare Isolation Forest, Local Outlier Factor (LOF), and Robust Covariance.
7. Combine model predictions using majority voting (at least two of three models).
8. Detect persistent upward and downward changes relative to a robust training baseline.

### Current rebuilt test results

The full-test ensemble scored **4,452,047 observations** and flagged **168,957 observations (3.80%)** as anomalies.

| Model | Anomaly count | Anomaly rate |
|---|---:|---:|
| Isolation Forest | 287,895 | 6.47% |
| Local Outlier Factor | 221,522 | 4.98% |
| Robust Covariance | 190,468 | 4.28% |
| Majority-voting ensemble | 168,957 | 3.80% |

The ensemble models were fitted using a comparison sample of 20 training observations per meter. The full-test predictions therefore represent a **sample-trained benchmark**, not a model trained on the entire training dataset.

There are no ground-truth anomaly labels in this analysis. Anomaly rates are model-generated flags, not measured accuracy or confirmed equipment faults. Persistent-change flags are a separate analysis and should not be interpreted as point-anomaly labels.

### Reproducibility

The notebook and exported outputs will be added to the folders above. To reproduce the analysis, obtain the source dataset according to its terms, place it in your Google Drive, and follow the notebook's setup instructions. The notebook uses Google Colab and Drive paths; update paths if running elsewhere.

### Limitations

- No labelled ground truth is available, so precision, recall, F1-score, and detection accuracy cannot be established.
- The three-model ensemble uses a sampled training benchmark (20 observations per meter).
- A high anomaly rate can reflect distribution shift, operational changes, data-quality issues, or unusual consumption; it does not prove a fault.
- Meter readings are described as readings; units should only be stated if confirmed by the dataset documentation.

## Original implementation

The original scripts, notebook, model artifacts, and result files are retained in the repository root for reference. Their assumptions and outputs are distinct from the rebuilt notebook results above.
