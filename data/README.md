# Dataset

## Source file and access

The raw hourly electricity dataset is **not included** in this repository. The analysis was run in Google Colab using a CSV-formatted file named `electricity.txt`, stored in Google Drive.

The notebook expects the file at:

```text
/content/drive/MyDrive/electricity.txt
```

Update the input path in the notebook if your copy is stored elsewhere. Obtain the dataset from its original provider and follow its licence and attribution requirements before redistribution.

## Dataset structure

The source data contains a timestamp column and individual electricity-meter reading columns. The original table used in this workflow had **17,544 hourly timestamps** and **1,578 meter columns** (1,579 columns including the timestamp).

The time range was 1 January 2016 through 31 December 2017. The meter columns represent individual meters; do not assume each column represents an entire building.

## Cleaning and preparation summary

The notebook applies the following preparation steps:

- Converts and orders the timestamp column.
- Removes two completely empty meter columns.
- Excludes four meters that contain only zero readings.
- Interpolates short internal gaps using time-based interpolation, with a maximum gap of three consecutive time steps.
- Retains meters with no more than 5% missing readings for the modelling dataset.
- Preserves original missingness information for later analysis; zero readings are not treated as missing values.

After removing the two completely empty columns, the table contained **17,544 rows and 1,576 meter columns**. The modelling dataset contained **1,280 eligible meters** before later model-specific exclusions.

These figures describe the dataset and cleaning workflow used in this project; they may differ if you use a different source-data version.

## Data handling

- Do not upload private, restricted, or very large raw data files to this repository.
- Keep local copies and intermediate files in an appropriate storage location such as Google Drive.
- Check the source dataset's terms before sharing or redistributing it.
