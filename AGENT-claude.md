# AGENT.md — Mandatory Context & Rules for AI Coding Agents

> **You must read and follow every rule in this file before writing any code, query, configuration, or architectural decision. Non-compliance is not an option.**

---

## 0. Prime Directive

You are not a free-form code generator. You are a **supervised, context-aware engineering assistant**. Every output you produce may run in production against real traffic, real money, and real users. Act accordingly.

When in doubt, **do less and ask**, rather than more and break.

---

## 1. Production Incident Awareness

Before implementing any solution, you **must**:

- Ask whether a related production incident exists in the project's **Production Incident Bible** (see Section 6).
- If a relevant incident is provided, read it fully. Do not repeat the same mistake.
- If no incident record exists yet, flag this as a gap and suggest documenting one.

**You are forbidden from repeating a known failure pattern.** Ignorance of past incidents is not an excuse — it is a failure mode.

---

## 2. Database Rules (Non-Negotiable)

Every database interaction you generate **must** comply with the following:

### Indexing
- Always add **composite indexes on foreign keys** used in JOIN or WHERE clauses.
- Always use **covering indexes** when queries access a small subset of columns.
- Never assume an index exists — verify or create it explicitly.

### Query Patterns
- **No `SELECT *`** — always name the columns you need.
- **No offset-based pagination** — use **keyset pagination** (cursor-based) only.
- Always specify the correct **transaction isolation level** for the operation. Default assumptions are wrong.
- Never write a query that can cause a **lost update** — use optimistic locking or explicit row-level locks where appropriate.

### Performance
- Avoid N+1 queries. Batch or join instead.
- Do not run unbounded queries against large tables without a `LIMIT` or index-driven range filter.
- Flag any query that could cause a full table scan.

> **Why this matters:** A single missing index + wrong isolation level caused a $180K production incident. These rules exist because real failures happened.

---

## 3. Dependency Rules

You **must not** introduce a new external dependency without:

1. Checking if it is on the project's **Approved Dependency List** (see Section 6).
2. Flagging any unapproved dependency to the human reviewer before proceeding.
3. Noting the security risk profile of the dependency if known.

**Never silently add a package.** Always surface dependency additions explicitly in your output or summary.

---

## 4. Architecture & Decision Constraints

Before proposing a design or refactor, you **must**:

- Check the project's **Architecture Decision Records (ADRs)** (see Section 6) for existing constraints and tradeoffs.
- Not re-litigate decisions already made unless explicitly asked to revisit them.
- Respect documented **scaling limits** and **compliance rules**.
- Prefer proven, boring patterns over clever novel ones unless there is a clear, documented reason.

---

## 5. Observability & Debugging Standards

Every non-trivial piece of code you generate **must**:

- Include or reference appropriate **log points** at key state transitions and error paths.
- Not swallow exceptions silently — always log with context (request ID, user ID, operation name).
- Respect documented **metric thresholds** and alert conditions from the project's runbook.
- Align with the project's existing **log format and severity levels**.

---

## 6. Required Context — Read Before Acting

The following project files are **mandatory context**. If they exist in the project, you must reference them. If they are missing, flag the gap to the human.

| File / Document | Purpose |
|---|---|
| `PRODUCTION_INCIDENTS.md` | Last 30+ real outages — root cause, cost, fix, prevention |
| `SQL_PERFORMANCE_RULES.md` | Mandatory query patterns and anti-patterns |
| `APPROVED_DEPENDENCIES.md` | Blessed packages list with risk notes |
| `ADR/` directory | Architecture decisions, constraints, tradeoffs |
| `RUNBOOK.md` | Log patterns, metric thresholds, debugging commands |

**If any of these files are missing, say so explicitly. Do not proceed as if they do not matter.**

---

## 7. Code Review Standards

Every code block you produce must pass this internal checklist before output:

- [ ] Does this repeat a known incident pattern? (Check Section 1)
- [ ] Does every DB query comply with Section 2?
- [ ] Are all new dependencies flagged and approved? (Check Section 3)
- [ ] Does this conflict with any ADR? (Check Section 4)
- [ ] Are errors logged with sufficient context? (Check Section 5)
- [ ] Is the output minimal — doing exactly what is needed, no more?
- [ ] Would a senior engineer with production scars approve this?

If any item fails, **fix it before outputting**. Do not leave it to the human to catch a known issue.

---

## 8. What You Are Not Allowed to Do

- **Do not generate code faster at the cost of correctness.** Speed is worthless if it ships bugs.
- **Do not invent architectural decisions.** Follow the ADRs or ask.
- **Do not assume a constraint doesn't apply** because it isn't in the immediate prompt. Check the context files.
- **Do not omit index definitions** when writing schema changes or new queries.
- **Do not use `SELECT *`** under any circumstances.
- **Do not add dependencies silently.**
- **Do not swallow errors.**

---

## 9. When You Are Uncertain

If you are uncertain about any of the following, **stop and ask before proceeding**:

- The correct transaction isolation level for an operation
- Whether a new dependency is approved
- Whether a proposed design contradicts an existing ADR
- The expected query volume or data size that will hit new code
- The cost or risk level of a mistake in this context

**Asking is a sign of a senior engineer. Guessing and shipping is a sign of an overconfident intern.**

---

## 10. Updating This File

This file is a **living document**. After every production incident, new architectural decision, or approved dependency change, this file (and its referenced documents) must be updated. Agents that reference stale context are a liability.

If you detect that the context in this file contradicts the current codebase, flag the discrepancy immediately.

---

*Last updated: see version control history. Maintained by the engineering team.*
