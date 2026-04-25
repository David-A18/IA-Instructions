# Claude Code — Optimization Kit

Curated **`instructions/`** so Claude Code reads **one targeted file per task** (lower tokens, fewer mistakes). The folder is also a small **Obsidian vault** for humans (`_MOC.md`, `.obsidian/`).

**Strategy & repo roadmap:** see [`../analysis.md`](../analysis.md) (cockpit/executor split, hooks/skills, MCP policy, monorepo router naming).

---

## What's inside

| Path | Role |
|------|------|
| [`instructions/`](instructions/) | **Router** (`router.md`), topic guides, MCP sample, **Obsidian** hub |

### Topic folders

| Folder | Topics | Files |
|--------|--------|-------|
| [`context7/`](instructions/context7/) | Live library docs (MCP) | 3 |
| [`sequential-thinking/`](instructions/sequential-thinking/) | Structured reasoning (MCP) | 3 |
| [`claude-code-optimization/`](instructions/claude-code-optimization/) | Tokens, **usage metering**, workflows, `CLAUDE.md` | 9 |
| [`claudectx/`](instructions/claudectx/) | Token audit CLI + MCP | 4 |
| [`anthropic-cookbook/`](instructions/anthropic-cookbook/) | API patterns (cache, JSON, SQL, tools) | 6 |
| [`claude-sdks/`](instructions/claude-sdks/) | Python / TS SDKs | 4 |
| [`mcp-servers/`](instructions/mcp-servers/) | MCP ecosystem overview | 1 |

**Root of `instructions/`:** `router.md`, `_MOC.md`, `OBSIDIAN.md`, `mcp-config-example.json`, `.gitignore`, `.obsidian/*` (3 JSON). **~38 files total.**

---

## Quick start

### 1. Copy into your repo

```bash
cp -r claude-code/instructions/ /your/project/instructions/
```

### 2. Point `CLAUDE.md` at the router

```markdown
## Instructions
- **Router:** `instructions/router.md` — one file per task.
- **Optional:** `instructions/_MOC.md` — wikilink map / Obsidian only; not every turn.
```

### 3. MCP (optional)

Merge `instructions/mcp-config-example.json` into `.claude/settings.json` or `.cursor/mcp.json`.

### 4. Audit usage (optional)

```bash
npx claudectx analyze
```

---

## Obsidian

1. **Open folder as vault** → select `claude-code/instructions/`.
2. Open **`_MOC.md`** → **Local graph** to jump between linked notes.
3. Details: **`OBSIDIAN.md`**.

Claude Code: **always `router.md` first**; `_MOC.md` when exploring many linked notes.

---

## Router behavior

Three tables in **`router.md`**: coding, optimizing Claude Code, MCP setup. Match the row → open **one** linked file.

---

## Metrics (typical)

| | Before | After guides + hygiene |
|--|--------|-------------------------|
| Tokens / request | ~18K | ~3.7K |
| Cache hit rate | ~12% | ~74% |

---

## File roll-up

| Bucket | Count |
|--------|-------|
| Markdown guides (all subfolders + router) | 32 |
| Hub + config + Obsidian + ignore | 6 |
| **Total** | **38** |

Keep each guide **self-contained** and **under ~200 lines** where possible.
