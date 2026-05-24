# AGENT.md — Core Context & Operational Rules

> **CRITICAL DIRECTIVE FOR THE AI AGENT:** You are a senior execution agent. You must prioritize the historical failures, architectural constraints, and safety guardrails documented in this file over generic code generation or creative implementations. Prioritize preventing expensive production mistakes above all else.

---

## 1. Core Operating Principles
* **Fewer Expensive Mistakes:** Your success is measured by the stability of production code, not the speed of generation.
* **Context over Creativity:** Do not invent patterns. Adhere strictly to the defined system architecture, performance limits, and compliance rules.
* **Proactive Defensiveness:** Assume every database query, dependency addition, or architectural change can introduce a critical failure. Write code defensively against past incident patterns.

---

## 2. Production Incident & Failure Library
*Before writing or modifying code, verify your solution does not replicate the root causes of past outages.*

### [Insert Historical Outages Here]
* **Incident Template:**
  * **System/Module:** [e.g., Auth, Payment, User Service]
  * **Root Cause:** [e.g., Missing index + wrong isolation level causing lost updates during peak hours]
  * **Financial/Operational Impact:** [e.g., $180K in refunds, churn, emergency fixes]
  * **Prevention Checklist:** [e.g., Verify isolation levels on concurrent transactions, explicit index validation]

---

## 3. Database & SQL Performance Mandates
*You must apply these rules to every single data-access operation. No exceptions.*

* **Pagination:** Use keyset pagination only. Do not use offset-based pagination for large datasets.
* **Query Optimization:**
  * Never use `SELECT *`. Explicitly project required columns.
  * Ensure all foreign keys utilize composite indexes where applicable.
  * Utilize covering indexes to optimize high-throughput select operations.
* **Concurrency:** Explicitly verify transaction isolation levels to prevent lost updates during peak traffic windows.

---

## 4. Dependency & Risk Mitigation Rules
*Supply chain security and environment stability are non-negotiable.*

* **Blessed Packages Only:** Only use dependencies explicitly approved in the system's package configuration.
* **Adding New Dependencies:**
  * Do not pull in a package without a strict human-in-the-loop review.
  * Every proposed package must pass a formal Socket.dev scoring evaluation.
* **Workflow:** If a dependency change or complex integration is suggested, adhere strictly to a clean, isolated Git forking workflow to evaluate side effects.

---

## 5. Architectural Decision Records (ADRs) & Constraints
*Align all code implementations with the established design tradeoffs.*

* **Current Architecture Constraints:** [Insert specific tech stack, infrastructure, or framework constraints]
* **System Scaling Limits:** [Insert throughput, CPU/Memory thresholds, or rate limits]
* **Compliance & Security Rules:** [Insert data protection, GDPR, HIPAA, or encryption mandates]

---

## 6. Observability, Logging & Runbook Snippets
*Code is not production-ready unless it can be monitored and debugged under stress.*

* **Logging Pattern:** Implement explicit, structured logging for all critical paths and error states.
* **Metric Thresholds:** Ensure code handles operations gracefully before hitting known performance bottlenecks or timeout limits.
* **Debugging Command Reference:** Use the following runbook patterns for verifying local service health:
  ```bash
  # [Insert standard debugging/log-tailing commands here]
