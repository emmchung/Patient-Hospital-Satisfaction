# Project data

## Provider and access

Provider: Centers for Medicare & Medicaid Services (CMS). Source: [CMS Hospital Provider Data Catalog](https://data.cms.gov/provider-data/topics/hospitals). The [HCAHPS hospital dataset](https://data.cms.gov/provider-data/dataset/dgck-syfz) is also linked directly.

Access method: hospital-level CSV downloads supplied locally by the user. Exact download and release dates were not supplied and remain unknown. The notebook records measurement periods and file hashes; `source_manifest.json` records exact source-file fingerprints. CMS updates its catalog, so a later download may differ from this analysis. Match the recorded hashes when exact reproduction is required; an archive release date cannot be inferred from the hashes alone.

## Exact files used

Download the CSV export of each **Hospital** dataset and save it in this directory with the filename below. Do not use national or state summaries.

| Dataset | Local filename | Source rows |
| --- | --- | ---: |
| Patient Survey (HCAHPS) – Hospital | `HCAHPS-Hospital.csv` | 325,720 |
| Hospital General Information | `Hospital_General_Information.csv` | 5,419 |
| Complications and Deaths – Hospital | `Complications_and_Deaths-Hospital.csv` | 95,800 |
| Medicare Spending Per Beneficiary – Hospital | `Medicare_Hospital_Spending_Per_Patient-Hospital.csv` | 4,626 |

Raw CSV files are kept local and excluded from Git. The public notebook contains small sample rows and aggregate outputs from these public CMS files. No restricted patient-level data are used.

## Recreate the analytic data

Run `notebook.ipynb` from the repository root. It reads all columns as text, preserves hospital identifiers, selects the HCAHPS overall rating linear-score target, and joins the chosen predictors using `Facility ID` with one-to-one validation. General Information defines the reference hospital list. Mortality, Safety, and Readmission performance are summarized as Better / (Better + No Different + Worse), with missing or zero denominators left missing. Only `Hybrid_HWM` and `MSPB-1` score rows are selected from the two measure datasets.

The notebook preserves the descriptive table, excludes missing targets from a separate supervised sample, and creates a reproducible 80/20 split. Numeric predictor gaps use training-only medians plus missingness indicators; categorical gaps use a `Missing` category. Raw training data and an unfitted preprocessor remain available for later cross-validation. No prediction models have been built. Derived dataframes are created in memory, not exported as committed datasets.
