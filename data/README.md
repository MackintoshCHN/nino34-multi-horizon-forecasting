# Dataset

This folder is reserved for local dataset files used by the notebook.

The required dataset files are not included in this repository. To rerun the notebook, provide the data locally using one of the options below.

## Option 1: Use the Dataset Archive

Place the archive in the project root:

```text
Dataset.zip
```

The notebook will extract the archive into this `data/` folder.

## Option 2: Use Extracted CSV Files

Place the extracted CSV files directly under this folder:

```text
data/Nino3.4_data.csv
data/test_years.csv
```

The notebook checks for the extracted CSV files first. If they are already available, it uses them directly. If they are not found, it attempts to extract `Dataset.zip` from the project root.

## Expected Data Format

The modelling workflow expects a supervised-learning table with monthly Niño 3.4 lag features and future forecast targets.

Expected columns include:

```text
year
month
nino_tminus2
nino_tminus1
nino_t
nino_tplus1
nino_tplus2
nino_tplus3
nino_tplus4
nino_tplus5
nino_tplus6
```

## Public Data Option

The workflow can also be adapted to use the public NOAA PSL Niño 3.4 monthly time series:

```text
https://psl.noaa.gov/data/timeseries/month/Nino34_CPC/
```

The public NOAA file is a raw monthly time series. It does not already contain the supervised-learning columns used by this project.

To use it with this workflow, the monthly index values need to be converted into lag features and future forecast targets first. The train, validation, and test split logic should also be updated consistently after rebuilding the dataset.

## Files Not Tracked

Dataset files and archives should be provided locally and should not be committed to version control.

Examples of files that should remain local:

```text
Dataset.zip
Nino3.4_data.csv
test_years.csv
Nino3.4_data_hidden.csv
```

Only lightweight documentation is tracked in this folder.
