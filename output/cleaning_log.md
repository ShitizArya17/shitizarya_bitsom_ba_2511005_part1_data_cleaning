# Cleaning Log – Part 1: Data Cleaning

## 1. Issues Found

### Missing Values

| Column Name | Total Rows | Missing Count | % Missing |
| ----------- | ---------- | ------------- | --------- |
| region      | 932        | 26            | 2.79%     |
| ship_mode   | 932        | 22            | 2.36%     |
| discount    | 932        | 18            | 1.93%     |

### Duplicate Records

| Issue                      | Count |
| -------------------------- | ----- |
| Exact duplicate rows found | 40    |
| Duplicate order IDs found  | 63    |
| Records removed            | 39    |
| Records flagged for review | 43    |

### Discount Validation Issues

| Issue                                            | Count |
| ------------------------------------------------ | ----- |
| Negative discounts                               | 16    |
| Zero discounts (including filled missing values) | 245   |

**Note:** One negative discount record also contained a duplicate order ID with different order details.

### Date Validation Issues

| Issue                                                  | Count |
| ------------------------------------------------------ | ----- |
| Invalid Shipping Records (ship date before order date) | 95    |

### Order Status Issues

| Status                | Count |
| --------------------- | ----- |
| Cancelled Orders      | 146   |
| Returned Orders       | 164   |
| Completed Orders      | 622   |
| Payment Failed Orders | 69    |

### Sales & Profit Calculation Issues

| Issue                                       | Count |
| ------------------------------------------- | ----- |
| Sales calculated vs actual sales mismatch   | 6     |
| Profit calculated vs actual profit mismatch | 6     |

---

## 2. Cleaning Actions Performed

* Removed leading, trailing, and excess spaces from text fields using Excel TRIM functions.
* Standardized text formatting across categorical fields.
* Converted mixed date formats into a consistent DD/MM/YYYY format.
* Replaced missing values:

  * Missing `region` values filled with **"Unknown"**
  * Missing `ship_mode` values filled with **"Unknown"**
  * Missing `discount` values filled with **0** when other sales-related fields were present.
* Created calculated fields:

  * `shipping_delay_days`
  * `profit_margin`
  * `data_quality_flag`
* Identified exact duplicate records and removed redundant duplicate rows.
* Retained conflicting duplicate order IDs and flagged them for manual review.
* Applied validation checks to identify:

  * Invalid discounts
  * Shipping dates occurring before order dates
  * Sales calculation mismatches
  * Profit calculation mismatches
  * Missing mandatory business attributes

---

## 3. Business Rules Applied

* Missing `region` values were replaced with **"Unknown"**.
* Missing `ship_mode` values were replaced with **"Unknown"**.
* Missing `discount` values were replaced with **0** when sales-related fields were otherwise complete.
* Discount values must fall within the valid range of **0 to 1**.
* Orders with status **Cancelled** were excluded from sales reporting.
* Orders with status **Payment Failed** were excluded from sales reporting.
* Returned orders were retained but identified separately for business analysis.
* Shipping dates must not occur before order dates.
* Duplicate records with identical values were removed.
* Duplicate order IDs containing conflicting information were retained and flagged for business review.

---

## 4. Assumptions Made

* Discount values are stored as decimal percentages between 0 and 1.
* Missing discount values represent no discount when other transaction data is complete.
* "Unknown" is an acceptable placeholder value for missing region and shipping mode fields.
* Date values could be standardized without loss of business meaning.
* Duplicate order IDs may represent legitimate business exceptions and therefore require manual review before deletion.
* Sales and profit calculations provided in the dataset are expected to reconcile with calculated values.

---

## 5. Records Removed

| Category                        | Count |
| ------------------------------- | ----- |
| Exact duplicate records removed | 39    |

A total of **39 records** were removed after confirming they were redundant exact duplicates.

---

## 6. Records Flagged

| Flag Type                            | Count |
| ------------------------------------ | ----- |
| Warning Records                      | 41    |
| Invalid Records                      | 88    |
| Records Flagged for Duplicate Review | 43    |
| Excluded Records                     | 176   |

### Warning Records

* Missing Region: 24 records
* Missing Ship Mode: 17 records

### Invalid Records

* Invalid discount values
* Shipping dates before order dates
* Sales calculation mismatches
* Profit calculation mismatches

### Excluded Records

* Cancelled orders
* Payment failed orders

Conflicting duplicate order IDs were retained and flagged for manual business review.

---

## 7. Final Data Quality Summary

| Metric            | Count |
| ----------------- | ----- |
| Total Raw Records | 932   |
| Clean Records     | 627   |
| Warning Records   | 41    |
| Excluded Records  | 176   |
| Invalid Records   | 88    |

---

## 8. Limitations

* Automated cleaning cannot determine the correct record when duplicate order IDs contain conflicting business information.
* Missing values filled with "Unknown" may still require investigation by business users.
* Discount values replaced with 0 are based on business assumptions and may not reflect actual discounts if source data was incomplete.
* Date validation only identifies logical inconsistencies and does not verify source-system accuracy.
* Sales and profit mismatch detection identifies discrepancies but does not determine root causes.
* Additional business validation may be required to confirm the legitimacy of flagged records before operational use.
