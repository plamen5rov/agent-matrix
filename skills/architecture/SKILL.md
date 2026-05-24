---
name: architecture
description: Use when proposing, reviewing, or modifying system architecture, design patterns, or technical decisions. Covers ADR compliance, scaling limits, and architectural consistency rules.
---

# Architecture & Design Constraints

> **Align all code implementations with the established design tradeoffs. Do not invent architecture.**

---

## Architecture Decision Records (ADRs)

### Before Proposing a Design

- Check the project's **Architecture Decision Records (ADRs)** for existing constraints and tradeoffs
- Do not re-litigate decisions already made unless explicitly asked to revisit them
- Respect documented **scaling limits** and **compliance rules**
- Prefer proven, boring patterns over clever novel ones unless there is a clear, documented reason

### When ADRs Conflict

- Follow the decision-making hierarchy in `AGENT.md`
- If a new approach is genuinely better, document why in a new ADR — do not silently override
- Flag discrepancies between ADRs and current codebase immediately

---

## Architectural Consistency

### Prefer

- Existing frameworks
- Existing abstractions
- Existing conventions
- Existing libraries
- Existing naming patterns

### Do NOT Introduce

- New patterns without justification
- Unnecessary abstractions
- Duplicate systems
- Parallel architectures
- Speculative generalization

**Every new dependency must be justified.**

---

## Scaling Limits

- Respect documented throughput limits
- Respect documented CPU/memory thresholds
- Respect documented rate limits
- Design for the documented data volume, not the current data volume
- Flag any operation that could exceed documented limits

---

## Compliance & Security Rules

- Follow documented data protection requirements (GDPR, HIPAA, etc.)
- Follow documented encryption mandates
- Follow documented access control policies
- Follow documented audit logging requirements

---

## Architecture Review Checklist

Before proposing any design change:

- [ ] Does this conflict with an existing ADR?
- [ ] Does this introduce a new pattern? If so, is it justified?
- [ ] Does this respect documented scaling limits?
- [ ] Does this respect compliance requirements?
- [ ] Could this be solved with an existing abstraction?
- [ ] Is this the simplest solution that works?

If any item fails, **flag it before proceeding**.

---

## Current Architecture Constraints

> **[INSERT_TECH_STACK_CONSTRAINTS_HERE]**
>
> *Document the current tech stack, infrastructure, and framework constraints here. This section should be populated by the project team.*

---

## System Scaling Limits

> **[INSERT_SCALING_LIMITS_HERE]**
>
> *Document throughput, CPU/memory thresholds, rate limits, and expected data volumes here.*

---

## Compliance & Security Rules

> **[INSERT_COMPLIANCE_RULES_HERE]**
>
> *Document data protection, GDPR, HIPAA, encryption mandates, and other compliance requirements here.*
