# Building Energy Consumption Anomaly Detection

An unsupervised machine-learning project for identifying unusual electricity-meter behaviour and persistent changes in hourly energy-consumption patterns.

This repository preserves the original implementation and provides a separately organised improved implementation on the `improved-energy-anomaly-detection` branch.

## Improved implementation

The improved, end-to-end workflow is documented in the [Google Colab notebook](notebooks/Energy_anomaly_detection.ipynb).

| Folder | Contents |
|---|---|
| [`notebooks/`](notebooks/) | End-to-end notebook and instructions for running it in Google Colab |
| [`data/`](data/) | Dataset structure, access, and preprocessing notes |
| [`models/`](models/) | Detection methods, model settings, and limitations |
| [`results/`](results/) | Test-set summary and interpretation |
| [`results/visualizations/`](results/visualizations/) | Exported project charts |

### Dataset

The analysis uses hourly electricity-meter readings from the Building Data Genome Project 2 dataset. The source table covers 1 January 2016 through 31 December 2017 and contains individual meter columns.

The raw dataset is not included in this repository. See [`data/README.md`](data/README.md) for dataset details and setup notes. The cleaned modelling dataset retained 1,280 eligible meters; two additional meters were excluded from model-specific analysis because their baseline scale was invalid, leaving 1,278 valid meters for the main model workflow.

### Methodology

1. Inspect timestamps, meter coverage, missingness, and constant readings.
2. Remove fully empty columns and exclude constant-zero meters.
3. Interpolate short internal gaps while preserving original-missingness flags.
4. Select eligible meters and engineer per-meter lag, rolling-statistic, deviation, and calendar features.
5. Split the time series chronologically into training and test periods.
6. Fit an Isolation Forest and benchmark Isolation Forest, Local Outlier Factor (LOF), and Robust Covariance (Elliptic Envelope).
7. Combine the three benchmark predictions using majority voting (at least two of three models).
8. Separately detect persistent upward and downward changes using a rolling 168-hour median and a robust training baseline.

### Full-test benchmark results

The three-model ensemble scored **4,452,047 observations** and flagged **168,957 observations (3.80%)** as anomalies.

| Model | Anomaly count | Anomaly rate |
|---|---:|---:|
| Isolation Forest | 287,895 | 6.47% |
| Local Outlier Factor | 221,522 | 4.98% |
| Robust Covariance | 190,468 | 4.28% |
| Majority-voting ensemble | 168,957 | 3.80% |

The three ensemble estimators were fitted using a comparison sample of **20 training observations per meter** (25,560 rows across 1,278 meters). The full-test predictions therefore represent a sample-trained benchmark, not models trained on every available training observation.

The dedicated Isolation Forest workflow used a separate sample of 150 observations per meter. See [`models/README.md`](models/README.md) for the distinction between these runs.

### Visual results

The [visualization image](results/visualizations/energy_anomaly_visualizations.png) contains:
- A comparison of individual model and ensemble anomaly rates.
- The majority-voting agreement distribution.
- The top 15 meters by ensemble anomaly rate.
- Monthly median reading trends for five selected meters, with the test-period boundary marked.

### Reproducibility

To reproduce the analysis:

1. Obtain the source dataset from its original provider and follow its licence and attribution requirements.
2. Place the CSV-formatted source file in Google Drive at the path expected by the notebook (`/content/drive/MyDrive/electricity.txt`), or update the input path.
3. Open [`notebooks/Energy_anomaly_detection.ipynb`](notebooks/Energy_anomaly_detection.ipynb) in Google Colab.
4. Run the notebook cells in order. Intermediate feature and prediction Parquet files are stored in Google Drive during the workflow.

See [`notebooks/README.md`](notebooks/README.md) for more detailed execution notes.

### Limitations

- The analysis has no ground-truth anomaly labels. Precision, recall, F1-score, and validated detection accuracy cannot be established.
- The three-model benchmark was fitted on a limited sample of 20 training observations per meter.
- An anomaly flag indicates an observation that is unusual relative to the fitted model; it does not confirm an equipment fault or its cause.
- Persistent-change detection is a separate analysis and should not be interpreted as the same thing as point-anomaly classification.
- High anomaly rates may reflect distribution shifts, operational changes, data-quality issues, or unusual consumption.
- Readings are described as meter readings; units are not stated because they have not been independently confirmed here.

## Original implementation

The original scripts, notebook, model artifacts, and result files are retained in the repository root for reference. Their assumptions and outputs are distinct from the improved notebook-based workflow documented above.
