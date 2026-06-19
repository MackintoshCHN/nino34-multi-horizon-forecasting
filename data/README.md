# Dataset

This folder is used for local dataset files.

The original dataset files are not included in this repository. To rerun the notebook, provide the required data locally using one of the following options.

## Option 1: Use the dataset archive

Place the archive in the project root:

```
Dataset.zip
```

The notebook will extract it into this `data/` folder.

## Option 2: Use extracted CSV files

Place the extracted files directly under this folder:

```
data/Nino3.4_data.csv
data/test_years.csv
```

The notebook checks for the extracted CSV files first. If they are already available, it uses them directly. If they are not found, it attempts to extract `Dataset.zip` from the project root.

## Public data option

The modelling workflow can also be adapted to use the public NOAA PSL Niño 3.4 monthly time series:

```
https://psl.noaa.gov/data/timeseries/month/Nino34_CPC/
```

The public NOAA file is a raw monthly time series, so it does not already contain the supervised-learning columns used by this project. To use it, the monthly index values need to be converted into lag features and future forecast targets, such as:

```
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
