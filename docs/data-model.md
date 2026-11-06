# Data model

## Evidence provenance and grain

The relationship metadata below was supplied by the project owner from Power BI Desktop using `INFO.VIEW.RELATIONSHIPS()`. Measure definitions were supplied from `INFO.VIEW.MEASURES()`. They were not independently extracted from the PBIX during this documentation update. Earlier direct inspection of readable PBIX layout and diagram files established object and page presence; it is distinct from this newly supplied semantic-model evidence.

`FactSales` represents one source transaction per row. Independent source checks and owner-confirmed Power Query validation agree on 12,575 rows and 12,575 distinct Transaction IDs. This is not documented as a multi-line basket model.

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

The report owner supplied selected functional results: 25 customers unfiltered, 3 on September 16, 2022, 1 on March 17, 2023 (with 2 transactions), and 1 on June 6, 2023. This evidence supports the corrected card in those scenarios; it does not imply that every filter combination or relationship path has been tested. The relationship configuration is unchanged by this documentation integration.

## Recovered source dimensions and report sections

The supporting source files contain 25 customers, 200 products, 8 categories, 2 locations, 3 payment methods and 1,114 dates. The preparation package includes a separate category file; the inspected PBIX diagram does not show a separate DimCategory table. Source-file organization is distinct from the final model schema.

The six visible sections are Executive, Data Quality, Discounts, Products, Customers and Channel & Payment. Three sections are hidden: Customer Detail, Product Tooltip and Visual Calc Lab. Page presence is confirmed; the final PDF confirms the six main pages. The owner-supplied selected measure tests are distinct from the static export and do not establish every interactive path.

Exported cardinality and filter metadata do not prove correct results under every filter combination, working RLS, refresh behavior or all supporting model features.

[Metric definitions](metric-definitions.md) | [Back to overview](../README.md)
