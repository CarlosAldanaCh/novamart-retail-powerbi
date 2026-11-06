# Methodology, evidence and limitations

## Evidence register

Final integration review: November 6, 2026.

| Evidence layer | What it supports | Boundary |
| --- | --- | --- |
| Source-level validation: recovered CSV and independent portfolio checks | Grain, unique IDs, missingness, totals, denominators and decimal-exact lookup reconstruction | Separate from the original Power BI implementation |
| Source preparation brief and dimension CSVs | Fact/dimension separation and source dimension counts | Final relationships are described by the supplied model metadata |
| Power Query: owner-inspected FactSales, FactSales_Working and item_lookup M logic | Recovery, classification, discount handling, 200 unique keys, complete supported imputations and no actual-model fanout | Owner-supplied evidence; complete verbatim scripts are not included |
| Semantic-model metadata: owner-exported INFO.VIEW.MEASURES() and INFO.VIEW.RELATIONSHIPS() | Eight original DAX definitions, five primary relationships and two supporting relationships | Not independently extracted in this integration; not proof of every filter combination or RLS |
| Final validation supplied by the report owner | New fact-based customer measure, selected date tests, valid-ticket denominator test and unmatched-product TREATAS test | Tests were not independently rerun; only supplied scenarios are claimed |
| Final PDF and direct page renders | Six ordered pages, global totals, corrected labels/formatting and explicit ranking ties | Static view cannot expose every hidden filter or reproduce interactions |

The model evidence comes from the report owner, distinct from the independent source and file checks performed during portfolio preparation. Definitions are recorded in [metric definitions](metric-definitions.md), relationship settings in [data model](data-model.md) and transformation logic in [data cleaning](data-cleaning.md). No independent verification script is claimed as part of the original Power BI implementation.

## Canonical final export

The supplied `NovaMartRetail_Final.pdf` was inspected before replacement and copied unchanged to [the report PDF](../reports/novamart-retail.pdf).

**SHA-256:** `fd17d6c42fed5adc3325ea5519e338e2f68e808836531e66b045f5ccc9d6b161`

All six screenshots are full-page, lossless WebP renders of this same final export at 2,400 x 1,388 pixels. The PDF page size is 996 x 576 points; rendering retains the full page and aspect ratio subject to integer-pixel rounding. No values, labels or visual elements were retouched. Earlier exports are no longer the canonical assets. This is the final export supplied after the owner's prioritized report corrections, replacing the older snapshot.

The verified page order is Executive Overview, Data Quality Overview, Discount Analysis, Product Performance, Customer Analysis, and Channel & Payment Analysis. Global cards reconcile to sales 1,552,071, transactions 12,575, units 66,276 and average ticket 129.65 where those cards appear. The customer page displays 25 Customers With Transactions. Discount usage remains 50.37% among 8,376 known statuses, with 4,219 discounted transactions.

No reduced global totals, accidental visible testing selections or cross-highlighting were observed in the six pages. This is a visual and KPI consistency check of the export, not an independent inspection of every hidden model/report filter.

## Completed prioritized checks and presentation corrections

| Item | Final evidence | Status and scope |
| --- | --- | --- |
| Availability label | PDF displays Sales Metrics Availability % at 95.20% | Visible label corrected; underlying Data Completeness % name/calculation unchanged |
| Incomplete Transactions | PDF displays 604 | Integer formatting confirmed |
| Imputation and unknown discount cards | PDF displays 609 prices, 1,213 items and 4,199 unknown discounts | Values preserved |
| Customer card | Customers With Transactions displays 25 | Uses the new fact-based measure; original dimension-count measure remains |
| Customer transaction ranking | Top 10 Customers by Transactions (ties included) | 11 customers shown because three share the cutoff count of 507; tie explanation now explicit |
| Canonical assets | PDF copy and all six previews derive from the final source | Earlier PDF version mismatch resolved for this case-study package |

## Owner-supplied functional validation

The corrected measure is `Customers With Transactions = DISTINCTCOUNT(FactSales[Customer ID])`. The following results were supplied by the report owner and were not independently rerun during integration:

| Context | Reported result |
| --- | --- |
| Unfiltered | Customers With Transactions: 25 |
| September 16, 2022 | Customers With Transactions: 3 |
| March 17, 2023 | Customers With Transactions: 1; Total Transactions: 2; Valid Transactions: 1; Incomplete Transactions: 1; Total Sales: 230; Average Ticket: 230 |
| June 6, 2023 | Customers With Transactions: 1 |
| Unmatched product selection through a DAX TREATAS query | BLANK for Total Transactions, Customers With Transactions, Total Sales and Average Ticket |

The March 17 test verifies that Average Ticket uses the valid-sales denominator in that tested date context. The unmatched-product test records BLANK, not an invented zero result. These selected scenarios do not establish every possible slicer interaction, filter combination, security context or empty selection.

## Remaining limitations

- NovaMart is fictional and the data is synthetic, with a small catalog and 25 customer identifiers. No real client engagement or measured commercial impact is claimed.
- The source does not specify a currency. Sales and ticket are presented without a currency symbol.
- January 2025 includes data only through January 18. Keep the partial-month notice and avoid full-month comparisons.
- Recorded sales exclude unknown amounts on 604 transactions; they are not an estimate of missing revenue.
- Discount status is unknown for 4,199 transactions. Known-status comparisons have a restricted population and do not establish a causal discount effect.
- RLS has not been functionally verified. The UserAccess relationship and bidirectional security-filter configuration do not prove correct role definitions or working security.
- The static PDF does not reproduce Power BI slicers, tooltips, drillthrough or other interactions. Functional validation covers selected scenarios only.
- Complete M scripts, remaining measure expressions, role definitions and refresh settings are not included; the package is not a fully reproducible semantic-model release.
- Some long category labels remain truncated in the source report visuals. Screenshots preserve this content without concealment.
- Source attribution and CC BY-SA 4.0 obligations remain applicable; see [source and license](data-source-and-license.md).

## Publication readiness and release boundary

**READY for publication review as a static portfolio case study**, with the limitations above disclosed. No known blocker remains in the prioritized PDF and documentation synchronization scope. This is not a claim that all model functions or RLS have been validated.

After approval: review the exact file list, initialize or use the intended repository, commit only the documentation and final public assets, create the public repository, push, verify its rendered README/PDF/images, then separately authorize portfolio integration. Do not include raw CSV, ZIP or PBIX files. No publication action is performed by this integration task.

[Back to overview](../README.md)
