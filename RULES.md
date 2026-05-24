# RULES.md — Vibe Coding Rules for This Project

> **Source:** Extracted and adapted from the [Vibe Coding Playbook](https://github.com/plamen5rov/vibe-coding-playbook) — consensus patterns from 10 AI LLMs, refined for production-grade context engineering projects.
>
> **Detailed rules are in skills/** — this file covers core principles. Domain-specific rules live in OpenCode-format skills that auto-load when relevant.

---

## Core Operating Principles

### 1. Plan Before Coding

Before the agent writes a single line, ask it to write a plan. This is the single most effective way to avoid long debugging sessions caused by the agent misunderstanding your intent.

### 2. Work in Small Iterations

Never ask the agent to build an entire feature in one prompt. Check in every few steps. Course-correct early — redirecting is much faster than debugging a large block of wrong code.

### 3. Use Git as Your Undo Button

Commit before any significant agent action. If the output is bad, `git checkout .` resets everything instantly.

### 4. Clear Context Between Tasks

Accumulated irrelevant context wastes token budget and can cause the agent to conflate different tasks. Start fresh when switching to unrelated work.

### 5. Treat the Agent as a Fast Junior Developer

You are the architect. The agent is a very fast implementer. Guide it with clear specs, review its output, and own the result. Accepting AI output without review leads to security issues and accumulated technical debt.

---

## Context File Rules

### The Minimum Set

| File | Purpose | Length |
| --- | --- | --- |
| `AGENT.md` | Main context — read every session | Under 200 lines |
| `README.md` | Human-readable project overview | As needed |
| `docs/PROJECT.md` | Goals, audience, constraints | 1 page |
| `docs/TASKS.md` | Current work items | Living doc |
| `.gitignore` | What to exclude from git | Standard |

### Key Principles

- **Keep AGENT.md under 200 lines** — longer files get skimmed, not read
- **Use progressive disclosure** — reference detailed docs, don't cram everything into AGENT.md
- **Every line must pass the test:** *"Would removing this line cause the agent to make mistakes?"*

---

## Git & GitHub Rules

### Create the Repo Yourself — Always

Create the GitHub repo first, then clone locally. Don't let the AI create the repo — you lose control over visibility, license, and `.gitignore` templates.

### Use Feature Branches, Even Solo

- If the agent goes off-track on `main`, you have no clean rollback point
- A branch lets you discard everything and start fresh
- Frequent small commits give you rollback points

### What to Commit vs Ignore

| Commit | Ignore |
| --- | --- |
| `AGENT.md`, `skills/`, `docs/` | `.env`, `.env.*` (except `.env.example`) |
| `opencode.json`, `.opencode/commands/` | `node_modules/`, `dist/`, `build/` |
| `README.md`, `CHANGELOG.md` | Personal override files (`*.local.*`) |

### The `.env.example` Pattern

Commit this file with all variable names but empty values. Shows the AI what environment variables exist without exposing secrets.

---

## Domain-Specific Skills

Detailed rules for specific domains live in `skills/`. These follow the OpenCode skill format and auto-load when relevant:

| Skill | When it loads |
| --- | --- |
| [`skills/prompt-engineering/SKILL.md`](skills/prompt-engineering/SKILL.md) | Writing prompts, correcting agent output, structuring tasks |
| [`skills/debugging/SKILL.md`](skills/debugging/SKILL.md) | Something breaks — debugging agent code, diagnosing errors |
| [`skills/session-management/SKILL.md`](skills/session-management/SKILL.md) | Resuming, forking, clearing sessions, context window overflow |
| [`skills/cost-models/SKILL.md`](skills/cost-models/SKILL.md) | Selecting AI models, managing API costs, optimizing tokens |
| [`skills/mcp-servers/SKILL.md`](skills/mcp-servers/SKILL.md) | Configuring MCP servers, security, troubleshooting |
| [`skills/security/SKILL.md`](skills/security/SKILL.md) | Input validation, secrets, auth, least privilege, `.env` hygiene |
| [`skills/sql-performance/SKILL.md`](skills/sql-performance/SKILL.md) | Database queries, indexing, pagination, migrations |
| [`skills/dependency-management/SKILL.md`](skills/dependency-management/SKILL.md) | Adding packages, approval workflow, supply chain security |
| [`skills/observability/SKILL.md`](skills/observability/SKILL.md) | Logging, error handling, metrics, traceability |
| [`skills/architecture/SKILL.md`](skills/architecture/SKILL.md) | ADR compliance, scaling limits, architectural consistency |

---

## Common Mistakes to Avoid

| Mistake | Why It's Bad | Fix |
| --- | --- | --- |
| Prompting too much at once | Leads to poor results | Break into smaller tasks |
| Skipping the plan phase | Debugging takes longer than planning | Always plan first |
| Not reviewing the agent's code | You own the result | Read every file the agent creates |
| Using the same model for everything | Wastes money, often works worse | Match model to task complexity |
| Ignoring context overflow | Agent starts repeating or forgetting | Clear session when needed |
| Not committing frequently | No rollback points | Git is your safety net — commit often |

---

## Per-Session Workflow Checklist

```text
□ git checkout -b feature/name
□ Ask agent to plan first → approve → execute
□ Work in small steps, check in frequently
□ Run lint + tests after changes
□ Commit frequently — git is your undo button
□ /clear between unrelated tasks
□ Update docs/SESSION.md before ending long sessions
□ Update CHANGELOG.md after features
□ Review the full diff before merging
□ Run the full test suite
□ Merge to main, push
```

---

## New Project Setup Checklist

```text
□ Create GitHub repo → clone locally
□ Write docs/PROJECT.md (goals, stack constraints, non-goals)
□ Run /init to generate AGENT.md → review and refine
□ Add .gitignore (include .env, CLAUDE.local.md)
□ Create .opencode/commands/ with your core slash commands
□ Set up global ~/.config/opencode/opencode.json with personal preferences
□ Install gh CLI for GitHub integration
□ Connect MCP servers you use daily
□ Create CHANGELOG.md stub
```
