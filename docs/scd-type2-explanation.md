# SCD Type 2: intended pattern and current implementation gaps

## Intended behavior

A Slowly Changing Dimension Type 2 design preserves history when tracked attributes change. A typical dimension row includes:
- A surrogate key (for example, `DimCustomerKey`).
- A stable natural/business key (for example, `customer_id`).
- Tracked descriptive attributes.
- `effective_start` and `effective_end` timestamps/dates.
- A current-row indicator such as `is_current`.

When a source record changes, the pipeline expires the current row and inserts a new row with a new surrogate key. Unchanged rows remain unchanged.

## What the inherited Gold customer notebook attempts

The supplied notebook:
1. Reads Silver customer records.
2. Reads existing Gold customer rows, depending on an initialization widget.
3. Joins incoming and existing rows by `customer_id`.
4. Separates records with and without an existing surrogate key.
5. Generates surrogate keys for new records.
6. Unions records and MERGEs them into a Delta target.

## Why this is not yet a reliable SCD Type 2 implementation

- It does not compare tracked attributes to identify changed rows.
- It updates `update_date` on existing rows during processing, even when attributes may be unchanged.
- It does not close the old version with an effective end date.
- It does not create a new version for changed records.
- It does not maintain an `is_current` flag.
- `monotonically_increasing_id()` is not a stable, deterministic business surrogate-key allocator across reruns.
- The initialization logic and target-table existence assumptions need validation.
- A merge keyed only by surrogate key does not itself implement versioning.

## Recommended design (not implemented or tested here)

1. Define tracked attributes and a deterministic change hash.
2. Compare incoming records to current dimension rows by natural key.
3. Leave unchanged rows untouched.
4. For changed rows, expire the current record (`effective_end`, `is_current=false`).
5. Insert a new version with a newly allocated surrogate key, `effective_start`, open-ended `effective_end`, and `is_current=true`.
6. Insert genuinely new natural keys.
7. Validate exactly one current row per natural key and ensure fact rows resolve to the dimension version valid at event time.

Do not claim the inherited notebook implements SCD Type 2 correctly until this design is implemented and tested against the actual source schema.
