---
name: dependency-management
description: Use when adding, removing, or evaluating any external package or library dependency. Covers approval workflow, risk assessment, and supply chain security.
---

# Dependency Management Rules

> **Supply chain security and environment stability are non-negotiable.**

---

## Before Adding a Dependency

Ask these questions:

1. Can the existing stack already solve this?
2. Is this dependency production-proven?
3. Is maintenance risk acceptable?
4. Is the package actively maintained?
5. Does it increase security exposure?

If the answer to any of these is unclear, **flag it to the human reviewer**.

---

## Approval Workflow

### Blessed Packages Only

- Only use dependencies explicitly approved in the project's approved dependency list
- Check the project's **Approved Dependency List** before adding anything
- Flag any unapproved dependency to the human reviewer before proceeding
- Note the security risk profile of the dependency if known

### Adding New Dependencies

- **Do not pull in a package without a strict human-in-the-loop review**
- Every proposed package must pass a formal security scoring evaluation (e.g., Socket.dev)
- Use a clean, isolated Git forking workflow to evaluate side effects before merging
- Never silently add a package — always surface dependency additions explicitly in your output or summary

---

## What to Avoid

- Trendy packages with no production track record
- Abandoned libraries with no recent commits
- Massive frameworks for small tasks
- Unnecessary transitive dependency chains
- Packages with known security vulnerabilities

---

## What to Prefer

- Standard library solutions
- Existing internal utilities
- Mature ecosystem tools with active maintenance
- Minimal, focused packages over bloated frameworks

---

## Dependency Review Checklist

Before outputting any code that adds a dependency:

- [ ] Is this on the approved list?
- [ ] If not, has it been flagged for human review?
- [ ] Has the security risk profile been noted?
- [ ] Are there transitive dependencies that introduce risk?
- [ ] Is there a simpler way to solve this with existing tools?

If any item fails, **fix it before outputting**.
