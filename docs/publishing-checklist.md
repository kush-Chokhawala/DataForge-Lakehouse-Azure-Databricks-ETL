# Publishing checklist

- [ ] Identify the original GitHub repository URL and author.
- [ ] Confirm the upstream license and rights to redistribute code and architecture images.
- [ ] Inspect the full Git history for secrets, PII, and workspace/resource identifiers.
- [ ] Rotate credentials found in upstream history if they were ever valid.
- [ ] Configure `storage_account` widgets in a private Databricks workspace.
- [ ] Review storage identity, Access Connector, RBAC, and ADLS ACL permissions.
- [ ] Confirm the target Databricks Runtime supports the notebook APIs and trigger options.
- [ ] Validate Auto Loader schema and checkpoint paths.
- [ ] Validate source schema, primary keys, and fact-table grain.
- [ ] Redesign and test customer SCD Type 2 logic.
- [ ] Replace product `sequence_by` with a true update timestamp/version.
- [ ] Review rescued-data handling and data-quality rules.
- [ ] Add the missing ADF exports and Synapse SQL scripts if reproducibility is required.
- [ ] Add a real Power BI report only if one is actually created and tested.
- [ ] Update the README to reflect only work actually completed and verified.
