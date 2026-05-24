---
name: debugging
description: Use when something breaks — debugging agent-generated code, diagnosing errors, or recovering from failed implementations. Covers explain-before-fix pattern, failure modes, and diagnostic loop.
---

# Debugging Rules

> **The agent will produce bugs. So will you. Debug effectively.**

---

## The Debugging Mindset

When something breaks, you have two possible sources:

1. Code the agent wrote (wrong implementation or logic)
2. Instructions you gave (unclear or missing context)

Most beginners blame the agent. Usually, the issue is in how they asked.

**Before you debug the code, ask:**
- Did I clearly describe what I wanted?
- Did I provide enough context?
- Did I mention constraints the agent might have missed?

---

## The Explain-Before-Fix Pattern

When the agent produces wrong output, don't just say "fix it." Ask it to explain first.

```text
"This output isn't what I expected. Before you fix it, explain:
1. What your code does, step by step
2. Why you chose this approach
3. Where the bug likely is"
```

This reveals the agent's reasoning. Often the bug is a wrong assumption you can correct directly.

---

## Common Failure Modes

| Failure Mode | Symptoms | Fix |
| --- | --- | --- |
| Hallucinated APIs | Import errors, "function not found" | Tell the agent exactly which libraries and versions you're using |
| Wrong assumptions about stack | Agent assumes different framework | State your stack explicitly in every prompt |
| Overengineering | Complex solution for simple problem | Set scope explicitly: "Keep this simple" |
| Conflating tasks | Mixes previous task with current | Clear session between unrelated tasks |
| Ignoring error messages | Agent skips or misinterprets errors | Paste the exact error into your prompt |

---

## The Diagnostic Loop

When something breaks, run this loop:

1. **Run the code** — see the actual error
2. **Read the error message** — understand what happened
3. **Ask the agent to explain** — "What does this error mean?"
4. **Fix or guide** — either let the agent fix it, or direct it specifically
5. **Verify** — run again to confirm the fix

**Never accept "it should work now" without running the code.**

---

## Using Git to Recover

Git is your safety net. When the agent produces something unusable:

### Discard all changes

```bash
git checkout .
```

This resets to the last commit. Start fresh with a better prompt.

### See what changed

```bash
git diff
```

Review every line the agent modified. You might find it did something unexpected.

### Go back to a specific commit

```bash
git log --oneline -10
git checkout <commit-hash> -- .
```

Useful when you want to keep some changes but not others.

---

## When to Restart vs Course-Correct

| Situation | Action |
| --- | --- |
| Agent misunderstood the goal | Course-correct — explain the goal differently |
| Agent keeps going in circles | Course-correct — narrow the scope |
| Agent produced completely wrong code | Course-correct — ask it to explain first |
| Agent is confused about project state | Restart — clear context and re-explain |
| Multiple fixes didn't help | Restart — `git checkout .` and start over |

The nuclear option:

```text
"Let's start over. Run git checkout . to reset, then I'll give you a clearer prompt."
```

---

## Debugging Checklist

```text
□ Run the code and see the actual error
□ Read the error message carefully
□ Ask: did I give the agent enough context?
□ Ask the agent to explain its reasoning
□ Fix at the right level (prompt vs code)
□ Verify the fix by running again
□ Commit the working state
□ If it broke again, document what changed
```

---

## Preventing Bugs in the First Place

The best debugging is not needing to debug. These habits prevent issues:

- **Plan-first** — catch misunderstandings before code is written
- **Small steps** — don't let the agent run for 20 minutes unattended
- **Run tests after every change** — tell the agent to do this automatically
- **Review every file** — don't accept code you haven't read
- **Keep AGENT.md accurate** — outdated context causes wrong assumptions
