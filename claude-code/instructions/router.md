# Instruction Router — Read ONLY what you need

> **STOP.** Do NOT read all files in this folder.
> Find your situation in the tables below → read that ONE file.

---

## I'm writing code

| Situation | Read this file |
|-----------|---------------|
| Need a library's API docs (boto3, DuckDB, Typer, Jinja2, etc.) | [`context7/usage-guide.md`](context7/usage-guide.md) |
| Need to plan architecture or a complex feature | [`sequential-thinking/prompt-patterns.md`](sequential-thinking/prompt-patterns.md) |
| Need Claude API patterns (streaming, tools, caching) | [`anthropic-cookbook/api-patterns.md`](anthropic-cookbook/api-patterns.md) |
| Need reliable JSON output from Claude | [`anthropic-cookbook/json-mode.md`](anthropic-cookbook/json-mode.md) |
| Need SQL generation from natural language | [`anthropic-cookbook/sql-generation.md`](anthropic-cookbook/sql-generation.md) |
| Need tool calling / function calling patterns | [`anthropic-cookbook/tool-use.md`](anthropic-cookbook/tool-use.md) |
| Need prompt caching to reduce API costs | [`anthropic-cookbook/prompt-caching.md`](anthropic-cookbook/prompt-caching.md) |
| Need to build an autonomous agent | [`claude-sdks/agent-sdk-python.md`](claude-sdks/agent-sdk-python.md) |
| Need to call Claude API from Python | [`claude-sdks/api-sdk-python.md`](claude-sdks/api-sdk-python.md) |
| Not sure which SDK to use | [`claude-sdks/choosing-sdk.md`](claude-sdks/choosing-sdk.md) |

## I'm optimizing Claude Code

| Situation | Read this file |
|-----------|---------------|
| Session feels slow or expensive | [`claude-code-optimization/token-saving.md`](claude-code-optimization/token-saving.md) |
| Need a command or keyboard shortcut | [`claude-code-optimization/commands-shortcuts.md`](claude-code-optimization/commands-shortcuts.md) |
| Writing or improving a CLAUDE.md file | [`claude-code-optimization/claudemd-guide.md`](claude-code-optimization/claudemd-guide.md) |
| Setting up skills, subagents, or hooks | [`claude-code-optimization/skills-subagents.md`](claude-code-optimization/skills-subagents.md) |
| Need the right workflow pattern for a task | [`claude-code-optimization/workflow-patterns.md`](claude-code-optimization/workflow-patterns.md) |
| Want to audit token usage | [`claudectx/commands.md`](claudectx/commands.md) |
| Want to auto-optimize everything | [`claudectx/commands.md`](claudectx/commands.md) → `optimize` |
| Want symbol-level file reads (97% savings) | [`claudectx/mcp-server.md`](claudectx/mcp-server.md) |
| Need a daily optimization routine | [`claudectx/workflow.md`](claudectx/workflow.md) |
| Need a `.claudeignore` template | [`claude-code-optimization/claudeignore-template`](claude-code-optimization/claudeignore-template) |

## I'm configuring MCP servers

| Situation | Read this file |
|-----------|---------------|
| Install Context7 (live library docs) | [`context7/config-and-tools.md`](context7/config-and-tools.md) |
| Install Sequential Thinking (structured reasoning) | [`sequential-thinking/tool-reference.md`](sequential-thinking/tool-reference.md) |
| Install claudectx MCP (smart file reads) | [`claudectx/mcp-server.md`](claudectx/mcp-server.md) |
| Overview of all MCP servers + tiers | [`mcp-servers/README.md`](mcp-servers/README.md) |
| Copy-paste MCP config JSON | [`mcp-config-example.json`](mcp-config-example.json) |

---

## Folder index

| Folder | Files | Purpose |
|--------|-------|---------|
| `context7/` | 3 | Live library docs injection via MCP |
| `sequential-thinking/` | 3 | Structured reasoning via MCP |
| `claude-code-optimization/` | 8 | Token saving, workflows, config templates |
| `claudectx/` | 4 | Token audit CLI + MCP server |
| `anthropic-cookbook/` | 6 | Official Claude API patterns |
| `claude-sdks/` | 4 | Anthropic SDKs (Python + TypeScript) |
| `mcp-servers/` | 1 | MCP ecosystem overview + config |
