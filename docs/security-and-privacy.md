# Security and privacy review

## Changes made

- Replaced the hardcoded ADLS storage account name in notebook/script paths with a `storage_account` widget placeholder where used.
- Replaced the workspace host and SQL warehouse HTTP path in the PBIDS file with placeholders.
- Cleared all saved notebook outputs and execution counts.
- Removed `.ipynb_checkpoints` duplicates.
- Excluded the original Parquet sample files because their records could not be inspected with an available Parquet reader for privacy and redistribution review.
- Excluded Azure portal screenshots other than the architecture diagrams because they may expose workspace, subscription, resource, or personal identifiers.

## Before publishing

- Inspect the full Git history for secrets, personal information, and environment identifiers.
- Rotate any credentials that may have been committed in upstream history.
- Confirm that the upstream license permits reuse and redistribution.
- Review the architecture images for source attribution and licensing.
- Configure storage identity using a supported managed identity / Access Connector or approved secret manager.
- Grant least privilege to the Databricks access identity.
- Do not commit local `.env` files, tokens, client secrets, account keys, connection strings, or private workspace endpoints.

The absence of a visible secret in this edited copy does not prove that the original upstream Git history is clean.
