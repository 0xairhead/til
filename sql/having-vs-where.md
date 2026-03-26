# HAVING vs. WHERE

Both clauses filter rows in a query, but they operate at different stages of execution—particularly when **aggregate functions** are involved.

## 1. WHERE (Pre-Aggregation)

Filters individual rows *before* any grouping (`GROUP BY`) or aggregation occurs.
**You cannot use aggregate functions inside a `WHERE` clause.**

```sql
SELECT department_id, salary
FROM Employees
WHERE salary > 50000; -- Filters individual rows immediately
```

## 2. HAVING (Post-Aggregation)

Filters grouped rows *after* the `GROUP BY` clause and aggregation are applied.
**It is primarily used to filter on standard aggregate functions** (e.g., `COUNT()`, `SUM()`, `AVG()`).

```sql
SELECT department_id, SUM(salary) AS total_salary
FROM Employees
GROUP BY department_id
HAVING SUM(salary) > 500000; -- Filters the aggregate result for each group
```

## 3. Combining Both

You can use both in the same query to filter raw data first, and then filter the resulting aggregated groups:

```sql
SELECT department_id, COUNT(employee_id) AS num_employees
FROM Employees
WHERE status = 'Active'               -- 1. Filter out inactive employees
GROUP BY department_id                -- 2. Group the remaining active employees
HAVING COUNT(employee_id) >= 10;      -- 3. Only keep groups with 10+ properties
```
