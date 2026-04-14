# Instructions Index — Read ONLY what you need

> **IMPORTANT**: Do NOT read all files. Use the scenario table below to find the
> exact file for your current task. Each file is self-contained.

## Scenario routing — what to read and when

### I'm working on code (most common)

| Situation | Read this file |
|-----------|---------------|
| Need a library's API docs (boto3, DuckDB, Typer, etc.) | `context7/usage-guide.md` |
| Need to plan architecture or complex feature | `sequential-thinking/prompt-patterns.md` |
| Need Claude API patterns (streaming, tools, caching) | `anthropic-cookbook/api-patterns.md` |
| Need to generate JSON output from Claude | `anthropic-cookbook/json-mode.md` |
| Need SQL generation from natural language | `anthropic-cookbook/sql-generation.md` |
| Need tool calling / function calling patterns | `anthropic-cookbook/tool-use.md` |
| Need prompt caching to reduce costs | `anthropic-cookbook/prompt-caching.md` |
| Need to build an autonomous agent | `claude-sdks/agent-sdk-python.md` |
| Need to call Claude API from Python | `claude-sdks/api-sdk-python.md` |
| Not sure which SDK to use | `claude-sdks/choosing-sdk.md` |

### I'm optimizing Claude Code itself

| Situation | Read this file |
|-----------|---------------|
| Session feels slow or expensive | `claude-code-optimization/token-saving.md` |
| Need a command or shortcut | `claude-code-optimization/commands-shortcuts.md` |
| Writing or improving CLAUDE.md | `claude-code-optimization/claudemd-guide.md` |
| Setting up skills, subagents, or hooks | `claude-code-optimization/skills-subagents.md` |
| Need the right workflow for a task | `claude-code-optimization/workflow-patterns.md` |
| Want to audit token usage | `claudectx/commands.md` → `analyze` |
| Want to auto-optimize everything | `claudectx/commands.md` → `optimize` |
| Want symbol-level file reads (save tokens) | `claudectx/mcp-server.md` |
| Daily optimization routine | `claudectx/workflow.md` |

### I'm configuring MCP servers

| Situation | Read this file |
|-----------|---------------|
| Install Context7 | `context7/config-and-tools.md` |
| Install Sequential Thinking | `sequential-thinking/tool-reference.md` |
| Install claudectx MCP | `claudectx/mcp-server.md` |
| Overview of all MCP servers | `mcp-servers/README.md` |
| Copy-paste MCP config | `mcp-config-example.json` |

## File tree (37 files across 7 folders)

```
instructions/
├── README.md                          ← YOU ARE HERE (scenario router)
├── mcp-config-example.json            ← copy-paste MCP config
├── PROMPT-START-PROJECT-C.md          ← prompt to start building FinOps Autopilot
│
├── context7/                          ← live library docs (MCP)
│   ├── README.md                      ← install + overview
│   ├── usage-guide.md                 ← prompts, library IDs, version targeting
│   └── config-and-tools.md            ← MCP tools, CLI, config, API key
│
├── sequential-thinking/               ← structured reasoning (MCP)
│   ├── README.md                      ← install + when to use
│   ├── tool-reference.md              ← tool params, revision, branching, config
│   └── prompt-patterns.md             ← prompt examples, token economics
│
├── claude-code-optimization/          ← token saving + workflow
│   ├── README.md                      ← index + top 3 optimizations
│   ├── token-saving.md                ← 10 techniques ranked by impact
│   ├── commands-shortcuts.md          ← every command + keyboard shortcut
│   ├── claudemd-guide.md             ← how to write CLAUDE.md correctly
│   ├── skills-subagents.md            ← skills, subagents, hooks, rules config
│   ├── workflow-patterns.md           ← Explore→Plan→Implement→Commit + debugging
│   ├── claudeignore-template          ← ready-to-copy .claudeignore
│   └── CLAUDE-OPTIMIZED.md            ← template CLAUDE.md (legacy)
│
├── claudectx/                         ← token audit CLI tool
│   ├── README.md                      ← install + token breakdown
│   ├── commands.md                    ← all 13 commands with flags
│   ├── mcp-server.md                  ← smart_read, search_symbols, index
│   └── workflow.md                    ← daily routine: analyze→optimize→watch
│
├── anthropic-cookbook/                 ← official Claude API patterns
│   ├── README.md                      ← index + all notebook links
│   ├── prompt-caching.md              ← automatic + explicit caching (90% savings)
│   ├── json-mode.md                   ← 4 techniques for reliable JSON
│   ├── sql-generation.md              ← natural language → SQL + safety
│   ├── tool-use.md                    ← function calling + agentic loop
│   └── api-patterns.md               ← streaming, errors, batch API, models
│
├── claude-sdks/                       ← official Anthropic SDKs
│   ├── README.md                      ← 4-SDK comparison + install
│   ├── api-sdk-python.md              ← messages, streaming, tools, caching, batch
│   ├── agent-sdk-python.md            ← query(), ClaudeSDKClient, custom tools
│   └── choosing-sdk.md               ← decision flowchart
│
└── mcp-servers/                       ← MCP ecosystem overview
    └── README.md                      ← config locations, tier 1/2/3 servers
```
