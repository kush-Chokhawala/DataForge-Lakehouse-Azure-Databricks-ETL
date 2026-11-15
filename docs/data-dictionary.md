# Data dictionary

The archive README identifies four source domains: customers, products, orders, and regions. The notebooks reveal some field names, but the source Parquet files were excluded from this learning copy pending privacy and redistribution review. This dictionary therefore records only fields explicitly referenced in the source code.

| Domain | Fields referenced in code | Notes |
|---|---|---|
| Customers | `customer_id`, `email`, `first_name`, `last_name`, `domains`, `full_name` | Email domain is derived from `email`; full name is concatenated from first/last names |
| Products | `product_id`, `product_name`, `price`, `discounted_price`, `brand` | DLT expectation checks product ID and name; 10% discount is demonstrated |
| Orders | `order_id`, `customer_id`, `product_id`, `order_date`, `total_amount`, `year`, `month`, `day` | Date attributes are derived from `order_date`; ranking uses `total_amount` |
| Regions | `regions` table is referenced | Detailed columns are not established by the inspected transformation code |

## Validation still needed

Confirm actual data types, nullability, primary keys, foreign keys, order-line grain, timestamp timezone, currency, and business definitions from the authoritative source schema before implementing production transformations.
