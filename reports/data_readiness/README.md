
# Data Readiness Report – Bronze KPI Dataset

## 1. Purpose

This directory contains the **data quality validation outputs** and documentation for the Canonical Bronze KPI dataset.

The goal of the data readiness process is to ensure that the Bronze dataset is:

* Structurally consistent
* Deterministically reproducible
* Free from critical data quality issues
* Safe for downstream analytics and forecasting

---

# 2. Inputs

The Bronze dataset is built from the following source files:

1. Weekly KPI dataset (2022–2024)
2. August Actuals with DOM dataset
3. XLSB operational extract (where schema-compatible)

These inputs are processed through:

```
src/io/fedex_adapter.py
```

Which produces the canonical Bronze dataset.

---

# 3. How to Run Data Quality Checks

Data validation logic is implemented in:

```
src/quality/validation.py
```

To execute all QA checks:

```python
from src.io.fedex_adapter import build_bronze
from src.quality.validation import run_all_checks

bronze = build_bronze(weekly_path, aug_path, xlsb_path=None)
qa_results = run_all_checks(bronze)

```

---

# 4. QA Gates Implemented

The following deterministic checks are enforced:

## 4.1 Missingness Summary

* Percentage of null values per column
* Identification of mandatory key violations

---

## 4.2 Non-Negativity Checks

Ensures no negative values in:

* PACKS
* Shipments
* aPounds

---

## 4.3 Plausibility Flags

Flags potentially abnormal values such as:

* Extremely large weight spikes
* Abnormally high shipment counts
* Unexpected weekly jumps

---

# 5. Output Location

All QA artifacts are saved under:

```
reports/data_readiness/qa_outputs/
```

---

# 6. Bronze Dataset Definition

The validated dataset conforms to the schema defined in:

```
docs/fedex_data_mapping_v1.md
```

Primary keys:

* FY
* WeekNbr
* Orig_Ctry
* ORIG_RAMP
* Business_Region
* Dest_Lane
* Product_Code

Measures:

* PACKS
* Shipments
* aPounds

---

# 7. Data Readiness Status

Status: Baseline Bronze QA Implemented

The dataset is considered:

* Schema-harmonized
* Unit-standardized
* Lane-mapped
* Time-continuous
* QA-gated
