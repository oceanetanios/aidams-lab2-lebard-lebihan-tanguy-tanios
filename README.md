# Lab 2 – Predicting Steel Plant Production

This notebook predicts how much crude steel a plant produced in 2023 from its capacity, workforce, age, production route and region. The data is Global Energy Monitor's [Global Iron and Steel Tracker](https://globalenergymonitor.org/projects/global-iron-steel-tracker) (June 2026 release).

It covers the whole modelling lifecycle: a schema check with Pandera, cleaning and feature engineering, baseline and linear models, cross-validation, tuning with GridSearchCV and Optuna, experiment tracking with MLflow, and saving the final pipeline.


## What's in the repo

- `lab_2.ipynb`: the notebook, with all outputs
- `gem-download/`: the GEM data files
- `best_pipeline.joblib`: the saved model (preprocessing and Ridge regression together)
- `mlflow.db` and `mlruns/`: the MLflow tracking store

## Setup

You need Python 3.11 or newer.

```bash
pip install pandas openpyxl numpy scikit-learn pandera mlflow optuna optuna-integration matplotlib seaborn joblib
```

## Running the notebook

1. Put the GEM Excel files in `gem-download/`. The notebook only reads `Plant-level_data_Global_Iron_and_Steel_Tracker_June_2026_V1.xlsx`.
2. Open `lab_2.ipynb`, restart the kernel and run all cells. It takes a couple of minutes, mostly for the Optuna trials.
3. To browse the experiments, run:

```bash
   mlflow ui --backend-store-uri sqlite:///mlflow.db
```

   Then open http://127.0.0.1:5000.

Every random step is seeded, so a re-run gives the same results.

## Using the saved model

```python
import joblib, numpy as np

model = joblib.load("best_pipeline.joblib")
production_kt = np.expm1(model.predict(X))  # the model predicts log production
```

`X` needs the same columns as in the notebook, including the features built in Task 1.4.
