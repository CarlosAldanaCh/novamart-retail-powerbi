# Data cleaning and lookup integrity

## Original missingness

| Field | Missing source values | Treatment |
| --- | ---: | --- |
| Item | 1,213 | Recover from a unique category-price match |
| Price Per Unit | 609 | Recover as Total Spent / Quantity where both exist |
| Quantity | 604 | Remains unknown |
| Total Spent | 604 | Remains unknown |
| Discount Applied | 4,199 | Retain as unknown, separate from False |

All 604 missing Quantity values occur on the same records as the 604 missing Total Spent values. No other original fields are missing. Only 7,579 records have all 11 fields present before cleaning.

## User-supplied Power Query evidence

The project owner inspected the M code for `FactSales`, `FactSales_Working` and `item_lookup` and supplied the logic and model-validation results below. This update documents that evidence; it does not independently extract M from the PBIX. The flow is:

`fact_sales_raw.csv -> FactSales_Working -> FactSales`

`item_lookup` supports product recovery.

The documented Power Query lookup converts numeric fields, derives missing unit prices from Total Spent / Quantity, converts cleaned prices to fixed-decimal Currency.Type, selects Item, Category and cleaned price, filters item names and removes identical rows. FactSales_Working left-joins on Category and cleaned price and expands the matching item.

When the original price is missing, `Price Per Unit Clean = Total Spent / Quantity`; the cleaned price uses `Currency.Type`. The lookup keys are Category and Price Per Unit Clean. The owner-confirmed actual Power Query model has 200 unique keys, no ambiguous mappings and no null products in the final lookup. All 609 originally missing prices and all 1,213 originally missing items were recovered, with none unresolved among records marked as imputed. There were no division-by-zero cases or relevant rounding changes. The actual model join preserves 12,575 rows and 12,575 distinct Transaction IDs, with no fanout.

The inspected M logic is available as owner-supplied evidence, but complete verbatim query scripts are not included. No replacement M script or additional transformation steps are invented here.

## Transaction classification and discount handling

A record with missing Quantity **or** missing Total Spent is classified as `Incomplete Core Metrics`. Otherwise it is classified as `Valid for Sales Analysis`. This produces 604 incomplete and 11,971 valid transactions, or 95.20% essential sales metric availability. It does not measure completeness of all original fields. The final report now uses **Sales Metrics Availability %** as the visible label. The underlying **Data Completeness %** measure name and calculation are unchanged; Incomplete Transactions now displays as the integer **604**. These are report presentation corrections, not a new cleaning strategy.

Discount Applied is classified as Yes, No or Unknown. Unknown is retained separately. Discount Usage divides discounted transactions by transactions with known status, excluding Unknown from that denominator.

## Independent source validation

Read-only decimal arithmetic on the recovered CSV, performed separately during portfolio preparation, confirms:

- All 609 originally missing prices have usable sales and positive quantity. No zero denominator or missing-input case blocks these recoveries.
- All original and reconstructed prices are exactly representable at four decimal places. Currency.Type rounding therefore changes none of these source-derived lookup keys. This dataset requires no floating-point approximation to establish matching.
- Deduplicated nonblank Item-Category-price rows form 200 lookup rows and 200 distinct category-price keys. Every key has exactly one product; none of these lookup rows has a blank product.
- The source-based join expands to 12,575 rows with 12,575 unique transaction IDs. No source record has multiple matches or lacks a usable match.
- All 1,213 missing product names have one recovery. No nonmissing item disagrees with its matched product.
- Sales total 1,552,071 before and after this value-only lookup; no row fanout or invented sales is needed.

These independent checks agree with the owner-supplied validation of the actual Power Query model. They were not part of the original report implementation unless separately documented. They establish this source's integrity and corroborate the supplied model results, without proving future refresh behavior.

## Analytical choices

Preserve original missingness indicators for imputation reporting. Keep incomplete sales transactions in transaction counts, without inventing quantity or sales. Keep unknown discounts separate. The lookup is supported by this synthetic catalog's unique category-price combinations; repeat uniqueness and row-count checks if the catalog or pricing changes.

No source, transformation code, PBIX or metric value was modified while preparing this case study.

[Metric definitions](metric-definitions.md) | [Back to overview](../README.md)
