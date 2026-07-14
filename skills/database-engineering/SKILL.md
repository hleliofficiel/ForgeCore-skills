# Database Engineering & Data Persistence

> Distilled engineering knowledge for designing schemas, optimizing queries, and ensuring data integrity at scale.

Code is ephemeral; data is forever. A flawed application can be rewritten in a weekend, but a corrupted or poorly designed database will cripple a business for years. Treat database engineering with the highest level of rigor.

---

# Core Principles

Every data persistence layer should satisfy:

**Integrity First:** Ensure data correctness at the database level using constraints, foreign keys, and triggers. Do not rely solely on application logic.
**Read vs. Write Optimization:** Understand if your system is read-heavy or write-heavy, and optimize accordingly. You rarely get both for free.
**Normalize Until It Hurts, Denormalize Until It Works:** Start with 3rd Normal Form (3NF). Denormalize only when proven necessary by performance metrics.
**Schema Evolution:** Design schemas that can evolve safely. Avoid breaking changes; prefer additive migrations.
**Understand the Engine:** Abstractions (ORMs) leak. You must understand how the underlying database engine executes queries and manages locks.

---

# Schema Design & Modeling

Data modeling is the foundation of system architecture.

**Primary Keys:** Use UUIDv7 (time-ordered) or Snowflake IDs for primary keys in distributed systems. Sequential integers (auto-increment) create hot spots in distributed databases and expose insertion rates to users.
**Foreign Keys:** Always use foreign key constraints. They enforce referential integrity and provide query optimizers with vital metadata.
**Soft Deletes:** Prefer `deleted_at` timestamps over hard `DELETE` statements. Hard deletes break historical records and can cause lock contention.
**JSON in SQL:** Use JSON/JSONB columns only for truly unstructured data (e.g., third-party API payloads, arbitrary user settings). If you need to index it heavily, it should probably be a relational column.

---

# Indexing Strategies

Indexes make reads fast and writes slow. Use them intentionally.

**B-Tree Indexes:** The default. Good for exact matches, ranges, and sorting.
**Composite Indexes:** When querying by multiple columns, order matters. Put the most selective column first. An index on `(A, B)` helps queries for `A` and `A, B`, but not `B` alone.
**Partial Indexes:** Index only a subset of the data (e.g., `WHERE deleted_at IS NULL`). This saves space and speeds up writes.
**Covering Indexes:** An index that includes all columns needed for a query. The database engine can satisfy the query entirely from the index, avoiding table lookups.
**Over-Indexing:** Too many indexes will kill write performance. Periodically audit and remove unused indexes.

---

# Query Optimization

Slow queries are the most common cause of system degradation.

**The N+1 Problem:** The classic ORM failure. Fetching a list of records and then issuing a separate query for each record's relations. Always use eager loading or JOINs.
**EXPLAIN is Your Friend:** Never guess why a query is slow. Use `EXPLAIN ANALYZE` to see the query execution plan. Look for sequential scans on large tables.
**Avoid `SELECT *`:** Only select the columns you actually need. It reduces memory usage, network I/O, and increases the chance of using a covering index.
**Pagination:** Avoid `OFFSET` pagination for large tables; the database still has to scan the skipped rows. Use keyset (cursor-based) pagination (`WHERE id > last_seen_id LIMIT 20`).

---

# Transaction Management

Protect data consistency during concurrent operations.

**ACID Properties:** Understand Atomicity, Consistency, Isolation, and Durability.
**Isolation Levels:** Know the difference between Read Committed (default in Postgres), Repeatable Read, and Serializable. Lower isolation increases concurrency but introduces phenomena like phantom reads.
**Deadlocks:** Occur when two transactions wait for locks held by each other. Keep transactions as short as possible. Always acquire locks in the same consistent order across the application.
**Optimistic vs. Pessimistic Locking:**

- *Optimistic (Row Versioning):* Assume conflicts are rare. Check version numbers before updating. Best for web apps.
- *Pessimistic (`SELECT ... FOR UPDATE`):* Lock the row immediately. Use only when conflicts are highly likely (e.g., financial ledger balances).

---

# Migration Strategies

Deploying database changes is high-risk.

**Separation of Concerns:** Never deploy code and schema changes in the same atomic step.
**Zero-Downtime Migrations:**
1. Add the new column/table.
2. Deploy code that writes to both old and new, but reads from old.
3. Backfill old data into the new structure.
4. Deploy code that reads from the new structure.
5. Drop the old structure.
**Backward Compatibility:** Old code must run successfully on the new schema during the deployment window.

---

# The ForgeCore Database Standard

A production-ready persistence layer is:
- **Resilient:** It enforces its own rules. It does not trust the application tier.
- **Performant:** It answers common queries in milliseconds using appropriate indexes.
- **Auditable:** It retains historical context through soft deletes and audit logs.
- **Evolvable:** Its migrations are automated, tested, and safe for zero-downtime deployments.

Treat your database as the ultimate source of truth, not a dumb storage bucket.
