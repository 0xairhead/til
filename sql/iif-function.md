# IIF() Function

A simpler shorthand function for the standard `CASE...WHEN` expression. It evaluates a boolean condition and returns one value if true, and another if false.

## 1. Syntax & Example

Takes three arguments: `IIF(condition, value_if_true, value_if_false)`.

```sql
SELECT *,
    IIF(was_successful = true, 'No action required', 'Perform an audit') AS audit
FROM transactions;
```

## 2. Equivalent `CASE` Expression

The `IIF()` function acts exactly like a `CASE` statement under the hood:

```sql
SELECT *,
    CASE 
        WHEN was_successful = true THEN 'No action required'
        ELSE 'Perform an audit'
    END AS audit
FROM transactions;
```

> **Note:** `IIF()` is natively supported in SQL Server (2012+) and MS Access (Snowflake uses `IFF()`). For maximum compatibility across all database systems, standard `CASE` statements are still best practice.
