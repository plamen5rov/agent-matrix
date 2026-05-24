---
name: observability
description: Use when writing, reviewing, or modifying code that involves logging, error handling, metrics, or debugging. Covers structured logging, error context, and operational visibility standards.
---

# Observability & Debugging Standards

> **Code is not production-ready unless it can be monitored and debugged under stress.**

---

## Logging Rules

### Required

- Implement explicit, **structured logging** for all critical paths and error states
- Include log points at key state transitions and error paths
- Every log must help operators answer:
  - What failed
  - Where it failed
  - Why it failed
  - What the impact is
  - What the probable fix is

### Error Handling

- Never swallow exceptions silently — always log with context
- Include contextual identifiers: request ID, user ID, operation name
- Provide actionable error messages, not generic "something went wrong"
- Fail securely — errors should not expose internal state

### Log Quality

- Avoid noisy logging — every log line should be actionable
- Align with the project's existing **log format and severity levels**
- Do not log sensitive data (PII, tokens, passwords, secrets)

---

## Metrics & Monitoring

- Include metrics where appropriate for critical operations
- Ensure code handles operations gracefully before hitting known performance bottlenecks or timeout limits
- Respect documented **metric thresholds** and alert conditions from the project's runbook

---

## Traceability

- Include traceable execution paths for complex operations
- Ensure errors propagate with sufficient context for debugging
- Use correlation IDs to track requests across service boundaries

---

## Debugging Standards

- Provide clear debugging commands or runbook references for new operations
- Document standard log-tailing and service health check commands
- Ensure local debugging patterns match production debugging patterns

---

## Observability Checklist

Before outputting any non-trivial code:

- [ ] Are key state transitions logged?
- [ ] Are error paths logged with sufficient context?
- [ ] Are exceptions propagated, not swallowed?
- [ ] Do logs follow the project's format and severity levels?
- [ ] Are metric thresholds respected?
- [ ] Would an on-call engineer be able to debug this from logs alone?

If any item fails, **fix it before outputting**.
