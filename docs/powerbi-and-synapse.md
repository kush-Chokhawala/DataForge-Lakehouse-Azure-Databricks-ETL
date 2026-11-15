# Power BI and Synapse artifacts

## Power BI

The supplied archive contains one `.pbids` file targeting the Databricks SQL Warehouse protocol in DirectQuery mode. It does not contain a `.pbix` report or `.pbit` template. The connection file has been converted to a placeholder template:
- workspace host: `<YOUR_DATABRICKS_WORKSPACE_HOST>`
- SQL warehouse HTTP path: `<YOUR_SQL_WAREHOUSE_HTTP_PATH>`
- catalog: `<YOUR_CATALOG>`
- schema/database: `<YOUR_SCHEMA>`

Fill these placeholders only in a private local copy. Do not commit a personal workspace endpoint unless you intentionally want it public.

## Synapse

The inherited README describes Synapse Analytics as a query endpoint over curated data. No Synapse SQL DDL, external data source definitions, credentials, views, or deployment scripts were present in the ZIP. Treat Synapse as a described architecture component, not a reproducible artifact in this repository.
