---
name: mcp-servers
description: Use when configuring, adding, or troubleshooting MCP (Model Context Protocol) servers. Covers server selection, configuration guidelines, and security best practices.
---

# MCP Server Rules

> **MCP (Model Context Protocol) connects the agent to external tools — GitHub, databases, web search, Notion, etc.**

---

## Where to Configure

| Scope | Location | Use for |
| --- | --- | --- |
| Project | `.claude/mcp.json` or `opencode.json` | Team-shared integrations |
| Global | `~/.claude.json` or `~/.config/opencode/` | Personal tools across all projects |

**Add via CLI:**

```bash
# Claude Code
claude mcp add github

# OpenCode
# Configure in opencode.json under "mcp" key
```

---

## Guidelines

- Install only what the project actually needs. Each MCP server consumes context tokens
- Keep to 5–8 MCP servers maximum per session
- Never hardcode API keys in MCP config files — use environment variables
- Add GitHub MCP early if your project involves issues, PRs, or repo management

---

## Security Rules

- Never hardcode API keys, tokens, or secrets in MCP configuration
- Use environment variables for all authentication
- Review MCP server permissions before enabling
- Disable MCP servers that are not actively needed

---

## Common MCP Servers

| Server | Purpose | When to use |
| --- | --- | --- |
| GitHub | Issues, PRs, repo management | Always — early setup recommended |
| Database | Query databases directly | When working with data-heavy features |
| Web search | Search documentation, current info | When agent needs external context |
| Filesystem | Extended file operations | When built-in tools are insufficient |
| Notion | Project management integration | When project uses Notion for docs |

---

## Troubleshooting

- **Server not connecting:** Check authentication and network access
- **High token usage:** Remove unused MCP servers — each one consumes context
- **Unexpected behavior:** Review server permissions and scope of access
- **Auth errors:** Re-authenticate using the CLI (`claude mcp auth <name>` or equivalent)
