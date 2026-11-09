# Methodology, validation and limitations

## Evidence and validation scope

The published report draws on three kinds of evidence, which should not be confused:

| Evidence | What it establishes |
| --- | --- |
| Source-level checks against prepared CSVs | Transaction grain, missingness, sales totals, unique lookup keys and absence of join fanout |
| Power Query logic and metadata exported from Power BI Desktop | Recorded transformations, model relationships and DAX definitions; full executable M scripts and PBIX are not published |
| Selected functional tests and the final PDF | Specific filter-context results and the six-page static presentation, not a comprehensive interactive or security test |

See [cleaning](data-cleaning.md), [data model](data-model.md) and [metric definitions](metric-definitions.md) for the technical details.

## Published report and visual checks

[The six-page report](../reports/novamart-retail.pdf) has SHA-256:

`7dd58c819e2a3c12a9d0025264679383836bf5a8a282aad87aebae91e89102c7`

The six screenshots were rendered from the same final PDF at 2,400 × 1,388 pixels. They preserve its visual content without retouching metrics.

The page order is Executive Overview, Data Quality Overview, Discount Analysis, Product Performance, Customer Analysis and Channel & Payment Analysis.

The published export displays:

- **Sales 1,552,071**, **12,575 transactions**, **66,276 units** and **average ticket 129.65**.
- **Sales Metrics Availability 95.20%** and **604 incomplete sales transactions**; the visible availability label describes core sales metrics, not completeness across every source column.
- **609 recovered prices**, **1,213 recovered items** and **4,199 unknown discount statuses**.
- **25 Customers With Transactions** using a fact-based customer measure.
- **Top 10 Customers by Transactions (ties included)** with 11 visible customers, because three customers share the cutoff transaction count of 507.

## Selected Power BI functional checks

The final Customer Analysis card uses `Customers With Transactions = DISTINCTCOUNT(FactSales[Customer ID])`.

| Test context | Recorded result |
| --- | --- |
| Unfiltered | Customers With Transactions: 25 |
| September 16, 2022 | Customers With Transactions: 3 |
| March 17, 2023 | Customers With Transactions: 1; Total Transactions: 2; Valid Transactions: 1; Incomplete Transactions: 1; Total Sales: 230; Average Ticket: 230 |
| June 6, 2023 | Customers With Transactions: 1 |
| Unmatched product tested with `TREATAS` | `BLANK` for Total Transactions, Customers With Transactions, Total Sales and Average Ticket |

The March 17 test illustrates why average ticket divides sales by **valid transactions**, not all transactions: the report includes one complete and one incomplete record on that date. These checks are limited to their tested contexts.

## Interpretation and remaining limitations

- **Synthetic case study:** NovaMart is fictional, with only 25 customer IDs and 2 locations. Results do not represent a real business or establish commercial impact.
- **No specified currency:** Sales and ticket figures are presented without a currency symbol.
- **Partial month:** January 2025 contains records only through January 18; it should not be compared as a full month.
- **Incomplete sales:** 604 transactions have unknown sales and quantities. The published sales total measures recorded eligible sales and does not estimate missing revenue.
- **Discount coverage:** 4,199 transactions lack an original discount status. The 50.37% usage metric applies only to the 8,376 transactions with known status, not to all transactions; the analysis does not establish discount causality.
- **Access and reproducibility:** The public repository contains the PDF, previews and documentation, not the PBIX, raw source or complete M scripts. Row-level security (RLS) has not been functionally verified, and the PDF cannot reproduce Power BI interactions.
- **Visuals:** Some longer category labels remain truncated in the report; previews preserve the source layout.

[Source attribution and CC BY-SA 4.0 terms](data-source-and-license.md) apply to the dataset and its adaptations.

[Back to overview](../README.md)
