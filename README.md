# DS 3001 Semester Project

## Project information

- **Project title:** Predicting Patient Satisfaction at U.S. Hospitals
- **Team members:** Emma Chung, Kevin Sun, Anastacia Oliynyk.
- **Repository URL:** https://github.com/emmchung/Patient-Hospital-Satisfaction
- **Research question:** To what extent can hospital characteristics, quality-of-care measures, clinical outcomes, and Medicare spending predict overall patient satisfaction at U.S. hospitals?
- **Current status:** Initial exploration and missing-value preparation complete. Prediction models, cross-validation, and the final report are not complete.

## Repository structure

```text
.
├── README.md
├── notebook.ipynb
├── final_report.docx
├── requirements.txt
├── .gitignore
└── data/
    ├── README.md
    ├── source_manifest.json
    └── [four local CSV files, excluded from Git]
```

- [`notebook.ipynb`](notebook.ipynb) is the primary notebook, renamed from `hospital_analysis.ipynb`. It follows the template's Setup, Load data, Prepare data, Explore data, Build models, Evaluate models, Final results, and AI usage disclosure sections. The modeling sections explicitly record that those stages have not started.
- [`final_report.docx`](final_report.docx) is the **original, unfilled course report template**, retained with its required filename and formatting. It is not a completed report and its example figure is not a project result.
- [`data/README.md`](data/README.md) documents the provider, access method, exact input files, reporting-date limitations, and reconstruction steps. [`data/source_manifest.json`](data/source_manifest.json) records source hashes.

The layout follows the [DS 3001 project template](https://github.com/sds3001/project_template). Local adaptation does not change the existing repository's fork relationship or publish any changes.

## Analysis so far

The target is `HCAHPS Linear Mean Value` for measure `H_HSP_RATING_LINEAR_SCORE`. Selected predictors are Hospital Type, Hospital Ownership, Emergency Services, Mortality/Safety/Readmission better proportions, `Hybrid_HWM`, and `MSPB-1`. Other individual outcome measures and the overall CMS star rating are excluded from predictor analysis.

There are 5,419 reference hospitals, including 3,183 with numeric targets. Among reported target scores, the mean is 87.88 and the range is 69–98. Of these 3,183 hospitals, 571 have incomplete predictors. Missing targets are excluded from a separate supervised sample; missing numeric predictors are handled using training-only medians and indicators, and categorical gaps receive a `Missing` category. Prepared training and held-out sets contain 2,546 and 637 hospitals, respectively. The raw descriptive table remains unchanged.

Scoring periods differ across sources. Associations shown in the notebook are descriptive, use observed values, and do not establish predictive accuracy or causation. For future cross-validation, use raw training data and the unfitted preprocessor inside a model pipeline, rather than cross-validating the already imputed tables.

## Expanded exploratory correlation screen

The notebook also screens 116 numeric/binary candidates across the four files; 109 meet the ranking rule of at least 100 paired hospitals and nonconstant values. It reports Pearson and Spearman correlations and sample sizes, using observed values rather than imputations. This additional search does not change the original selected predictors or fit models.

The strongest other HCAHPS linear-score associations are recommending the hospital (Pearson r = 0.948) and nurse communication (r = 0.869), each based on 3,183 hospitals. These share the same survey and should not be presented as independent clinical predictors. Target-rating response percentages, target star ratings, the HCAHPS summary star rating, and the CMS overall hospital rating are separately flagged for overlap.

Outside HCAHPS and overlapping ratings, the largest absolute Pearson correlation is the count of mortality measures rated No Different (r = -0.355, n = 3,131). This is a reporting/performance count, not a mortality rate. Clinical and spending scores show no strong individual linear associations under this screen. Different available hospital subsets and the large number of comparisons limit direct comparisons and generalization.

The complete ranking, including unranked candidates and reasons, is in [`results/expanded_correlations.csv`](results/expanded_correlations.csv). See [`results/README.md`](results/README.md) for interpretation. The source CSVs remain unchanged.

## Getting started

Use Python 3.9 or later. Obtain the four CSVs as documented in `data/README.md`, then run:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m jupyterlab notebook.ipynb
```

Select the environment's Python kernel and run all cells from beginning to end. Saved outputs are included. The notebook contains an AI-use disclosure with selected actual prompts and verification details.

## Before submission

Team names still need to be supplied. Modeling, evaluation, and the report remain pending. Complete the course's Word template without changing its layout or filename, and link it to this repository, notebook, and data instructions. Restart the notebook kernel and run all cells before submission. Check the course's fork requirement: this existing repository has only been adapted locally; no fork, remote rewrite, commit, or push was performed.
