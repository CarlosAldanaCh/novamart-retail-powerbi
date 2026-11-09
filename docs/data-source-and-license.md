# Data source and license

## Attribution

- Original dataset: **Retail Store Sales: Dirty for Data Cleaning**.
- Author: **Ahmed Mohamed**.
- Source: [Kaggle dataset page](https://www.kaggle.com/datasets/ahmedmohamed2003/retail-store-sales-dirty-for-data-cleaning).
- License: [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/).
- Nature: synthetic retail transactions. NovaMart is a fictional portfolio scenario, not the original author's client or an actual engagement.

## Adaptations and provenance

The source preparation files describe splitting `retail_store_sales.csv` into a transaction fact table and customer, product, category, date, location and payment-method dimension files, initially preserving dirty values. Subsequent Power Query preparation reconstructs supported unit prices, fills missing product names through a category-price lookup and distinguishes incomplete sales records. The report and this case study add derived metrics, aggregations and visualization.

The prepared fact file has 12,575 rows and 11 original fields. Its SHA-256 is `28a150de76b9252e79f73e5044630127adb7f4cafdcc94c998ebd64d943819cb`. The local package's brief and source attribution support provenance; a byte-for-byte comparison with the original Kaggle download is not available in this release.

## Reuse and distribution boundary

CC BY-SA 4.0 requires appropriate credit, a license link and an indication of changes. Adapted dataset material that is redistributed must remain under the same license, without additional restrictions inconsistent with that license. The original creator does not endorse this project.

This package intentionally excludes all raw CSV files, the original archive and the PBIX. It contains a report export, derived screenshots and explanatory documentation. Attribution above also accompanies the data-derived report material through this repository. Preserve it and the adaptation notice when sharing. The dataset license is not presented as a blanket license for unrelated third-party assets or software.

Before any later data or PBIX release, confirm the scope of embedded data and assets, retain this attribution and provide the adapted dataset under CC BY-SA 4.0. The public repository currently does not include the source CSVs or PBIX.

[Back to overview](../README.md)
