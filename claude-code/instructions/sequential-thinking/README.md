# Sequential Thinking — Structured Reasoning MCP Server

## Links

- **Source**: https://github.com/modelcontextprotocol/servers (83k+ stars)
- **Package**: `@modelcontextprotocol/server-sequential-thinking`
- **Docker**: `docker run --rm -i mcp/sequentialthinking`

## Files in this folder

| File | Content |
|------|---------|
| `README.md` | This index + install + when to use |
| `tool-reference.md` | Full tool parameters, revision, branching, config options |
| `prompt-patterns.md` | Prompt examples for architecture, debugging, planning + token economics |

## What it does

Official MCP server that externalizes Claude's reasoning into **auditable, revisable, step-by-step thought sequences**. Instead of a black-box answer, you get numbered steps with the ability to revise, branch, and adjust depth dynamically.

| Standard reasoning | Sequential thinking |
|-------------------|-------------------|
| Hidden internal state | Transparent, auditable steps |
| Stateless per turn | Persistent thought history |
| Requires re-prompting to correct | Explicit revision + backtracking |
| Single solution path | Non-linear branching logic |

## Install

```bash
npx -y @modelcontextprotocol/server-sequential-thinking
```

## MCP config

Add to `.cursor/mcp.json`, `.claude/settings.json`, or `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

## When to use it

| Use it for | Don't use it for |
|-----------|-----------------|
| Architecture design | Simple code fixes |
| Multi-file refactoring plans | Renaming a variable |
| Debugging intermittent bugs | Known one-liner bugs |
| Choosing between approaches | Obvious implementations |
| System design decisions | Small features (< 30 min) |
| Migration planning | CRUD endpoints |

## Why this saves tokens

- Gets architecture right on the **first attempt** → no "redo differently" cycles
- Pairs with Plan Mode (`/plan`) → validate approach before code generation
- Explores alternatives via branching → avoids implementing wrong approach
- Average: **2-3 fewer implementation iterations** for complex tasks
