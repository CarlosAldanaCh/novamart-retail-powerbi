# Data model

## Grain and model metadata

The documented relationship and measure metadata was exported from Power BI Desktop using `INFO.VIEW.RELATIONSHIPS()` and `INFO.VIEW.MEASURES()`. The PBIX is not distributed in this repository, so readers cannot inspect the semantic model directly.

`FactSales` represents one transaction per row. Source checks and Power Query validation recorded 12,575 rows and 12,575 distinct Transaction IDs. This is not a multi-line basket model.

## Business star schema: exported relationships

All five relationships are **active**, with **many-to-one cardinality from FactSales toward the dimension**. Filtering is **single-direction from the dimension to FactSales**. The endpoint order below describes cardinality, not the direction of filter propagation.

| Many-side endpoint | One-side endpoint | Filter propagation |
| --- | --- | --- |
| FactSales[Transaction Date] | DimDate[Date] | DimDate to FactSales |
| FactSales[Item] | DimProduct[Item] | DimProduct to FactSales |
| FactSales[Location] | DimLocation[Location] | DimLocation to FactSales |
| FactSales[Customer ID] | DimCustomer[Customer ID] | DimCustomer to FactSales |
| FactSales[Payment Method] | DimPaymentMethod[Payment Method] | DimPaymentMethod to FactSales |

These relationships support date, product/category, channel, customer and payment-method analysis. The exported date endpoint is `DimDate[Date]`; it should not be confused with the source CSV's `Transaction Date` column name.

## Supporting model relationships

| Many-side endpoint | One-side endpoint | Active | Filter configuration |
| --- | --- | --- | --- |
| DimDate[Date] | LocalDateTable[Date] | Yes | Single-direction filtering |
| UserAccess[Location] | DimLocation[Location] | Yes | Bidirectional filtering, with bidirectional security-filter configuration |

These are supporting relationships, separate from the five business dimensions around FactSales. The access relationship's security-filter setting does **not** prove that RLS roles exist, are correct or have been tested. Role definitions and role-based functional tests are still required before claiming verified RLS functionality. The diagram also includes `_Measures` and `Time Intelligence`; their names do not establish tested calculation-group or time-intelligence behavior.

## Customer measure correction and tested behavior

The original `Total Customers = DISTINCTCOUNT(DimCustomer[Customer ID])` remains in the semantic model. Because the primary relationship filters from DimCustomer toward FactSales, a date or product selection need not reduce the customer dimension. A dimension count and a count of customers with transactions answer different questions.

The final Customer Analysis card now uses the new measure:

```dax
Customers With Transactions =
    DISTINCTCOUNT(FactSales[Customer ID])
```

Selected Power BI functional tests recorded: 25 customers unfiltered, 3 on September 16, 2022, 1 on March 17, 2023 (with 2 transactions), and 1 on June 6, 2023. These tests support the corrected card in the listed contexts; the underlying relationship configuration did not change.

## Recovered source dimensions and report sections

The supporting source files contain 25 customers, 200 products, 8 categories, 2 locations, 3 payment methods and 1,114 dates. The preparation package includes a separate category file; the inspected PBIX diagram does not show a separate DimCategory table. Source-file organization is distinct from the final model schema.

The six visible sections are Executive, Data Quality, Discounts, Products, Customers and Channel & Payment. Three sections are hidden: Customer Detail, Product Tooltip and Visual Calc Lab. The final PDF shows the six main pages; selected measure checks are documented separately from the static export.

[Metric definitions](metric-definitions.md) | [Back to overview](../README.md)
