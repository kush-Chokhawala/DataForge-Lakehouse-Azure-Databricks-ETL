# Security policy

Never commit secrets, access tokens, storage account keys, service-principal credentials, or private connection strings. Use a supported managed identity / Databricks Access Connector or approved secret manager and grant least privilege.

The supplied archive included workspace/resource identifiers and notebook outputs. This learning copy replaces storage account/workspace connection values with placeholders, clears notebook outputs, removes checkpoint duplicates, and excludes sample data and portal screenshots pending privacy/licensing review.

If a credential was ever committed in the upstream repository, treat it as compromised: revoke/rotate it with the owning administrator. Removing it from a new ZIP does not remove it from Git history or invalidate it.
