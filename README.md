# Niño 3.4 Multi-Horizon Forecasting with Neural Networks

This repository presents a neural-network workflow for multi-horizon Niño 3.4 forecasting. It uses recent Niño 3.4 index values to predict the next six monthly horizons and compares independent, shared, weighted, and transfer-learning MLP strategies.

## Project Overview

The main question explored in this project is whether multi-step forecasting is better handled by separate models for each forecast horizon or by one shared model that predicts all horizons together.

The workflow compares:

* **Single-output MLPs**: one independent neural network for each forecast horizon.
* **Transfer learning for t+6**: a short-horizon model is reused and adapted for the longest forecast horizon.
* **Two-stage transfer learning for t+6**: a new prediction head is trained first, followed by selective fine-tuning.
* **Multi-output MLP**: one shared neural network predicts all six future horizons in a single forward pass.
* **Horizon-weighted multi-output MLP**: a shared model trained with higher loss weights for later forecast horizons.

## Dataset

This repository does not include the original dataset files.

The notebook expects a prepared supervised-learning CSV with monthly Niño 3.4 values, lag features, and six future monthly prediction targets. To rerun the notebook, provide the data in one of the following formats:

```text
Dataset.zip
```

or extracted CSV files under:

```text
data/Nino3.4_data.csv
data/test_years.csv
```

The expected modelling columns are:

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

The notebook checks for the extracted CSV files first. If they are already available, it uses them directly. If they are not found, it attempts to extract `Dataset.zip`.

A public-data version can be rebuilt from the NOAA PSL Niño 3.4 monthly time series:

https://psl.noaa.gov/data/timeseries/month/Nino34_CPC/

The NOAA PSL file is a raw monthly time series, so it does not already contain the supervised-learning columns used by this project. To use it with this workflow, the monthly index values need to be converted into lag features and future forecast targets first.

## Repository Structure

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── Niño_3_4_Multi_Horizon_Forecasting_with_Neural_Networks.ipynb
├── data/
│   └── README.md
└── reports/
    ├── single_output_mlp_metrics.csv
    ├── multi_output_mlp_metrics.csv
    ├── horizon_weighted_vs_multi_output_baseline.csv
    ├── model_comparison_test_metrics.csv
    ├── single_output_mlp_training_histories.png
    ├── multi_output_mlp_test_scatter.png
    └── horizon_weighted_multi_output_test_scatter.png
```

Trained model files, scaler objects, local datasets, and dataset archives are generated or provided separately and are intentionally excluded from version control.

## Method

The workflow follows these steps:

1. Prepare the local Niño 3.4 supervised-learning dataset.
2. Split the data into training, validation, and test sets using a year-based test split.
3. Standardise the input features using a scaler fitted only on the training set.
4. Train one MLP per forecast horizon for the single-output baseline.
5. Train transfer-learning variants for the longest forecast horizon.
6. Train a standard multi-output MLP for all six horizons.
7. Train a horizon-weighted multi-output MLP.
8. Evaluate each model using:

   * Root Mean Squared Error (RMSE)
   * Pearson correlation
9. Save comparison tables and diagnostic plots under `reports/`.

## Results

The saved run gives the following average test-set results across the six forecast horizons:

| Model                             | Average RMSE | Average Correlation |
| --------------------------------- | -----------: | ------------------: |
| Single-output MLP                 |       0.5576 |              0.7796 |
| Multi-output MLP                  |       0.5580 |              0.7794 |
| Horizon-weighted multi-output MLP |       0.5597 |              0.7799 |

In this run, the single-output MLP gives the lowest average RMSE, while the horizon-weighted multi-output MLP gives the highest average correlation. The differences are small, so the result is best interpreted as a controlled model comparison rather than a general claim that one architecture is always better.

Detailed horizon-level metrics are available in:

```text
reports/model_comparison_test_metrics.csv
reports/single_output_mlp_metrics.csv
reports/multi_output_mlp_metrics.csv
reports/horizon_weighted_vs_multi_output_baseline.csv
```

## Example Outputs

### Single-output MLP training histories

![Single-output MLP training histories](reports/single_output_mlp_training_histories.png)

### Standard multi-output MLP test scatter plots

![Standard multi-output MLP test scatter plots](reports/multi_output_mlp_test_scatter.png)

### Horizon-weighted multi-output MLP test scatter plots

![Horizon-weighted multi-output MLP test scatter plots](reports/horizon_weighted_multi_output_test_scatter.png)

## How to Run

This notebook was developed and tested in **Google Colab**.

The current notebook uses a Colab-style project workspace and Google Drive paths. It can be reproduced by placing the required dataset files in the expected project workspace before running the notebook cells.

### 1. Open the notebook in Google Colab

Open:

```text
Niño_3_4_Multi_Horizon_Forecasting_with_Neural_Networks.ipynb
```

### 2. Prepare the project workspace

In Google Drive, create the project folder expected by the notebook and place the dataset files there.

Use either:

```text
Dataset.zip
```

or:

```text
data/Nino3.4_data.csv
data/test_years.csv
```

### 3. Install dependencies

The notebook installs or imports the required Python packages during execution. The main dependencies are also listed in:

```text
requirements.txt
```

### 4. Run the notebook

Run the notebook cells from top to bottom.

The notebook will:

* mount Google Drive,
* locate or extract the dataset files,
* train and evaluate the neural-network models,
* save metric tables and plots under `reports/`,
* generate trained model and scaler files locally in the runtime or configured workspace.

## Local Execution Notes

The repository can be adapted to a local Python environment, but the current notebook is not fully path-independent for direct local execution.

For local execution, update the setup cell so that the project root points to the cloned repository folder instead of the Google Drive workspace. After that, install the dependencies and provide the required dataset files under the expected local paths.

Example local setup:

```bash
git clone https://github.com/MackintoshCHN/nino34-multi-horizon-forecasting.git
cd nino34-multi-horizon-forecasting
python -m venv .venv
```

Activate the environment:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Then update the notebook path configuration before running it locally.

## Notes and Limitations

* The repository includes implementation code, notebook outputs, metric tables, and result figures.
* Raw dataset files are not included.
* Trained model files and scaler files are not included.
* The model uses only recent Niño 3.4 lag values as input features.
* This is a compact neural-network forecasting experiment, not an operational ENSO forecasting system.
* Results may vary slightly depending on random seeds, hardware, TensorFlow version, and dataset split.
* The public NOAA PSL time series can be used to rebuild a similar supervised-learning table, but it requires preprocessing before it matches the modelling format used here.

## References

* NOAA Physical Sciences Laboratory. Niño 3.4 SST Index from the NOAA ERSST V5.
  https://psl.noaa.gov/data/timeseries/month/Nino34_CPC/

* NOAA Physical Sciences Laboratory. Climate Indices: Monthly Atmospheric and Ocean Time Series.
  https://psl.noaa.gov/data/timeseries/month/
