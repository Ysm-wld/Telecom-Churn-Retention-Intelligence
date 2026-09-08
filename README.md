# Device Lifecycle Intelligence System

Customer churn prediction and retention economics for Company A, an anonymous US telecom operator. The project combines exploratory analysis, lifecycle and segmentation features, model benchmarking, Optuna tuning, an ensemble champion, and retention ROI scenarios.

## What It Does

- Merges `telecom/Client.csv` and `telecom/Record.csv` on `Customer_ID`.
- Audits data quality and explores churn patterns.
- Builds lifecycle, usage, revenue, interaction, and customer-segmentation features.
- Compares Logistic Regression, XGBoost, LightGBM, CatBoost, and Random Forest.
- Tunes LightGBM with a reproducible 50-trial Optuna study.
- Produces a validation-weighted ensemble and ranks customers for retention action.
- Exports business-impact tables, figures, models, and reproducibility metadata.

The final notebook reports a validation-weighted ensemble with holdout PR-AUC 0.685036 and ROC-AUC 0.695620. These are historical project results; rerunning the workflow may regenerate artifacts and timestamps.

## Repository Layout

```text
.
├── README.md
├── requirements.txt
├── notebooks/YasminWalid.ipynb       # authoritative executable notebook
├── telecom/                           # source CSV data
├── outputs/                           # selected result tables, figures, and final models
├── docs/                              # concise project context
└── deliverables/                      # final PDF and presentation
```

## Setup

Use Python 3.11 or 3.12, create an isolated environment, and install the pinned dependencies:

```bash
python -m pip install -r requirements.txt
```

The notebook requires the two CSV files in `telecom/`. They are included here so the analysis is directly reproducible.

## Run

Open `notebooks/YasminWalid.ipynb` and run its cells from the repository root. The notebook is self-contained and uses deterministic seed `42`. It writes generated artifacts below `outputs/`.

The notebook is the sole executable project source. A full run trains several models and performs Optuna tuning, so runtime depends on the available CPU and memory.

## Results and Models

Important outputs include:

- `outputs/models/weighted_ensemble.joblib`: champion ensemble.
- `outputs/models/optuna_lightgbm.joblib`: tuned LightGBM champion.
- `outputs/tables/model_benchmark_expanded.csv`: model comparison.
- `outputs/tables/business_impact_summary.csv`: scenario-level retention economics.
- `outputs/tables/top_risk_customers.csv`: ranked customer-risk output.
- `outputs/figures/`: presentation-ready PNG figures.

The supplied models, figures, and tables are generated results retained as evaluation evidence. The notebook remains the reproducible source.

## Limitations

The source data is anonymized and contains abbreviated fields. Business impact depends on the assumptions documented in the notebook and project reports. The model is a proof of concept and should be validated with current operational data, campaign costs, treatment outcomes, and monitoring before production use.

## Documentation

See `docs/Telecom_Dataset_Intelligence_Report.md` for dataset context and field interpretation, and `docs/business_driver_report.md` for the executive interpretation. The final presentation is available in `deliverables/`.

## Credits

Prepared for the GCI World 2026 final assignment by Yasmin Walid. The project materials identify Company A as an anonymous telecom operator.
