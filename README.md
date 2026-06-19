# Part 1: Business Data Cleaning, Validation & Excel Reporting

## Problem Summary
A retail company exported order-level sales data from multiple systems. The dataset contained missing values, duplicate records, inconsistent date formats, invalid discount values, shipping date anomalies, and order status records that required business-rule-based treatment. This project focused on cleaning, validating, and standardizing the data to create an analysis-ready dataset and generate business reporting through Excel Pivot Tables.

## Dataset
- **raw_orders.xlsx** – Original unmodified dataset containing 932 order records.
- **cleaned_orders.xlsx** – Cleaned and validated dataset with calculated fields, data quality flags, and standardized formats.

## Tools Used
- Microsoft Excel
  - TRIM
  - PROPER
  - COUNTIF / COUNTIFS
  - COUNTBLANK
  - IF / AND / OR validation logic
  - Date functions
  - Pivot Tables
  - Conditional Formatting

---

## Cleaning Steps Performed

1. Trimmed leading and trailing spaces from text fields.
2. Standardized categorical text values and formatting.
3. Converted mixed date formats into a consistent DD/MM/YYYY format.
4. Identified and removed exact duplicate records.
5. Flagged duplicate Order IDs containing conflicting information for manual review.
6. Replaced missing Region values with "Unknown".
7. Replaced missing Ship Mode values with "Unknown".
8. Replaced missing Discount values with 0 where other sales-related fields were populated.
9. Created calculated fields:
   - calculated_sales
   - calculated_profit
   - profit_margin
   - shipping_delay_days
   - data_quality_flag
10. Applied business validation checks to identify:
    - Invalid discounts
    - Shipping dates before order dates
    - Sales calculation mismatches
    - Profit calculation mismatches
    - Missing mandatory fields

---

## Business Rules Applied

- Missing `region` values were replaced with **"Unknown"**.
- Missing `ship_mode` values were replaced with **"Unknown"**.
- Missing `discount` values were replaced with **0** when sales information was available.
- Discount values must fall between **0 and 1**.
- Orders with status **Cancelled** were excluded from sales reporting.
- Orders with status **Payment Failed** were excluded from sales reporting.
- Returned orders were retained for operational analysis but excluded from performance reporting where applicable.
- Shipping dates cannot occur before order dates.
- Exact duplicate records were removed.
- Duplicate Order IDs with conflicting information were retained and flagged for manual review.

---

## Data Quality Issues Found

### Missing Values

| Column | Missing Count | % Missing |
|----------|--------------|------------|
| Region | 26 | 2.79% |
| Ship Mode | 22 | 2.36% |
| Discount | 18 | 1.93% |

### Duplicate Records

| Issue | Count |
|---------|---------|
| Exact Duplicate Rows | 40 |
| Duplicate Order IDs | 63 |
| Records Removed | 39 |
| Records Flagged for Review | 43 |

### Validation Issues

| Issue | Count |
|---------|---------|
| Negative Discount Values | 16 |
| Invalid Shipping Records (Ship Date < Order Date) | 95 |
| Sales Calculation Mismatches | 6 |
| Profit Calculation Mismatches | 6 |

### Order Status Summary

| Status | Count |
|---------|---------|
| Completed | 622 |
| Returned | 164 |
| Cancelled | 146 |
| Payment Failed | 69 |

---

## Pivot Report Summary

### Sales & Profit by Region

| Region | Sales | Profit |
|---------|---------:|---------:|
| South | 2,296,951 | 603,334 |
| East | 2,216,584 | 674,863 |
| West | 2,116,899 | 594,440 |
| North | 2,075,272 | 600,993 |
| Unknown | 274,547 | 87,970 |

**Highest Sales Region:** South ($2.30M)

**Highest Profit Region:** East ($674.9K)

---

### Sales & Profit by Category

| Category | Sales | Profit |
|-----------|---------:|---------:|
| Technology | 3,123,588 | 887,426 |
| Furniture | 3,111,986 | 895,142 |
| Office Supplies | 2,744,680 | 779,032 |

**Highest Revenue Category:** Technology ($3.12M)

**Highest Profit Category:** Furniture ($895K)

---

### Monthly Sales Trend

| Year | Total Sales |
|---------|---------:|
| 2024 | 4,737,570 |
| 2025 | 4,242,684 |

**Strongest Months:**
- February 2024: $562,150
- February 2025: $598,551
- August 2025: $567,627

---

### Order Count by Ship Mode

| Ship Mode | Orders |
|------------|---------:|
| Standard Class | 246 |
| Second Class | 227 |
| First Class | 226 |
| Same Day | 211 |
| Unknown | 21 |

**Most Frequently Used Shipping Method:** Standard Class

---

### Profit Margin by Customer Segment

| Segment | Average Profit Margin |
|------------|---------:|
| Small Business | 29.40% |
| Home Office | 28.38% |
| Corporate | 26.31% |
| Consumer | 24.51% |

**Highest Margin Segment:** Small Business

---

## Key Business Insights

- South generated the highest sales revenue ($2.30M), while East delivered the highest overall profit ($674.9K).
- Technology generated the largest revenue contribution, while Furniture produced the highest total profit.
- Standard Class shipping accounted for the highest order volume, indicating customer preference for lower-cost delivery options.
- Small Business customers generated the highest average profit margin (29.4%), making them the most profitable customer segment.
- Sales remained relatively stable across both years, with February consistently being one of the strongest-performing months.

---

## Assumptions & Limitations

### Assumptions

- Discount values are stored as decimals between 0 and 1.
- Missing discount values represent no discount when all other transaction data is present.
- "Unknown" is an acceptable placeholder for missing Region and Ship Mode values.
- Duplicate Order IDs may represent legitimate business scenarios and therefore require manual review.

### Limitations

- Automated cleaning cannot determine the correct version of conflicting duplicate Order ID records.
- Missing values replaced with "Unknown" may still require business investigation.
- Date validation identifies logical inconsistencies but cannot verify source-system accuracy.
- Sales and profit mismatch checks identify discrepancies but do not determine the root cause.
- Some flagged records may require additional business context before final resolution.

---

## Final Dataset Summary

| Metric | Count |
|---------|---------:|
| Total Raw Records | 932 |
| Clean Records | 659 |
| Warning Records | 41 |
| Excluded Records | 176 |
| Invalid Records | 88 |

> Note: Warning, Invalid, and Excluded categories are not mutually exclusive. A record may belong to more than one classification.

---

## Screenshots

- screenshots/raw_data_preview.png
- screenshots/cleaned_data_preview.png
- screenshots/pivot_summary_1.png
- screenshots/pivot_summary_2.png