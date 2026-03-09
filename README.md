# SECOM Yield Optimization & Guardband Design

## Overview
End-to-end semiconductor yield optimization system built on the UCI SECOM dataset (1567 wafers, 590 sensors). Demonstrates predictive maintenance, feature selection, yield modeling, guardband design, and continuous improvement planning on GCP.

## Business Impact
- 48% reduction in yield loss costs vs no-model baseline
- 16/20 test failures caught at optimal threshold vs 11/20 at default
- Risk-tiered intervention policy validated against actual failure rates
- CUSUM drift detection identified July 2008 yield crisis root cause

## Technical Stack
- **ML**: Scikit-learn, Imbalanced-learn (SMOTE), SciPy
- **SPC**: Univariate control charts, CUSUM drift detection
- **Cloud**: GCP Vertex AI Workbench, Cloud Storage
- **Languages**: Python, Pandas, NumPy, Matplotlib

## Project Structure
```
secom-yield-optimization/
├── notebooks/
│   ├── 01_eda_cleaning.ipynb              # EDA, missing data, feature reduction
│   ├── 02_feature_selection.ipynb         # LASSO + Mann-Whitney feature ranking
│   ├── 03_yield_modeling.ipynb            # Model training and evaluation
│   ├── 04_guardband_design.ipynb          # Threshold optimization, cost analysis
│   └── 05_continuous_improvement.ipynb    # Yield trends, SPC, CUSUM
├── src/                                   # Reusable Python modules
├── data/
│   ├── raw/                               # SECOM files (not tracked — download separately)
│   └── processed/                         # Outputs (not tracked — regenerate from notebooks)
├── models/                                # Trained model artifacts (not tracked)
├── requirements.txt
└── README.md
```

## Key Results

| Stage | Result |
|-------|--------|
| Raw sensors | 590 |
| After cleaning | 441 |
| After LASSO + Mann-Whitney | 18 |
| Model (Logistic Regression) | AUC-ROC: 0.735, Recall: 55% |
| Optimal threshold | 0.2828 (vs default 0.5) |
| Cost reduction | 48% vs baseline |

## Intervention Policy

| Tier | Probability | Action | Actual Fail Rate |
|------|------------|--------|-----------------|
| Low Risk | 0.00 – 0.15 | Normal processing | 2.0% |
| Medium Risk | 0.15 – 0.28 | Enhanced monitoring | 2.8% |
| High Risk | 0.28 – 0.60 | Engineering review | 8.2% |
| Critical | 0.60 – 1.00 | Hold wafer | 15.3% |

## Top Sensors

| Sensor | Separation Score | Risk Direction |
|--------|-----------------|----------------|
| sensor_478 | 0.664 | High values → failure |
| sensor_60 | 0.640 | High values → failure |
| sensor_511 | 0.638 | High values → failure |

## Data
[UCI SECOM Dataset](https://archive.ics.uci.edu/dataset/179/secom) — place `secom.data` and `secom_labels.data` in `data/raw/` before running notebooks.

## Setup
```bash
git clone https://github.com/YOUR_USERNAME/secom-yield-optimization
cd secom-yield-optimization
pip install -r requirements.txt
```
Run notebooks in order: 01 → 02 → 03 → 04 → 05
