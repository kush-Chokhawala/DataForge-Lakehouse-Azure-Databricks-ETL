# DataForge Lakehouse
## Azure Databricks — End-to-End ETL & Streaming Data Platform

**Project positioning:** personal learning project based on an inherited GitHub project archive. This repository documents and reviews an Azure Databricks lakehouse workflow; it is not represented as an independently deployed production platform.

DataForge Lakehouse explores a cloud data pipeline built around Azure Data Factory, Azure Data Lake Storage Gen2 (ADLS Gen2), Azure Databricks, Apache Spark, Delta Lake, Delta Live Tables, and Unity Catalog. The supplied notebooks demonstrate ingestion and transformation patterns for customer, product, order, and region datasets.

> **Execution status:** the notebooks and scripts have been edited for safer configuration and clearer documentation, but have not been executed in an Azure Databricks workspace by the current maintainer. Treat runtime behavior and cloud screenshots as inherited evidence, not as independently revalidated results.

## Architecture

![Azure Databricks lakehouse architecture](azure%20screenshots/databricks/AzureDatabricksArchitecture_Big.jpeg)

```text
Parquet source files
       |
       v
Azure Data Factory (copy/orchestration, as described in source README)
       |
       v
ADLS Gen2 — Bronze
(Auto Loader / Structured Streaming ingestion)
       |
       v
Databricks + Spark + Delta Lake — Silver
(cleaning, standardization, derived attributes)
       |
       v
Databricks / Delta Live Tables — Gold
(customer & product dimensions, orders fact)
       |
       v
Databricks SQL Warehouse / BI consumption
(PBIDS connection template supplied; no PBIX report included)
```

The original architecture also mentions Synapse Analytics and Power BI. The archive contains no Synapse SQL deployment scripts and no Power BI report (`.pbix`); it includes only a Databricks SQL Warehouse PBIDS connection definition, now converted into a placeholder template.

## Technology areas

- **Azure Data Factory:** source-to-Bronze copy orchestration described in the inherited README.
- **ADLS Gen2:** source, Bronze, Silver, and Gold storage zones.
- **Databricks Auto Loader:** file discovery using `cloudFiles`, schema tracking, and checkpoints.
- **Apache Spark Structured Streaming:** incremental file ingestion patterns.
- **Delta Lake:** transactional Silver/Gold storage and MERGE/upsert examples.
- **PySpark and Spark SQL:** transformations, UDFs, and analytical window functions.
- **Delta Live Tables:** declarative pipeline patterns and data-quality expectations.
- **Unity Catalog:** catalog/schema/table organization and governed data access concepts.
- **Dimensional modeling:** customer/product dimensions and an orders fact-table pattern.
- **Power BI connectivity:** a parameterized PBIDS template for Databricks SQL Warehouse.

## Repository structure

```text
.
├── README.md
├── SECURITY.md
├── NOTICE.md
├── .gitignore
├── azure screenshots/
│   └── databricks/
│       ├── AzureDatabricksArchitecture.jpeg
│       └── AzureDatabricksArchitecture_Big.jpeg
├── scripts/
│   └── python/
│       ├── Parameters.ipynb
│       ├── Bronze_Layer.ipynb
│       ├── Silver_Customers.ipynb
│       ├── Silver_Products.ipynb
│       ├── Silver_Orders.py
│       ├── Silver_Regions.ipynb
│       ├── Gold_Customers.ipynb
│       ├── Gold_Products.ipynb
│       └── Gold_Orders.py
├── powerbi/
│   └── databricks-connection-template.pbids
└── docs/
    ├── architecture-and-data-flow.md
    ├── notebook-review.md
    ├── data-dictionary.md
    ├── scd-type2-explanation.md
    ├── unity-catalog-governance.md
    ├── powerbi-and-synapse.md
    ├── security-and-privacy.md
    └── publishing-checklist.md
```

Generated `.ipynb_checkpoints` backups and saved notebook outputs are intentionally excluded.

## Pipeline walkthrough

### 1. Parameters and orchestration

`Parameters.ipynb` defines `orders`, `customers`, and `products` as dataset parameters and publishes them through `dbutils.jobs.taskValues.set("output_datasets", datasets)`. The Regions dataset is present in the downstream notebooks but is not included in that original parameter list; align orchestration metadata if Regions is intended to run in the same job.

### 2. Bronze ingestion

`Bronze_Layer.ipynb` reads Parquet files using `spark.readStream.format("cloudFiles")`, configures `cloudFiles.schemaLocation`, and writes append-mode Parquet data with a checkpoint location. The original notebook uses `trigger(once=True)`. Confirm the supported trigger option for the target Databricks Runtime; `availableNow=True` is commonly used for bounded processing of all currently available files.

### 3. Silver transformations

- **Customers:** extracts email domains, aggregates customer counts by domain, and creates `full_name` from first and last names.
- **Products:** demonstrates SQL/Python UDFs, a 10% discounted-price calculation, and uppercase brand transformation.
- **Orders:** converts order dates to timestamps, derives year/month/day, and demonstrates `dense_rank`, `rank`, and `row_number`.
- **Regions:** reads the Bronze Regions table, drops `_rescued_data`, and writes Delta output.

### 4. Gold modeling

- **Gold Customers:** attempts customer dimension key management and Delta MERGE. The inherited implementation is not a complete or reliable SCD Type 2 implementation; see `docs/scd-type2-explanation.md`.
- **Gold Products:** uses Delta Live Tables expectations and an SCD Type 2 change-application pattern. The inherited `sequence_by="product_id"` is not a reliable change-order field and must be replaced with a source update timestamp/version before claiming correct CDC behavior.
- **Gold Orders:** joins Silver orders to Gold dimensions and upserts a fact table. Validate the fact grain and merge key: `order_id` alone may not uniquely identify line-item facts.

## Configuration and safe setup

The notebooks now use a `storage_account` widget placeholder where the original source hardcoded the storage account. Set it in the Databricks notebook/job before running. SQL magic cells use Databricks widget substitution where applicable. The PBIDS file contains placeholders for workspace host, SQL warehouse HTTP path, catalog, and schema.

Do not commit real workspace identifiers, secrets, connection strings, tokens, or exported credentials. Use managed identity / Access Connector or an approved secret manager according to the workspace's supported configuration. Apply least-privilege access.

## Data and screenshot handling

The original archive included seven Parquet sample files and many Azure portal screenshots. They are not included in this public-ready learning copy because the Parquet contents could not be inspected with an available Parquet reader for personal-data/licensing review, and the portal screenshots may expose account/resource identifiers or personal information. The two architecture diagrams are retained. Re-add sample datasets or screenshots only after reviewing privacy, ownership, and redistribution permission.

## Important implementation caveats

- No ADF JSON pipeline, linked-service, dataset, or trigger exports were included.
- No Synapse SQL scripts or infrastructure-as-code were included.
- No Power BI `.pbix` report was included.
- The PBIDS template is not a report and will not connect until configured with a valid workspace and warehouse.
- Gold customer SCD Type 2 logic requires redesign and runtime testing.
- Gold product change sequencing requires an authoritative update timestamp/version.
- Fact-table grain and dimensional key lookup logic require validation.
- `dropDuplicates` is assigned in the Gold customer notebook after correcting the original no-op call, but the full notebook still needs runtime validation.
- Notebook output cells have been cleared to avoid distributing cached records.
- No notebook or cloud pipeline has been executed after these edits.

## Attribution and license

This repository is adapted from the user-supplied archive named `End-To-End-Data-Engineering-ETL-Pipeline-Project-Azure-Databricks-Spark-UnityCatalog-main.zip`. The archive did not include an upstream repository URL or a LICENSE file. Confirm the exact upstream source, attribution requirements, and redistribution rights before publishing inherited code. Public GitHub visibility does not itself grant permission to redistribute code or assets.

## Personal learning statement

This repository is maintained as a personal learning project to study Azure-based lakehouse architecture, streaming ingestion, Delta Lake transformations, dimensional modeling, and Unity Catalog governance. Clearly distinguish inherited source material, edits in this learning copy, and proposed work that remains unimplemented or untested.
