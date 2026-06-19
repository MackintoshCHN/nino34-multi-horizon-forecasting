# Dataset

This folder is used for local dataset files.

The original dataset files are not included in this public repository. To rerun the notebook, provide the required data locally using one of the following options.

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

## Public repository note

The original dataset files are excluded from this repository because they were provided as coursework materials.

For a fully public-data version of this project, the supervised learning table can be rebuilt from the public NOAA PSL Niño 3.4 monthly time series. In that case, the raw monthly index needs to be converted into lag features and future forecast targets before running the modelling workflow.
