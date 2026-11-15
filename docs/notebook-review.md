# Notebook and script review

## Inventory

| File | Observed purpose |
|---|---|
| `Parameters.ipynb` | Supplies the `orders`, `customers`, and `products` task-value list |
| `Bronze_Layer.ipynb` | Auto Loader reads Parquet and writes Bronze files with checkpointing |
| `Silver_Customers.ipynb` | Email-domain derivation, domain counts, full-name creation, Delta output |
| `Silver_Products.ipynb` | SQL/Python UDF examples, discounted price, uppercase brand, Delta output |
| `Silver_Orders.py` | Timestamp/date attributes and rank/dense_rank/row_number examples |
| `Silver_Regions.ipynb` | Reads Regions, drops `_rescued_data`, writes Delta |
| `Gold_Customers.ipynb` | Surrogate key and customer dimension merge attempt |
| `Gold_Products.ipynb` | DLT expectations, streaming table/view and SCD Type 2 change application |
| `Gold_Orders.py` | Dimension joins and Delta MERGE for fact orders |

## Changes made in this learning copy

- Removed `.ipynb_checkpoints` files from the repository.
- Cleared saved notebook outputs and execution counts.
- Parameterized the hardcoded storage-account name with a `storage_account` widget in notebooks/scripts where paths are used.
- Replaced the PBIDS workspace host and SQL warehouse path with placeholders.
- Fixed the Gold customer notebook's unassigned `dropDuplicates()` call by assigning the returned DataFrame.
- Added warnings about incomplete SCD Type 2 and change sequencing logic.
- Added repository documentation and security/publishing guidance.

## Known issues requiring runtime validation

- The notebooks use a mix of classic Delta Live Tables APIs and Spark/Databricks APIs. Confirm compatibility with the target runtime.
- Gold customer SCD Type 2 logic does not reliably detect changed attributes, close prior records, or preserve stable surrogate keys.
- Gold product `sequence_by="product_id"` does not represent update order.
- Gold fact-table merge grain may be wrong if orders have multiple lines.
- Silver Regions drops `_rescued_data` without first quarantining or reporting malformed rows.
- The Bronze task parameter list omits Regions.
- Auto Loader schema evolution and trigger settings need explicit decisions.
- No source data or cloud workspace was available for execution testing.

These issues are documented rather than represented as fixed, because a correct implementation depends on source schema, business keys, change timestamps, runtime version, and workspace configuration that were not supplied.
