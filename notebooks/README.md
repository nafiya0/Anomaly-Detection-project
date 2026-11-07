# Notebooks

This folder contains the executable notebook for the improved energy-consumption anomaly detection project.

## Main notebook

[`Energy_anomaly_detection.ipynb`](Energy_anomaly_detection.ipynb)

The notebook implements an end-to-end workflow covering:

1. Loading and inspecting the Building Data Genome Project 2 electricity time-series data.
2. Cleaning timestamps, removing empty/constant meters, and handling limited missing values.
3. Engineering per-meter lag, rolling-statistic, and calendar features.
4. Creating a chronological train/test split to reduce temporal leakage.
5. Fitting an Isolation Forest and benchmarking Isolation Forest, Local Outlier Factor (LOF), and Robust Covariance (Elliptic Envelope).
6. Combining the three benchmark models using majority voting.
7. Detecting persistent upward/downward changes using a rolling 168-hour median and a robust training baseline.
8. Reviewing anomaly summaries and visualizations.

## Running in Google Colab

1. Open the notebook file in GitHub and choose **Open in Colab**, or download it and upload it to Google Colab.
2. Make a copy in your own Google Drive before running it.
3. Make sure the source electricity dataset is available at the path expected by the notebook: `/content/drive/MyDrive/electricity.txt`. If your file is stored elsewhere, update the input path in the notebook.
4. Run the cells in order. The workflow uses Google Drive for intermediate feature and prediction Parquet files so that large outputs do not need to remain in the Colab runtime.
5. Review the generated tables and plots. The visualization exported for this repository is stored in [`results/visualizations/`](../results/visualizations/).

## Important reproducibility notes

- The notebook expects access to the source dataset; the raw dataset is not included in this repository.
- Intermediate Parquet files and model outputs are written to Google Drive during execution. Create the expected output folders or follow the notebook's folder-creation cells before running later stages.
- Runtime, memory use, and execution time depend on the Colab environment and available resources.
- The three-model benchmark estimators were fitted on a limited sample of 20 training observations per meter. This limitation should be considered when interpreting the ensemble results.
- The dataset does not provide ground-truth anomaly labels for this analysis, so the notebook reports anomaly flags and descriptive rates rather than supervised classification metrics.

See the [root README](../README.md), [model documentation](../models/README.md), and [results summary](../results/README.md) for the project overview, model details, and reported findings.
