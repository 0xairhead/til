# Zero-Downtime Column Migration

1. Create new column
2. Copy data from old → new
3. Deploy code using new column
4. Re-sync data (catch updates during deploy window)
5. Remove old column

## 1. Add Nullable Column
```sql
ALTER TABLE your_table ADD COLUMN new_col TEXT;
```

## 2. Backfill Data (Batched)
```sql
-- PostgreSQL
UPDATE your_table SET new_col = old_col WHERE id IN (
  SELECT id FROM your_table WHERE new_col IS NULL LIMIT 1000
);

-- MySQL
UPDATE your_table SET new_col = old_col WHERE new_col IS NULL LIMIT 1000;
```

## 3. Deploy Dual-Write App Code
* **Read:** `new_col` with fallback to `old_col`.
* **Write:** Save to both `new_col` and `old_col`.

## 4. Re-sync Missed Data
```sql
-- PostgreSQL
UPDATE your_table SET new_col = old_col WHERE new_col IS DISTINCT FROM old_col;

-- MySQL
UPDATE your_table SET new_col = old_col WHERE NOT (new_col <=> old_col);
```

## 5. Deploy Final Code & Drop Old Column
App now only reads/writes `new_col`.
```sql
ALTER TABLE your_table DROP COLUMN old_col;
```

## (Optional) Constraints & Indexes
```sql
ALTER TABLE your_table ALTER COLUMN new_col SET NOT NULL;
CREATE INDEX idx_new_col ON your_table(new_col);
```
