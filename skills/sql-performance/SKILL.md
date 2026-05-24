---
name: sql-performance
description: Use when writing, reviewing, or modifying any database queries, migrations, or data-access logic. Covers indexing, query patterns, pagination, transaction isolation, and performance anti-patterns.
---

# SQL Performance & Data Access Rules

> **You must apply these rules to every single data-access operation. No exceptions.**

---

## Indexing Rules

- Always add **composite indexes on foreign keys** used in JOIN or WHERE clauses
- Always use **covering indexes** when queries access a small subset of columns
- Never assume an index exists — verify or create it explicitly
- Missing indexes on foreign keys are a production incident waiting to happen

---

## Query Patterns

### Forbidden

- **No `SELECT *`** — always name the columns you need
- **No offset-based pagination** — use **keyset pagination** (cursor-based) only for large datasets
- No unbounded deletes — always scope with a WHERE clause and LIMIT
- No unbounded updates — always scope with a WHERE clause
- No N+1 query patterns — batch or join instead
- No implicit cross joins
- No long-running locks without explicit justification

### Required

- Explicit column selection in every query
- Pagination limits on all list operations
- Indexed joins
- Transactional consistency for multi-step operations
- Idempotent operations where possible
- Rollback-safe migrations

---

## Concurrency & Isolation

- Always specify the correct **transaction isolation level** for the operation. Default assumptions are wrong
- Never write a query that can cause a **lost update** — use optimistic locking or explicit row-level locks where appropriate
- Explicitly verify transaction isolation levels to prevent lost updates during peak traffic windows
- Reason about concurrency before writing any multi-step data operation

---

## Performance Rules

- Avoid N+1 queries — batch or join instead
- Do not run unbounded queries against large tables without a `LIMIT` or index-driven range filter
- Flag any query that could cause a full table scan
- Reason about scale before writing SQL — what happens at 10x the current data volume?
- Reason about indexes — will this query use them?
- Reason about isolation levels — could concurrent transactions corrupt data?

---

## Migration Safety

- All migrations must be rollback-safe
- Avoid destructive migrations (DROP, TRUNCATE) without explicit approval
- Never break API contracts in migrations
- Add indexes in separate migrations from schema changes when possible
- Test migrations against production-sized data before deploying

---

## Before Generating SQL

Before generating any SQL, reason about:

1. **Scale** — what happens at 10x, 100x current data volume?
2. **Indexes** — will this query use existing indexes? Does it need new ones?
3. **Concurrency** — could concurrent transactions cause conflicts?
4. **Isolation levels** — is the default isolation level correct for this operation?

---

> **Why this matters:** A single missing index + wrong isolation level caused a $180K production incident. These rules exist because real failures happened.
