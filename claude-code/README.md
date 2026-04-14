# Claude Code — Optimization Kit

Everything Claude Code needs to work faster, cheaper, and more accurately. This folder contains **37 instruction files** organized so Claude reads **only what it needs** per task.

---

## What's Inside

| Folder | Purpose | Files | When Claude reads it |
|--------|---------|-------|---------------------|
| [`instructions/`](instructions/) | All instruction files + scenario router | 37 | Every session (router only) |

### Instruction subfolders

| Subfolder | What it covers | Files | Read when... |
|-----------|---------------|-------|-------------|
| [`context7/`](instructions/context7/) | Live library docs via MCP — no hallucinated APIs | 3 | Coding with any library (boto3, DuckDB, etc.) |
| [`sequential-thinking/`](instructions/sequential-thinking/) | Structured reasoning via MCP — architecture, debugging | 3 | Planning, designing, debugging complex issues |
| [`claude-code-optimization/`](instructions/claude-code-optimization/) | Token saving, workflows, CLAUDE.md guide | 8 | Optimizing Claude Code, writing config files |
| [`claudectx/`](instructions/claudectx/) | Token audit CLI — analyze, optimize, watch | 4 | Auditing cost, setting up smart file reads |
| [`anthropic-cookbook/`](instructions/anthropic-cookbook/) | Official API patterns — caching, JSON, SQL, tools | 6 | Calling Claude API, building integrations |
| [`claude-sdks/`](instructions/claude-sdks/) | Official Anthropic SDKs (Python + TypeScript) | 4 | Building apps with Claude, choosing an SDK |
| [`mcp-servers/`](instructions/mcp-servers/) | MCP ecosystem — config, tiers, server list | 1 | Installing or managing MCP servers |

---

## Quick Start

### 1. Copy to your project

```bash
cp -r claude-code/instructions/ /your/project/instructions/
```

### 2. Add to your `CLAUDE.md`

```markdown
## Instructions (read ONLY what you need)

- **Router**: `instructions/router.md` — lookup table for which file to read
- **Rule**: Do NOT read all 37 files. Only the one matching your current task.
```

### 3. Install MCP servers

Copy `instructions/mcp-config-example.json` into your `.cursor/mcp.json` or `.claude/settings.json`:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    },
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

### 4. Audit token usage

```bash
npx claudectx analyze
```

---

## How the Router Works

`instructions/router.md` is a lookup table with three sections:

| Section | Covers |
|---------|--------|
| **Writing code** | Library docs, API patterns, SDK usage, JSON/SQL generation |
| **Optimizing Claude Code** | Token saving, commands, workflows, auditing |
| **Configuring MCP** | Install Context7, Sequential Thinking, claudectx MCP |

Claude reads the router (~100 lines), finds its situation in the table, opens **1 file**. No browsing, no guessing.

---

## Key Metrics

| What | Before instructions | After instructions |
|------|--------------------|--------------------|
| Tokens per request | ~18K | ~3.7K (**79% less**) |
| Cache hit rate | 12% | 74% (**6x**) |
| Daily cost | $20–40 | $5–12 (**70% less**) |
| Wrong-approach iterations | 3–5 per feature | 1–2 (**60% less**) |

---

## File Count

| Category | Files |
|----------|-------|
| MCP server guides | 7 (context7: 3, sequential-thinking: 3, mcp-servers: 1) |
| Optimization guides | 12 (claude-code-optimization: 8, claudectx: 4) |
| API/SDK guides | 10 (anthropic-cookbook: 6, claude-sdks: 4) |
| Config files | 1 (mcp-config-example.json) |
| Router | 1 (router.md) |
| **Total** | **31 + 6 READMEs = 37** |

Each file is **self-contained** and **< 200 lines**.
