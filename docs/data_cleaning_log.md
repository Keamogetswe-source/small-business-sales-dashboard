# Data Cleaning Log: Small Business Sales Dataset

**Tool:** Google Sheets
**Input:** `sales_data_messy.csv` (508 rows, 13 columns)
**Output:** `sales_data_clean.csv` (500 rows, 13 columns)

> The dataset is fictional (a South African small-business scenario). Data-quality problems were deliberately introduced so the cleaning process could be practised and documented.

## Method

For every problem I followed the same loop: **find it, fix it, check it.**

1. Never edit the raw data. Work on a copy (`working` tab); keep the original (`sales_data_messy` tab).
2. Detect problems with a count or a helper-column formula *before* changing anything.
3. Fix in a helper column, then paste the results back as **values only** so nothing depends on a formula.
4. Re-run the same check to prove the fix worked.

## Issues found and fixed

| # | Problem | How I found it | Fix | Check after fix |
|---|---|---|---|---|
| 1 | Duplicate orders | 508 rows but only 500 unique IDs (`COUNTA` vs `COUNTA(UNIQUE())`). Sorted by ID and confirmed each pair was identical. | Removed one copy of each of the 8 duplicate orders (Data > Data cleanup > Remove duplicates). | Rows = unique IDs = 500 |
| 2 | Branch capitalisation | Helper column `=IF(EXACT(C2,PROPER(TRIM(C2))),"OK","CHECK")` flagged 8 rows (`TEMBISA`, `centurion`, ...). | `=PROPER(TRIM(C2))`, pasted back as values. | 0 flagged rows; 5 unique branches |
| 3 | Category spelling (`Grocery`, `House Hold`) | `=UNIQUE(E2:E501)` returned 5 names instead of 3. | `SUBSTITUTE` to map `Grocery` to `Groceries` and `House Hold` to `Household`. | 3 unique categories |
| 4 | Blank Quantity (4 cells) | `COUNTBLANK` per column. | Recovered as Sales / Unit_Price (all whole numbers). | 0 blanks in Quantity |
| 5 | Blank Customer_Type (3) and Payment_Method (5) | `COUNTBLANK` per column (12 blanks in total). | Not recoverable from other columns, so labelled `Unknown`. | 0 blanks in both columns |
| 6 | Negative Quantity (3 rows) | `COUNTIF(G2:G501,"<0")`. | Sales and Unit_Price were consistent on these rows, so Quantity = Sales / Unit_Price. | 0 negative quantities |
| 7 | Impossible dates (2 rows in 2062) | `MAX(B2:B501)` returned 2062-09-06; `COUNTIF` for dates after 2026 returned 2. | Assumed a swapped-digit typo (2062 to 2026); month and day unchanged. | Latest date is 2026-09-30 |
| 8 | Sales stored as text (6 cells, e.g. `R230.00`) | `COUNT(J2:J501)` returned 494, not 500. | `=VALUE(SUBSTITUTE(J2,"R",""))`, pasted back as values. | 500 numeric Sales values |
| 9 | Profit not equal to Sales - Cost (6 rows) | `=IF(ROUND(J2-K2,2)=ROUND(L2,2),"OK","ERROR")` returned 6 errors. Sales = Quantity x Unit_Price and Cost = Quantity x Unit_Cost held on the rows I inspected, so Profit was the wrong value. | `=ROUND(J2-K2,2)`, pasted back as values. | 0 errors |

Mixed date formats (for example `26/04/2026` and `5 February 2026`) were converted to real dates by Google Sheets on import (`COUNT` on the date column returned 500). The date column was then formatted as `yyyy-mm-dd` before export.

## Decisions and assumptions

- **`Unknown` labels:** 8 blank cells (3 Customer_Type, 5 Payment_Method) cannot be derived from other columns, so I labelled them `Unknown` instead of guessing.
- **Year typo fix:** the two 2062 dates were assumed to be typos of 2026. In a real project I would confirm this with the data owner.
- **Large orders kept and flagged, not changed:** two Individual-customer orders have unusually large quantities (typical orders are 1 to 15 items). Individuals can buy in bulk, and nothing proves these are errors, so they are unchanged. For the team to review:
  - `ORD0291`: Laundry Soap, quantity 300, Sales R10,080
  - `ORD0390`: Bath Soap, quantity 200, Sales R5,600
  - Reports should show totals with and without these two orders.

## Final validation

A single formula confirmed all fixes together and returned **ALL CHECKS PASS**: 500 unique Order_IDs, 0 blank cells, 5 branches, 3 categories, no dates after 2026, 500 numeric Sales values, 0 profit errors.
