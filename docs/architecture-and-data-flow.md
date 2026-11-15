# Architecture and data flow

## Layers

| Layer | Role | Evidence in supplied files |
|---|---|---|
| Source | Parquet input files | README describes customers, products, orders, regions |
| Bronze | Raw landing zone | Auto Loader notebook reads source and writes append-mode Parquet |
| Silver | Cleansed/conformed Delta datasets | Four Silver notebooks/scripts |
| Gold | Analytical dimensions and fact | Gold customer/product/order notebooks/scripts |
| Consumption | SQL/BI | PBIDS connection template; no PBIX report supplied |

## Bronze streaming pattern

The Bronze notebook uses Auto Loader (`cloudFiles`) to discover Parquet files. It configures a schema location and a checkpoint location, then writes to a Bronze path in append mode. The original trigger is `once=True`. Review the supported trigger semantics for the selected Databricks Runtime and consider `availableNow=True` for bounded file processing.

Schema tracking and stream checkpointing have different responsibilities. Keep their paths separate and persistent between runs. Do not delete checkpoints casually: doing so can cause previously processed files to be discovered again.

## Silver transformations

- Customers: email domain extraction, domain counts, and full-name derivation.
- Products: discounted price and uppercase brand UDF examples.
- Orders: timestamp conversion, date attributes, and ranking examples.
- Regions: Bronze table read, rescued-data removal, and Delta write.

## Gold transformations

The repository intends to create customer/product dimensions and an orders fact table. The actual implementation has gaps described in `scd-type2-explanation.md` and `notebook-review.md`. Do not state that a fully correct SCD Type 2 or production CDC pipeline has been delivered.

## Evidence boundaries

The inherited README describes ADF, Synapse, and Power BI. The ZIP does not contain deployable ADF JSON, Synapse SQL scripts, or a Power BI PBIX. The only Power BI artifact is a Databricks SQL Warehouse PBIDS connection definition. Therefore the complete architecture cannot be redeployed from this repository alone.
