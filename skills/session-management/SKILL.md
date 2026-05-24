---
name: session-management
description: Use when managing AI coding sessions — resuming, forking, clearing context, or handling context window overflow. Covers session lifecycle and context hygiene.
---

# Session Management Rules

> **Sessions are how AI coding tools remember what you did. Manage them correctly and the AI stays sharp. Ignore them and you start over every time.**

---

## Why Sessions Matter

A session accumulates context: your chat history, code changes, tool results, and learned preferences. The longer a session runs, the better the AI understands your project.

New sessions start blank. The AI re-reads files, re-discovers patterns, and forgets decisions you already made. That wastes time and tokens.

**Rule of thumb:** Resume unless you have a reason to start fresh.

---

## Resume vs. Starting Fresh

### When to resume

- Picking up where you left off on the same task
- Continuing work after a break
- Building deep context for a complex project

Both tools support session continuation:

**Claude Code:**
```bash
claude --continue
```

**OpenCode:**
Sessions persist automatically. Load your last session from the session list.

### When to start fresh

- Switching to a completely unrelated task
- Context is stale or confusing the AI
- You need a clean slate for a different project or branch

**Claude Code:**
```bash
/clear    # clears current session
claude    # starts fresh
```

**OpenCode:**
Start a new session from the session list.

Context accumulated from Task A will interfere with Task B. Clear between unrelated tasks.

---

## Forking a Session

Forking copies your entire session history up to that point, then starts an independent session from there. The original session stays unchanged. Think of it as `git branch` for conversations.

### When to fork

- **Testing risky ideas** — try a new architecture without polluting your main session
- **Parallel experiments** — run multiple "what if" scenarios from the same starting point
- **Multi-terminal work** — resume across terminals without context corruption
- **Exploring tangents** — prototype a feature while your primary session stays focused

### When not to fork

- **Simple continuations** — resume the original instead to build deeper context
- **File-heavy work** — forks share the same filesystem; use Git for true isolation
- **Every minor change** — each fork duplicates history and burns extra tokens; reserve forks for genuine divergences, not every tweak

### Resume vs. Fork

| Aspect | Resume | Fork |
| --- | --- | --- |
| History | Appends to original | Copies then branches separately |
| Session ID | Same as before | New, unique ID |
| Original | Continues evolving | Unchanged, like a snapshot |
| File changes | Shared within project dir | Shared (no filesystem isolation) |
| Use case | Steady progress on main work | Safe experiments |

---

## Reverting Inside a Fork

Reverting rolls back a session to an earlier checkpoint — undoing everything after that point without losing the full log.

### When to revert a fork

- **Failed experiments** — if a fork goes wrong, revert to a stable point, then fork again from there
- **Bad AI suggestions** — revert to before the bad response and try a different prompt
- **Question tool detours** — if an AI question led you astray, revert to before it and re-answer

### When not to revert

- **Main session tweaks** — don't revert your primary work; use undos or instructions instead
- **File-shared projects** — revert doesn't snapshot files uniquely; commit to Git first
- **Every small misstep** — course-correct with prompts before reaching for revert

---

## Context Window Hygiene

Both tools have finite context. Quality degrades as it fills up. Signs you need to clear:

- The agent starts contradicting earlier instructions
- It forgets what files it already modified
- Responses become slower or less relevant

### Preventing context overflow

- Use `@filename` references instead of pasting file contents
- Use progressive disclosure in `AGENT.md` (short root file, detailed docs loaded on demand)
- For deep investigation, ask the agent to use a subagent so the main context stays clean
- When context exceeds ~60%, consider clearing and resuming with a summary

### Resuming after a clear

```text
"Read AGENT.md and docs/TASKS.md. We were working on [feature].
The last completed step was [X]. Continue from step [Y]."
```

If sessions run long, keep a `docs/SESSION.md` scratch file where the agent logs what was done and what's next. Ask it to update this before you end the session.

---

## Quick Reference Checklist

```text
Before each session:
□ Decide: resume, new session, or fork?
□ If switching tasks → /clear (Claude Code) or new session (OpenCode)

During long sessions:
□ Keep docs/SESSION.md updated with progress and next steps
□ Fork before risky experiments
□ Revert inside forks — don't touch the main session

After forking:
□ Pull insights back to main session manually
□ Don't let orphaned forks pile up — clean up old ones
```
