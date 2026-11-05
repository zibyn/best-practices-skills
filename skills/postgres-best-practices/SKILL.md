---
name: postgres-best-practices
description: "Postgres best practices for schema design, indexing, query performance, connection management, locking, and row-level security, for Postgres running anywhere. Load this skill BEFORE writing or changing anything that lives in a Postgres database: creating or altering tables and columns (including choosing column types), migrations, constraints, indexes, RLS policies, privileges, and SQL queries. Also load it when diagnosing slow queries, high CPU, timeouts, EXPLAIN plans, connection exhaustion, locking, bloat, or rows visible to the wrong user or tenant."
license: MIT
metadata:
  version: "1.0.0"
---

# Postgres Best Practices

Performance and correctness guide for Postgres, independent of hosting provider. Rules are grouped into 8 categories and prioritized by impact.

## When to Apply

- Writing SQL queries or designing schemas
- Adding indexes or optimizing queries
- Reviewing database performance issues
- Configuring connection pooling or scaling
- Writing Row-Level Security (RLS) policies or granting privileges

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | Query Performance | CRITICAL | `query-` |
| 2 | Connection Management | CRITICAL | `conn-` |
| 3 | Security & RLS | CRITICAL | `security-` |
| 4 | Schema Design | HIGH | `schema-` |
| 5 | Concurrency & Locking | MEDIUM-HIGH | `lock-` |
| 6 | Data Access Patterns | MEDIUM | `data-` |
| 7 | Monitoring & Diagnostics | LOW-MEDIUM | `monitor-` |
| 8 | Advanced Features | LOW | `advanced-` |

Category descriptions: `references/_sections.md`.

## Rules

Each rule lives at `references/<rule>.md` with an explanation, incorrect and correct SQL, and references.

- `advanced-full-text-search` - Use tsvector for Full-Text Search
- `advanced-jsonb-indexing` - Index JSONB Columns for Efficient Querying
- `conn-idle-timeout` - Configure Idle Connection Timeouts
- `conn-limits` - Set Appropriate Connection Limits
- `conn-pooling` - Use Connection Pooling for All Applications
- `conn-prepared-statements` - Use Prepared Statements Correctly with Pooling
- `data-batch-inserts` - Batch INSERT Statements for Bulk Data
- `data-n-plus-one` - Eliminate N+1 Queries with Batch Loading
- `data-pagination` - Use Cursor-Based Pagination Instead of OFFSET
- `data-upsert` - Use UPSERT for Insert-or-Update Operations
- `lock-advisory` - Use Advisory Locks for Application-Level Locking
- `lock-deadlock-prevention` - Prevent Deadlocks with Consistent Lock Ordering
- `lock-short-transactions` - Keep Transactions Short to Reduce Lock Contention
- `lock-skip-locked` - Use SKIP LOCKED for Non-Blocking Queue Processing
- `monitor-explain-analyze` - Use EXPLAIN ANALYZE to Diagnose Slow Queries
- `monitor-pg-stat-statements` - Enable pg_stat_statements for Query Analysis
- `monitor-vacuum-analyze` - Maintain Table Statistics with VACUUM and ANALYZE
- `query-composite-indexes` - Create Composite Indexes for Multi-Column Queries
- `query-covering-indexes` - Use Covering Indexes to Avoid Table Lookups
- `query-index-types` - Choose the Right Index Type for Your Data
- `query-missing-indexes` - Add Indexes on WHERE and JOIN Columns
- `query-partial-indexes` - Use Partial Indexes for Filtered Queries
- `schema-constraints` - Add Constraints Safely in Migrations
- `schema-data-types` - Choose Appropriate Data Types
- `schema-foreign-key-indexes` - Index Foreign Key Columns
- `schema-lowercase-identifiers` - Use Lowercase Identifiers for Compatibility
- `schema-partitioning` - Partition Large Tables for Better Performance
- `schema-primary-keys` - Select Optimal Primary Key Strategy
- `security-privileges` - Apply Principle of Least Privilege
- `security-rls-basics` - Enable Row Level Security for Multi-Tenant Data
- `security-rls-performance` - Optimize RLS Policies for Performance

## Conventions in Examples

- RLS examples read the current user from a session setting: `current_setting('app.current_user_id')`. Replace it with however your application or auth layer sets the user identity.
- `app_user` stands for the role the application connects as.

## References

- https://www.postgresql.org/docs/current/
- https://wiki.postgresql.org/wiki/Performance_Optimization
