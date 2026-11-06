# NovaMart Retail | Sales Analytics with Power BI

A six-page sales report that connects retail performance with the quality of the data behind it. NovaMart is a fictional retailer and the dataset is synthetic. This portfolio case study demonstrates an analytical deliverable, not client work or measured commercial impact.

## Business problem

A retail analyst needs a consistent view of sales, products, customers, channels and payment methods. Missing product, price and sales fields can distort that view. The task was to recover supported product information, preserve transaction counts and make the populations behind each KPI clear.

## Delivered report

The Power BI report covers executive performance, data quality, discounts, products, customers, and channel/payment analysis. The final PDF and all six previews come from one export, with global totals restored and the prioritized presentation corrections applied.

[View the six-page PDF](reports/novamart-retail.pdf)

![Executive overview](screenshots/01-executive-overview.webp)

| Page | Analytical purpose | Preview |
| --- | --- | --- |
| Executive Overview | Sales, transactions, units and time trend | [View](screenshots/01-executive-overview.webp) |
| Data Quality Overview | Essential metric availability and imputation | [View](screenshots/02-data-quality.webp) |
| Discount Analysis | Known discount status and usage denominator | [View](screenshots/03-discount-analysis.webp) |
| Product Performance | Product sales, units and transactions | [View](screenshots/04-product-performance.webp) |
| Customer Analysis | Customers with transactions and activity rankings | [View](screenshots/05-customer-analysis.webp) |
| Channel & Payment | Channel and payment-method breakdowns | [View](screenshots/06-channel-payment.webp) |

This is a Power BI report, not a Power BI Service dashboard. The static PDF does not reproduce interactive filtering, tooltips or drillthrough. The source model also contains three hidden supporting pages, outside the six-page export.

## Technical approach

**Tools:** Power BI Desktop, Power Query and DAX, using recovered CSV fact and dimension tables.

- Recovered 609 missing unit prices from recorded sales divided by quantity.
- Recovered 1,213 missing item names with a validated category-price lookup containing 200 unique keys.
- Preserved 12,575 rows and distinct transaction IDs through the join, with no fanout.
- Kept 604 transactions with incomplete essential sales metrics identifiable, without inventing their sales amounts.
- Used five active dimensional relationships with single-direction filtering toward FactSales.
- Added Customers With Transactions on the fact table for the Customer Analysis card; retained the original dimension-count measure.

Read-only Python checks were performed separately during portfolio preparation to corroborate source totals and lookup integrity. They are not presented as part of the original Power BI implementation. Supplied model definitions and functional-test results are attributed in the technical documentation.

## Verified findings and analytical decisions

| Measure | Value | Interpretation |
| --- | ---: | --- |
| Total transactions | 12,575 | Includes incomplete sales records |
| Valid sales transactions | 11,971 | Both Quantity and Total Spent are present |
| Sales Metrics Availability % | 95.20% | 11,971 / 12,575; not all-field completeness |
| Total sales | 1,552,071 | Recorded sales for valid transactions; currency unspecified |
| Average ticket | 129.65 | 1,552,071 / 11,971 valid transactions |
| Customers With Transactions | 25 | Distinct FactSales customer IDs at full-data scope |
| Discounted transactions | 4,219 | Known Yes discount status |
| Known discount transactions | 8,376 | Yes or No, excluding Unknown |
| Unknown discount transactions | 4,199 | Missing original discount status |
| Discount usage | 50.37% | 4,219 / 8,376 known statuses |

Only 7,579 source records (60.27%) contain all 11 original fields. Price and product recovery improves usability without making missing quantity or sales known. Transaction counts retain incomplete records; average ticket uses valid sales transactions. Unknown discounts remain separate from No.

The report owner validated customer counting on selected dates. On March 17, 2023, the model returned two transactions, one valid and one incomplete, with sales and average ticket both 230. This supports the valid-transaction denominator in that tested context. An unmatched-product DAX test returned BLANK for the four tested measures; it does not establish every slicer behavior. See the [test record](docs/methodology-and-limitations.md).

The customer transaction ranking explicitly includes ties: three customers share the cutoff count of 507, so the Top 10 visual contains 11 customers.

## Data source

[Retail Store Sales: Dirty for Data Cleaning](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning), by Ahmed Mohamed, is synthetic data licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The recovered preparation package separates the source into fact and dimension tables before cleaning.

The fact table has one transaction per row and dates from January 1, 2022 through January 18, 2025. Raw data and the PBIX are excluded from this publication package. [Source, adaptations and license](docs/data-source-and-license.md) records attribution and ShareAlike obligations.

## Technical documentation and limitations

- [Data model](docs/data-model.md): business star schema, supporting relationships and customer-count correction.
- [Metric definitions](docs/metric-definitions.md): supplied DAX, eligibility rules and denominators.
- [Data cleaning](docs/data-cleaning.md): original missingness, Power Query logic and integrity checks.
- [Methodology and limitations](docs/methodology-and-limitations.md): evidence provenance, final-export hash and selected functional tests.

The final export labels availability as Sales Metrics Availability %, formats Incomplete Transactions as 604 and explicitly labels customer ranking ties. The underlying Data Completeness % measure name and calculation are unchanged.

The source specifies no currency, and January 2025 is partial through January 18. RLS has not been functionally verified. Functional tests covered selected scenarios, not every filter combination. The static files are not a fully reproducible model release. Synthetic descriptive results cannot establish real customer behavior, discount causality or commercial impact.

This version is prepared locally for publication review; no public repository or interactive report is claimed here.
