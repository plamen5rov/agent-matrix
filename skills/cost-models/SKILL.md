---
name: cost-models
description: Use when selecting AI models for tasks, managing API costs, or optimizing token usage. Covers model tier selection, cost estimates, and cost-saving strategies.
---

# Cost & Model Selection Rules

> **API costs add up fast if you're not paying attention. Use the right model for the right task.**

---

## Which Model to Use When

Not every task needs the most expensive model.

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

---

## Cost Estimates

A typical session:
- **Prompt tokens:** ~5K (your instructions + context)
- **Context tokens:** ~50K (reading files, existing code)
- **Output tokens:** ~10K (code + explanations)

**Approximate cost for a 30-minute session** (verify current prices):
- Claude Sonnet (mid-tier): $0.30–0.50
- GPT-4o (mid-tier): $0.40–0.70
- Claude Opus / GPT-4 (premium): $1.00–2.00+

**Monthly estimate (10 sessions/week):**
- Cloud: $15-40/month
- With local models: $0 (after GPU investment)

---

## Cost-Saving Strategies

### 1. Use context efficiently

Don't paste file contents. Use `@filename` references:

```text
❌ Bad: "Here's the file contents: [pastes 500 lines]"

✅ Good: "Read @src/auth/login.ts and fix the issue."
```

### 2. Clear context between tasks

Context you don't need costs money. Clear sessions when switching topics.

### 3. Use the right model for the task

Don't use Opus for something Haiku can do.

### 4. Set budget alerts

Most providers have usage dashboards. Check weekly.

### 5. Try local models for simple tasks

Ollama is free after setup. Use it for exploration, simple edits, file searching.

---

## Model Recommendations by Use Case

### For beginners

Start with OpenCode Zen or Claude Sonnet. Good balance of capability and cost.

### For cost-conscious

Use local models (Ollama) with Qwen2.5-Coder or CodeLlama. Quality is decent for most tasks.

### For maximum capability

Claude Opus (current premium tier). Use for complex architecture, debugging, or when the task is high-value.

### For speed

Use Haiku or GPT-4o-mini for quick tasks where capability matters less than response time.

---

## When to Upgrade

Signs you need a more capable model:
- The cheap model keeps making the same mistakes
- It can't follow complex instructions
- It produces code that doesn't compile

Signs you can downgrade:
- The task is straightforward (simple edits, file reading)
- You're getting consistent results with a cheaper model
- You're trying to reduce costs

---

## Quick Reference

| Model tier | Examples | Best for |
| --- | --- | --- |
| Cheap | Haiku, GPT-4o-mini, Qwen2.5-Coder 7B | Planning, simple tasks |
| Mid-tier | Sonnet, GPT-4o | Most coding tasks |
| Premium | Opus | Complex architecture, debugging |

**Rule of thumb:** Start with mid-tier. Upgrade when you hit limitations. Downgrade when the task is simple.
