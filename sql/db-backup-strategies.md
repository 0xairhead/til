# Database Backup Strategies

Two common strategies to protect against accidental data loss:

## 1. Automated Backups

Always turn on automated backups for production databases (e.g., GCP Cloud SQL, AWS RDS).
- Automatically takes a snapshot of your entire database on a schedule.
- Retains history for a set period (e.g., 30 days).
- Protects against catastrophic mistakes.

## 2. Soft Deletes

Instead of actually deleting records (`DELETE`), mark them as deleted using a `deleted_at` timestamp column.
- **How it works:** Your queries must filter out active rows (e.g., `WHERE deleted_at IS NULL`).
- **Why use it:** Allows easily restoring specific pieces of user data.
- **Caveat:** Only use when there is a specific business requirement. For most applications, automated backups are sufficient.