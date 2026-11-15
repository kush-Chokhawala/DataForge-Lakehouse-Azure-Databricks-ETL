# Unity Catalog and governance

The source notebooks reference a catalog named `databricks_etl_catalog` and schemas named `bronze`, `silver`, and `gold`. The screenshots/README describe Unity Catalog and an Azure Databricks Access Connector.

## Concepts represented

- Catalogs organize schemas and governed data objects.
- Schemas organize tables and functions by data domain/layer.
- Delta tables may be registered with Unity Catalog while their underlying files reside in ADLS Gen2.
- Storage credentials and external locations provide governed access to cloud storage.
- Grants and ownership define which principals can access catalogs, schemas, tables, and volumes.

## Recommended publication/deployment documentation

For a reproducible implementation, add the actual catalog/schema DDL, storage credential and external location setup, role/grant matrix, Access Connector identity configuration, and ADLS RBAC/ACL requirements. Do not publish tenant IDs, storage account identifiers, access tokens, secrets, or private endpoint details.

The supplied ZIP does not include a complete Unity Catalog deployment script or privilege matrix, so these details cannot be asserted as implemented.
