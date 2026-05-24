# RULES.md — Vibe Coding Rules for This Project

> **Source:** Extracted and adapted from the [Vibe Coding Playbook](https://github.com/plamen5rov/vibe-coding-playbook) — consensus patterns from 10 AI LLMs, refined for production-grade context engineering projects.

---

## Core Operating Rules

### 1. Plan Before Coding

Before the agent writes a single line, ask it to write a plan.

```text
"Before touching code, write a plan to docs/SPEC.md and wait for my approval."
```

This is the single most effective way to avoid long debugging sessions caused by the agent misunderstanding your intent. Once you approve the plan, the agent executes it. If it goes wrong, you have a written spec to refer back to.

### 2. Work in Small Iterations

Never ask the agent to build an entire feature in one prompt.

```text
❌ Bad:  "Build the entire authentication system."
✅ Good: "Add a login form that calls POST /api/auth/login and handles errors."
```

Check in every few steps. Course-correct early — redirecting is much faster than debugging a large block of wrong code.

### 3. Use Git as Your Undo Button

Commit before any significant agent action. If the output is bad, `git checkout .` resets everything instantly.

```bash
git add . && git commit -m "checkpoint before agent refactor"
# let the agent work
# review the result
# if bad: git checkout .
# if good: git add . && git commit -m "feat: ..."
```

### 4. Clear Context Between Tasks

Both tools accumulate context from previous work. When switching to a new unrelated task, clear the session.

- **Claude Code:** `/clear`
- **OpenCode:** start a new session

Accumulated irrelevant context wastes token budget and can cause the agent to conflate different tasks.

### 5. Treat the Agent as a Fast Junior Developer

You are the architect. The agent is a very fast implementer. Guide it with clear specs, review its output, and own the result. Accepting AI output without review leads to security issues and accumulated technical debt.

---

## Context File Rules

### The Minimum Set

For any new project, create these before running `opencode` or `claude` for the first time:

| File | Purpose | Length |
| --- | --- | --- |
| `AGENT.md` | Main context — read every session | Under 200 lines |
| `README.md` | Human-readable project overview | As needed |
| `docs/PROJECT.md` | Goals, audience, constraints | 1 page |
| `docs/TASKS.md` | Current work items | Living doc |
| `.gitignore` | What to exclude from git | Standard |

### Keep AGENT.md Under 200 Lines

Longer files start being skimmed, not read — and both tools inject this file as a system reminder that the model may deprioritize if it grows too large.

Every line should pass this test: *"Would removing this line cause the agent to make mistakes?"*

### Use Progressive Disclosure for Complex Projects

Don't cram everything into `AGENT.md`. Reference additional files when they're needed:

```markdown
## References
- When working on payments: read @docs/payments.md
- When modifying the API: read @docs/api-conventions.md
- When writing tests: read @docs/testing.md
```

This way, `AGENT.md` stays short and the agent loads detailed context only when the task requires it.

### What to Exclude from AGENT.md

- Exhaustive documentation of every file
- Instructions that only apply to one rare task
- Coding style rules already enforced by a linter
- API documentation (link to a doc file instead)

---

## Git & GitHub Rules

### Create the Repo Yourself — Always

**Create the GitHub repo first, then clone locally.**

```bash
# Option A: GitHub CLI (recommended)
gh repo create my-project --private
git clone https://github.com/yourname/my-project.git
cd my-project
```

**Why not let the AI create the repo?**

- You lose control over visibility, license, and `.gitignore` templates
- Auth and remote-origin setup is less reliable when agent-initialized
- Both OpenCode and Claude Code work best when the remote already exists

### Use Feature Branches, Even Solo

- OpenCode and Claude Code write directly to your working directory. If the agent goes off-track on `main`, you have no clean rollback point
- A branch lets you `git checkout main && git branch -D bad-branch` to discard everything and start fresh
- Frequent small commits give you rollback points

### What to Commit, What to Ignore

| File / Path | Commit? | Why |
| --- | --- | --- |
| `AGENT.md` | ✅ Yes | Core context — always commit |
| `opencode.json` (project-level) | ✅ Yes | Project config, no secrets |
| `.opencode/commands/*.md` | ✅ Yes | Slash commands — share across machines |
| `.opencode/agents/*.md` | ✅ Yes | Project-specific agents |
| `skills/*/SKILL.md` | ✅ Yes | Domain-specific rules — share with team |
| `docs/` context files | ✅ Yes | Part of project intelligence |
| `.env` | ❌ No | Secrets — never commit |
| `.env.*` (except `.env.example`) | ❌ No | Secrets |
| `node_modules/`, `dist/`, `build/` | ❌ No | Generated — always ignore |

### The `.env.example` Pattern

Commit this file with all variable names but empty values. It shows the AI (and your future self) what environment variables exist without exposing secrets.

```bash
# .env.example — commit this
DATABASE_URL=
API_KEY=
SECRET_TOKEN=

# .env — never commit this
DATABASE_URL=postgres://user:pass@localhost/mydb
API_KEY=sk-abc123...
SECRET_TOKEN=supersecret
```

---

## Prompt Engineering Rules

### The Anatomy of a Good Prompt

Every effective prompt has four parts:

| Part | What it does | Example |
| --- | --- | --- |
| **Context** | What the agent needs to know | "Read AGENT.md and docs/TASKS.md" |
| **Goal** | What you want done | "Add user authentication" |
| **Constraints** | What to avoid or limit | "No external dependencies" |
| **Format** | How you want the output | "Write a plan in SPEC.md first" |

A prompt missing any of these parts is incomplete.

### The Plan-Then-Execute Pattern

This is the single most effective prompt pattern in vibe coding.

```text
"Before touching any code, write a plan to docs/SPEC.md that covers:
1. What files need changing
2. The approach you'll take
3. Any risks or edge cases
Wait for my approval before implementing."
```

This forces the agent to think before acting. You catch misunderstandings in the plan phase — where they cost 30 seconds to fix, not hours in the debug phase.

### How to Correct the Agent Without Restarting

When the agent goes wrong, you don't need to start over. Guide it back:

**Point out the specific issue:**
```text
"That's not quite right — I need X, not Y. Please revise."
```

**Provide missing context:**
```text
"I forgot to mention: the API uses a Python Flask backend, not Express."
```

**Request a smaller step:**
```text
"Stop. Just do the first part — create the file and one function. Don't proceed further until I say."
```

### Pro Tips

- **One task per prompt.** If you ask for three things, the agent may forget one. Break it up
- **Use the agent's output.** If the agent writes something good, keep it. Don't rewrite unnecessarily
- **Tell the agent what you liked.** "That approach worked well — use the same pattern for the next feature."
- **Be specific about file paths.** "Edit src/api/users.py, not just 'the user file'."
- **State constraints explicitly.** "Use only the dependencies already in package.json" is clearer than "keep it simple."

---

## Debugging Rules

### The Explain-Before-Fix Pattern

When the agent produces wrong output, don't just say "fix it." Ask it to explain first.

```text
"This output isn't what I expected. Before you fix it, explain:
1. What your code does, step by step
2. Why you chose this approach
3. Where the bug likely is"
```

This reveals the agent's reasoning. Often the bug is a wrong assumption you can correct directly.

### Common Failure Modes

| Failure Mode | Symptoms | Fix |
| --- | --- | --- |
| Hallucinated APIs | Import errors, "function not found" | Tell the agent exactly which libraries and versions you're using |
| Wrong assumptions about stack | Agent assumes different framework | State your stack explicitly in every prompt |
| Overengineering | Complex solution for simple problem | Set scope explicitly: "Keep this simple" |
| Conflating tasks | Mixes previous task with current | Clear session between unrelated tasks |
| Ignoring error messages | Agent skips or misinterprets errors | Paste the exact error into your prompt |

### The Diagnostic Loop

When something breaks, run this loop:

1. **Run the code** — see the actual error
2. **Read the error message** — understand what happened
3. **Ask the agent to explain** — "What does this error mean?"
4. **Fix or guide** — either let the agent fix it, or direct it specifically
5. **Verify** — run again to confirm the fix

**Never accept "it should work now" without running the code.**

### When to Restart vs Course-Correct

| Situation | Action |
| --- | --- |
| Agent misunderstood the goal | Course-correct — explain the goal differently |
| Agent keeps going in circles | Course-correct — narrow the scope |
| Agent produced completely wrong code | Course-correct — ask it to explain first |
| Agent is confused about project state | Restart — clear context and re-explain |
| Multiple fixes didn't help | Restart — `git checkout .` and start over |

---

## Session Management Rules

### Resume vs. Starting Fresh

**Resume when:**
- Picking up where you left off on the same task
- Continuing work after a break
- Building deep context for a complex project

**Start fresh when:**
- Switching to a completely unrelated task
- Context is stale or confusing the AI
- You need a clean slate for a different project or branch

### Forking a Session

Forking copies your entire session history up to that point, then starts an independent session from there. The original session stays unchanged.

**When to fork:**
- Testing risky ideas — try a new architecture without polluting your main session
- Parallel experiments — run multiple "what if" scenarios from the same starting point
- Exploring tangents — prototype a feature while your primary session stays focused

**When not to fork:**
- Simple continuations — resume the original instead to build deeper context
- Every minor change — each fork duplicates history and burns extra tokens

### Context Window Hygiene

Both tools have finite context. Quality degrades as it fills up. Signs you need to clear:

- The agent starts contradicting earlier instructions
- It forgets what files it already modified
- Responses become slower or less relevant

**Preventing context overflow:**
- Use `@filename` references instead of pasting file contents
- Use progressive disclosure in `AGENT.md` (short root file, detailed docs loaded on demand)
- When context exceeds ~60%, consider clearing and resuming with a summary

---

## Security Hygiene Rules

- Never put API keys, passwords, or tokens in files the agent can read
- Use `.env` files (gitignored) and reference via environment variables only
- Add `.env.example` (committed) to show what variables are needed without exposing values
- In OpenCode, use the permission system to deny file reads for sensitive files
- Treat all external input as untrusted

---

## Cost & Model Rules

### Which Model to Use When

| Task | Recommended Model |
| --- | --- |
| Planning, exploration | Cheap model (Haiku, GPT-4o-mini) |
| Writing simple code | Mid-tier (Sonnet, GPT-4o) |
| Complex architecture, debugging | Premium (Opus) |
| Writing tests | Mid-tier |
| Refactoring | Mid-tier |
| Reading and explaining code | Mid-tier |
| Debugging tricky issues | Premium |

**Pattern:** Use a cheap model to plan, then switch to a capable model to implement.

### Cost-Saving Strategies

1. **Use context efficiently** — Don't paste file contents. Use `@filename` references
2. **Clear context between tasks** — Context you don't need costs money
3. **Use the right model for the task** — Don't use Opus for something Haiku can do
4. **Set budget alerts** — Most providers have usage dashboards. Check weekly
5. **Try local models for simple tasks** — Ollama is free after setup

---

## Slash Commands & Agents

### Custom Slash Commands

Custom slash commands are markdown files that trigger a pre-written prompt. Create them once, reuse forever.

```text
.opencode/commands/review.md     → /review
.opencode/commands/ship.md       → /ship
.opencode/commands/push.md       → /push
```

A `/ship` command that runs lint, tests, writes a commit message, and pushes — created once — saves enormous repeated prompting.

### When to Create Agents and Skills

- **In advance:** for anything you'll use repeatedly — a `reviewer`, a `planner`, slash commands for your core workflows
- **On the fly:** for one-off tasks. Let the agent create and run ad-hoc scripts
- **As patterns emerge:** when you find yourself giving the same 10-line explanation every session, that's a candidate for a skill or custom agent

---

## MCP Server Rules

MCP (Model Context Protocol) connects the agent to external tools — GitHub, databases, web search, Notion, etc.

### Guidelines

- Install only what the project actually needs. Each MCP server consumes context tokens
- Keep to 5–8 MCP servers maximum per session
- Never hardcode API keys in MCP config files — use environment variables
- Add GitHub MCP early if your project involves issues, PRs, or repo management

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
