## FedEx - Digital & Sustainable Supply Chain Modelling & Analytics

---

## Data Sources

1. **Weekly KPI Dataset (2022–2024)**
   
   Contains aggregated weekly metrics including:

   * Origin country and ramp
   * Business region
   * Destination lane
   * Fiscal year and week number
   * Product code
   * Pounds / aPounds
   * PACKS
   * Shipments

3. **August Actuals Dataset**
   
   Contains detailed weekly shipment KPIs including:

   * ORIG_RAMP
   * Orig_Ctry
   * Business_Region
   * Lane_ (short code)
   * Lane (long name)
   * Product
   * PACKS
   * aLbs
   * Shipments
   * yyyymm
   * WeekNbr

---

## What Was Implemented

### 1. Canonical Bronze Schema Creation

A standardized Bronze KPI table was created with the following keys:

* ship_date
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

This ensures consistency across all upstream data sources.

---

### 2. Lane Mapping Harmonization

Different datasets used inconsistent lane representations (e.g., EU vs Europe, NA vs Americas).

A standardized lane mapping was created:

* EU → Europe
* AS → APAC
* ME → MEISA
* NA / LA → Americas

Additionally, a recovery rule was applied:

If `Lane_` is null and `Lane == "Americas"`, then `Lane_` is set to `"LA"`.

All datasets were aligned to unified business lane naming.

---

### 3. Unit Standardization

Weight fields were inconsistent across datasets (aLbs, aPounds, kg).

We:

* Standardized all weights to `aPounds`
* Converted fields to numeric format
* Ensured consistent aggregation logic
* Optionally derived kg for analytical use

This prevents reporting discrepancies and aggregation errors.

---

### 4. Bronze Dataset Consolidation

* Columns were renamed to canonical names
* Schemas were aligned across datasets
* Data was concatenated into a single Bronze KPI table
* Missing measure values were filled with 0

This created a stable and governed Bronze layer.

---

### 5. Time Coverage & Continuity Grid (Modeling Requirement)

To prevent sparse reporting bias in time-series modeling:

* A complete weekly grid was generated across:
  ORIG_RAMP × Dest_Lane × Product_Code × FY × WeekNbr

* Missing week combinations were filled with zero volumes and weights

This guarantees:

* No missing time periods
* Accurate trend modeling
* Reliable forecasting inputs

---

## Final Output

A modeling-ready dataset:

* Harmonized lane definitions
* Standardized weight units
* Continuous weekly coverage
* Canonical Bronze schema
