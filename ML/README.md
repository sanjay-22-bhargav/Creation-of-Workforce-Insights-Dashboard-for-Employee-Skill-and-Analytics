# Machine Learning — Workforce Insights Dashboard

This directory contains the machine-learning workflow used by the **Workforce Insights Dashboard for Employee Skills and Analytics**. It turns prepared workforce data into reproducible training datasets, model outputs, and prediction results that can support workforce analysis and dashboard reporting.

> **Important:** The models in this project are analytical decision-support tools. They must not be used as the sole basis for hiring, promotion, compensation, disciplinary, or termination decisions.

## What this module does

The ML workflow is designed to:

- prepare employee and workforce data for modeling;
- classify and document input features;
- create separate training and testing datasets;
- train and evaluate supervised learning models;
- generate predictions for workforce analysis;
- preserve model inputs and outputs for review; and
- provide outputs that can be consumed by the dashboard or backend services.

## Machine-learning workflow

```mermaid
flowchart LR
    A[Source workforce data] --> B[Cleaning and validation]
    B --> C[Feature classification and engineering]
    C --> D[ML-ready dataset]
    D --> E[Train/test split]
    E --> F[Model training]
    F --> G[Evaluation and comparison]
    G --> H[Saved model or pipeline]
    H --> I[Prediction notebook]
    I --> J[Dashboard and workforce insights]
```

### Workflow stages

1. **Data collection and validation** — confirm that the input file exists, has the expected columns, and contains usable values.
2. **Preprocessing** — clean missing values, normalize formats, encode categorical variables, and prepare numeric features.
3. **Feature classification** — document which fields are inputs, targets, identifiers, categorical variables, or numeric variables.
4. **Dataset preparation** — create an analytics-ready dataset and preserve the transformations used.
5. **Train/test splitting** — separate training examples from hold-out evaluation data.
6. **Model training** — fit one or more supervised models to the training features and labels.
7. **Evaluation** — inspect classification metrics, confusion matrices, and other appropriate validation results.
8. **Prediction** — apply the selected model to hold-out or new records and export results.
9. **Integration** — use validated outputs in the backend, dashboard, reports, or further analysis.

## Directory contents

| File or artifact | Purpose |
| --- | --- |
| [`04_ml_modeling.ipynb`](./04_ml_modeling.ipynb) | Main model-development notebook. It prepares data, trains models, evaluates results, and may save model artifacts. |
| [`05_ml_predictions.ipynb`](./05_ml_predictions.ipynb) | Applies the trained model or pipeline to test/new data and reviews prediction outputs. |
| [`Feature_Classification.xlsx`](./Feature_Classification.xlsx) | Feature reference workbook used to classify or document variables. |
| [`ML_Ready_Dataset.csv`](./ML_Ready_Dataset.csv) | Processed dataset prepared for machine-learning experiments. |
| [`X_train.csv`](./X_train.csv) | Training feature matrix. |
| [`y_train.csv`](./y_train.csv) | Training target labels. |
| [`X_test.csv`](./X_test.csv) | Hold-out feature matrix for evaluation. |
| [`y_test.csv`](./y_test.csv) | Hold-out target labels. |
| `ML part.docx` | Supporting ML documentation. |
| `milestone 1.pdf` | First project milestone report. |
| `Milestone_2 .pdf` | Follow-up milestone report. |

Generated files may change when notebooks are rerun. Treat generated datasets, metrics, and model artifacts as outputs tied to a particular dataset and code version.

## Environment setup

Use a virtual environment so that notebook results are not affected by unrelated global packages.

### macOS / Linux

```bash
cd ML
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install jupyter pandas numpy scikit-learn xgboost matplotlib seaborn openpyxl joblib
```

### Windows PowerShell

```powershell
cd ML
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install jupyter pandas numpy scikit-learn xgboost matplotlib seaborn openpyxl joblib
```

If the project later adds a dedicated ML dependency file, install it with:

```bash
python -m pip install -r requirements.txt
```

Recommended baseline:

- Python 3.9 or newer
- Jupyter Notebook or JupyterLab
- pandas and NumPy for data handling
- scikit-learn and XGBoost for modeling
- matplotlib and seaborn for visualizations
- openpyxl for Excel workbooks
- joblib for serialized model artifacts

## Running the notebooks

Start Jupyter from the repository root or from this directory:

```bash
jupyter lab
# or
jupyter notebook
```

Run the workflow in this order:

1. Complete the repository's data-cleaning and preprocessing workflow.
2. Confirm that `ML_Ready_Dataset.csv` and the feature documentation are current.
3. Open and run [`04_ml_modeling.ipynb`](./04_ml_modeling.ipynb).
4. Review the target definition, selected features, split strategy, metrics, and model outputs.
5. Save or export the final model/pipeline only after evaluating it.
6. Open and run [`05_ml_predictions.ipynb`](./05_ml_predictions.ipynb).
7. Compare predictions with the hold-out labels where available.
8. Publish only reviewed outputs to the dashboard or backend.

Restart the kernel and run all cells from top to bottom when validating reproducibility. Avoid relying on variables left over from an earlier interactive session.

## Data and artifact conventions

### `ML_Ready_Dataset.csv`

This is the primary processed dataset for the modeling workflow. It should be generated from a documented source and preprocessing procedure. Record the date, source version, row count, and important transformations whenever it changes.

### Training and testing files

The `X_train`/`y_train` pair is used to fit the model. The `X_test`/`y_test` pair is held back for an unbiased evaluation of generalization. Do not tune the final model repeatedly against the test set; use a validation split or cross-validation during experimentation.

### Feature classification

Use `Feature_Classification.xlsx` as a reference for interpreting fields. Before training, confirm that:

- the target column is clearly identified;
- identifiers and leakage-prone fields are excluded;
- categorical columns are encoded consistently;
- numeric columns use expected units and ranges;
- missing-value handling is documented; and
- training and inference use the same feature order and transformations.

## Modeling guidance

The notebooks support tabular supervised-learning experiments. Depending on the target and implementation, relevant evaluation measures may include:

- accuracy;
- precision;
- recall;
- F1 score;
- confusion matrix;
- ROC-AUC or PR-AUC where appropriate;
- calibration or probability quality; and
- subgroup and fairness checks.

Do not report a single metric without context. For imbalanced outcomes such as attrition, accuracy can be misleading; inspect class distribution and prioritize metrics that reflect the intended use case.

### Avoiding leakage

Data leakage can make a model appear more accurate than it really is. Check that:

- target-derived fields are not included as predictors;
- preprocessing is fitted only on training data;
- duplicate employees or near-duplicate rows do not cross the train/test boundary;
- future information is not used to predict a past or present outcome; and
- feature selection and hyperparameter tuning do not use hold-out test labels.

## Reproducibility checklist

For each experiment, record:

- dataset filename, version, and source;
- target definition and positive class;
- feature list and preprocessing steps;
- train/validation/test strategy and random seed;
- model type and hyperparameters;
- package versions;
- evaluation metrics and confusion matrix;
- model artifact path and serialization format; and
- known limitations or data-quality issues.

A model file should be treated as inseparable from the preprocessing code, feature schema, training data version, and dependency versions used to create it.

## Connecting to the dashboard and backend

The ML output can be used by the wider repository through reviewed CSV/model artifacts or through the Flask backend. Before integration:

1. verify that the inference schema matches the training schema;
2. validate incoming values and reject malformed requests;
3. confirm that categorical labels and missing-value behavior match training;
4. test predictions on known examples;
5. expose model version and timestamp in internal logs or metadata; and
6. document whether an output is historical analysis, a heuristic, or a validated prediction.

Historical records labelled as attrition are not the same as predictions of future attrition. Keep those concepts separate in dashboards, API responses, and documentation.

## Responsible use, privacy, and security

Employee data is sensitive. When working with this directory:

- use anonymized, synthetic, or approved data whenever possible;
- remove names, contact details, employee IDs, and other unnecessary identifiers;
- do not commit confidential exports, credentials, API keys, or local environment files;
- restrict access to raw datasets, trained models, prediction files, and logs;
- review model behavior across relevant demographic and organizational groups;
- test for bias, instability, and changes in data distribution;
- communicate uncertainty and limitations to users; and
- keep a qualified human decision-maker responsible for employment decisions.

Serialized model files can execute code when loaded by unsafe mechanisms. Only load trusted artifacts, and keep their provenance documented.

## Troubleshooting

### Notebook cannot find a dataset

Run the notebook from the expected working directory or update its input path to the repository's actual data location. Prefer paths derived from the project root rather than machine-specific absolute paths.

### Excel import fails

Install the Excel dependency and confirm that the workbook path and sheet name are correct:

```bash
python -m pip install openpyxl
```

### Predictions have missing or unexpected columns

Compare the inference dataframe with the feature list used during training. Check spelling, capitalization, encoding, column order, and missing-value handling.

### Results change after rerunning the notebook

Set random seeds where supported, record package versions, restart the kernel, run cells in order, and verify that the input dataset has not changed.

### Model performance looks unusually high

Investigate leakage, duplicate records, target imbalance, train/test overlap, and preprocessing performed before the data split.

## Recommended future improvements

- Add a pinned `ML/requirements.txt` or repository-wide environment file.
- Add automated data-quality and schema checks.
- Move reusable preprocessing and training logic from notebooks into Python modules.
- Add tests for feature transformations and prediction input validation.
- Store model artifacts in a clearly versioned `models/` directory.
- Publish a data dictionary and model card with every production-quality model.
- Track experiments, metrics, and dataset versions.
- Add CI checks that execute notebook smoke tests without exposing private data.
- Include explainability outputs only after validating that they are appropriate for the model and audience.

## Related resources

- [Repository README](../README.md)
- [Backend README](../Backend/README%20(2).md)
- [Root data and dashboard files](../)
- [RAG module](../RAG/)

## License

See the repository [LICENSE](../LICENSE) for the applicable license terms.
