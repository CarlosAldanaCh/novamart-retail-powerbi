# Metric definitions

The nine DAX definitions below were recorded from Power BI Desktop using `INFO.VIEW.MEASURES()`, including the later Customers With Transactions measure. The table also summarizes source-level reconciliation checks. The public repository does not include the PBIX for independent inspection.

| Metric | Definition and denominator | Full-data value |
| --- | --- | ---: |
| Total Transactions | Count of source transactions; distinct IDs agree | 12,575 |
| Valid sales transactions | Both Quantity and Total Spent present | 11,971 |
| Incomplete Transactions | Missing Quantity or Total Spent; both missing on the same 604 source rows | 604 |
| Sales Metrics Availability % (visible label) | Existing Data Completeness % measure: valid sales transactions / all transactions | 95.20% |
| Incomplete transaction rate | Incomplete / all transactions | 4.80% |
| Total Sales | Sum of Total Spent restricted by the supplied DAX to valid sales records | 1,552,071 |
| Units Sold | Sum of recorded Quantity | 66,276 |
| Average Ticket | Total Sales / valid sales transactions | 129.65 |
| Discounted Transactions | Discount Applied is True | 4,219 |
| Known Discount Transactions | Discount Applied is True or False | 8,376 |
| Unknown Discount Transactions | Discount Applied is blank | 4,199 |
| Discount Usage | Discounted / known discount transactions | 50.37% |
| Unknown discount rate | Unknown / all transactions | 33.39% |
| Imputed Price Transactions | Originally missing price recovered from sales / quantity | 609 (4.84% of all) |
| Imputed Item Transactions | Originally missing item recovered with a unique lookup | 1,213 (9.65% of all) |
| Customers With Transactions | DISTINCTCOUNT of FactSales[Customer ID]; used by the final Customer Analysis card | 25 |
| Total Customers (retained original measure) | DISTINCTCOUNT of DimCustomer[Customer ID]; not used by the final customer card | 25 |
| All-field source completeness | Rows containing all 11 original fields / all rows | 7,579 / 12,575 = 60.27% |

## Supplied DAX definitions

```dax
Total Transactions =
COUNTROWS(FactSales)
```

```dax
Valid Transactions =
CALCULATE(
    [Total Transactions],
    FactSales[Record Status] = "Valid for Sales Analysis"
)
```

```dax
Incomplete Transactions =
CALCULATE(
    [Total Transactions],
    FactSales[Record Status] = "Incomplete Core Metrics"
)
```

```dax
Total Sales =
CALCULATE(
    SUM(FactSales[Total Spent]),
    FactSales[Record Status] = "Valid For Sales Analysis"
)
```

```dax
Average Ticket =
DIVIDE(
    [Total Sales],
    [Valid Transactions]
)
```

```dax
Data Completeness % =
DIVIDE(
    [Valid Transactions],
    [Total Transactions]
)
```

```dax
Discount Usage % =
DIVIDE(
    [Discounted Transactions],
    [Known Discount Transactions]
)
```

```dax
Total Customers =
DISTINCTCOUNT(DimCustomer[Customer ID])
```

```dax
Customers With Transactions =
    DISTINCTCOUNT(FactSales[Customer ID])
```

The original Total Customers measure remains in the semantic model. The final Customer Analysis card uses Customers With Transactions instead.

The record-status literals above preserve the supplied capitalization: `Valid for Sales Analysis` in Valid Transactions and `Valid For Sales Analysis` in Total Sales. Neither the expressions nor the PBIX were changed.

The report also uses Discounted Transactions, Unknown Discount Transactions, Known Discount Transactions, Imputed Price Transactions, Imputed Item Transactions, Incomplete Transaction Rate, Unknown Discount Rate, Units Sold and Category Sales Rank. Their full DAX expressions are not included in this documentation.

## Interpretation

The final PDF uses **Sales Metrics Availability %** for the existing **Data Completeness %** measure. Its name and calculation in the model are unchanged. The label means Quantity and Total Spent are available; it does not mean that 95.20% of rows contain all original fields. Source completeness and successful imputation remain different measures. Incomplete Transactions now displays as the integer **604**.

Average Ticket uses 11,971 valid sales transactions, not 12,575 total transactions. Its unrounded value is approximately 129.652577. Within each report context, sales and the denominator should refer to the same eligible records.

The Discount Analysis page's usage denominator excludes 4,199 unknown statuses. Unknown is neither No nor False. Sales and ticket comparisons for Yes and No do not cover the whole dataset. These descriptive comparisons cannot establish a causal discount effect.

There is no specified currency, so sales and ticket are reported without a currency symbol. Imputation rates use original missingness flags and all source transactions; they are not counts of still-missing fields after cleaning.

## Selected functional validation

Selected Power BI functional tests recorded:

| Context | Customers With Transactions | Other reported results |
| --- | ---: | --- |
| Unfiltered | 25 | Final PDF also displays this count |
| September 16, 2022 | 3 | No additional values supplied |
| March 17, 2023 | 1 | Total Transactions 2; Valid Transactions 1; Incomplete Transactions 1; Total Sales 230; Average Ticket 230 |
| June 6, 2023 | 1 | No additional values supplied |

The March 17 result supports the Average Ticket denominator of one valid transaction in that context, despite two total transactions.

A DAX query applying an unmatched product selection through `TREATAS` returned **BLANK** for Total Transactions, Customers With Transactions, Total Sales and Average Ticket. The test records the result of that query; the exact query expression is not included.

## Filter behavior

The original Total Customers counts the customer dimension. With single-direction relationships from dimensions to FactSales, date or product filters can constrain the fact population without reducing DimCustomer. The final card addresses the customer-count purpose by using Customers With Transactions on FactSales; it does not modify the original measure or relationship directions.

The final ranking title is **Top 10 Customers by Transactions (ties included)**. The visual retains 11 customers because three share the cutoff count of 507. Exact ranking expressions were not supplied and are not invented here.

These are selected test contexts, not comprehensive coverage of combined filters or report interactions. RLS has not been functionally verified.

[Methodology and limitations](methodology-and-limitations.md) | [Back to overview](../README.md)
