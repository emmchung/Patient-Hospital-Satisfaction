# Hospital patient satisfaction: initial data exploration

Research question: To what extent can hospital characteristics, quality-of-care measures, clinical outcomes, and Medicare spending predict overall patient satisfaction at U.S. hospitals?

The completed initial exploration is in `hospital_analysis.ipynb`. It loads the four supplied hospital-level CMS CSV files, displays their actual columns and sample rows, checks hospital identifiers and measure keys, identifies the satisfaction target, compares coverage and reporting periods, and examines missing values. The notebook includes computed findings, tables, and charts. A descriptive table is joined by Facility ID with one-to-one validation. Missing-value handling is completed in Section 8; no prediction models are built.

## Data used

| File in `data/` | Source rows |
| --- | ---: |
| `HCAHPS-Hospital.csv` | 325,720 |
| `Hospital_General_Information.csv` | 5,419 |
| `Complications_and_Deaths-Hospital.csv` | 95,800 |
| `Medicare_Hospital_Spending_Per_Patient-Hospital.csv` | 4,626 |

The source is the [CMS Hospital Data Catalog](https://data.cms.gov/provider-data/topics/hospitals). Local CSVs remain unchanged and are excluded from Git. File hashes in the notebook identify the analyzed downloads. Download and CMS release dates were not provided; measurement periods are inspected directly from the files.

## Selected predictors

The exploration focuses only on:

- Hospital Type, Hospital Ownership, and Emergency Services.
- Mortality, Safety, and Readmission performance: for each group, Better / (Better + No Different + Worse). Missing counts or zero denominators produce missing proportions. Source counts and reporting coverage are audited.
- `Hybrid_HWM`, using the outcome file's `Score`.
- `MSPB-1`, using the spending file's `Score`.

The overall CMS hospital star rating, other HCAHPS predictors, and other individual clinical outcome measures are excluded. The notebook reports predictor availability, satisfaction distributions by hospital characteristics, and pairwise scatterplots and correlations for the selected numeric predictors. Descriptive results use observed values. Section 8 separately prepares data with missing-value handling; no prediction model is fitted.

## Initial findings

The target is `HCAHPS Linear Mean Value` for `HCAHPS Measure ID == "H_HSP_RATING_LINEAR_SCORE"`, labeled “Overall hospital rating - linear mean score”. There are 4,790 target rows with unique hospital identifiers: 3,183 numeric scores and 1,607 values labeled “Not Available”. Numeric values range from 69 to 98.

The intended target scale is 0–100. It is separate from a hospital star rating or a percentage of patients selecting a response category. These files contain hospital aggregates, not individual patient records.

HCAHPS covers October 2024–September 2025, spending covers calendar year 2024, and outcome measures have several different reporting windows. Missingness and period differences need attention before any later modeling. Blank footnotes, unavailable scores, and structurally inapplicable fields are reported separately. Exact administrative reasons for missing scores remain unresolved without the release's footnote dictionary.

These descriptive findings do not establish predictive accuracy or causation. The full tables, plots, and limitations are in the notebook.

## Missing-value handling

Section 8 excludes reference hospitals without numeric targets from a separate supervised sample; targets are never imputed. All hospitals with observed targets are retained, including those with missing predictors. The original descriptive dataframe remains unchanged.

An 80/20 split (random seed 42) supports leakage-free preparation. Numeric predictor gaps are filled with training-only medians, with missingness indicators. Categorical gaps receive a `Missing` category. `X_train_prepared` and `X_test_prepared` have no missing values; targets are stored separately in `y_train` and `y_test`. Hospital IDs remain indices, not predictors. Categorical encoding is deferred to the modeling stage.

For future cross-validation, use raw `X_train` and `y_train` with the **unfitted** `preprocessor` inside a model pipeline, and leave the test set untouched. Do not cross-validate the pre-imputed tables. The median policy does not eliminate potential bias from missing or selectively reported data. No prediction models or cross-validation have been run.

## Reproduce

Use Python 3.9 or later. From this folder:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyterlab hospital_analysis.ipynb
```

Select the environment's Python kernel and run all cells in order. Keep the supplied filenames in `data/`. The saved notebook includes outputs; rerunning replaces them with results from the current files. Review notebook outputs before sharing because they may include source data.
