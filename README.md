# IA-Instructions

### Cut your Claude Code costs by up to 84% with structured instructions

A curated set of instruction files that make Claude Code **read only what it needs, when it needs it** — no wasted tokens on irrelevant context.

---

## The Problem (with real numbers)

| Metric | Without optimization | With optimization | Source |
|--------|---------------------|-------------------|--------|
| Average daily cost (moderate use) | $20–40/day | $5–12/day | [Morph AI Coding Costs 2026](https://www.morphllm.com/ai-coding-costs) |
| Cache efficiency per session | 0% (no caching) | **77–99.6%** cache hit rate | [Claude Code Cache Analysis](https://dev.to/kitaekatt/mastering-cache-hits-in-claude-code-5648) |
| Tokens wasted on context re-reads | **98.5%** of tokens in 100+ msg chats | <20% with `/compact` + scoped sessions | [Ryan Doser Token Tips 2026](https://ryandoser.com/claude-code-usage-tips/) |
| Input cost with prompt caching | $3–5/MTok (full price) | **$0.30–0.50/MTok** (cached reads) | [Anthropic Pricing](https://platform.claude.com/docs/en/about-claude/pricing) |
| MCP server context overhead | 18,000 tokens/message per unused server | 0 (disconnected) | [Medium: Claude Code Budget Fixes](https://medium.com/@0xmega/claude-code-is-eating-your-budget-7-fixes-that-cut-costs-without-killing-output-f8dbb3b8a04d) |
| Cost of a 20-turn session (100K ctx) | $6.00 | **$0.95** (84% reduction) | [Dev.to Cache Mastery](https://dev.to/kitaekatt/mastering-cache-hits-in-claude-code-5648) |

**90% of Claude Code users spend under $12/day** — but the top 10% running multi-file refactors spend $500–2,000/month. This project targets both groups.

---

## How This Works

```
┌─────────────────────────────────────────────────────┐
│  Claude Code gets a task                            │
│                    ↓                                │
│  Reads CLAUDE.md (50 lines, always cached)          │
│                    ↓                                │
│  Checks instructions/README.md (scenario router)    │
│                    ↓                                │
│  Reads ONLY the 1 file matching current task        │
│                    ↓                                │
│  Executes with full context, minimal tokens         │
└─────────────────────────────────────────────────────┘
```

**Traditional approach**: Claude reads everything → wastes 70% of tokens on irrelevant context.
**This approach**: Claude reads a lookup table → opens 1 file → gets exactly what it needs.

---

## Scenario Router (the core idea)

Claude Code doesn't read all 37 files. It reads `instructions/README.md` which contains three lookup tables:

### When writing code

| Situation | File to read |
|-----------|-------------|
| Need library API docs (boto3, DuckDB, etc.) | `context7/usage-guide.md` |
| Planning architecture or complex feature | `sequential-thinking/prompt-patterns.md` |
| Need Claude API patterns (streaming, tools) | `anthropic-cookbook/api-patterns.md` |
| Need JSON output from Claude | `anthropic-cookbook/json-mode.md` |
| Need SQL generation | `anthropic-cookbook/sql-generation.md` |
| Building an autonomous agent | `claude-sdks/agent-sdk-python.md` |

### When optimizing Claude Code

| Situation | File to read |
|-----------|-------------|
| Session feels slow or expensive | `claude-code-optimization/token-saving.md` |
| Need a command or shortcut | `claude-code-optimization/commands-shortcuts.md` |
| Writing CLAUDE.md | `claude-code-optimization/claudemd-guide.md` |
| Setting up skills, hooks, subagents | `claude-code-optimization/skills-subagents.md` |
| Auditing token usage | `claudectx/commands.md` |

### When configuring MCP servers

| Situation | File to read |
|-----------|-------------|
| Install Context7 (live library docs) | `context7/config-and-tools.md` |
| Install Sequential Thinking | `sequential-thinking/tool-reference.md` |
| Overview of all MCP servers | `mcp-servers/README.md` |

---

## Claude Code Performance (April 2026)

### Model benchmarks

| Model | SWE-bench Verified | Context | Input $/MTok | Output $/MTok |
|-------|-------------------|---------|-------------|---------------|
| **Claude Opus 4.6** | 80.8% | 1M tokens | $5 | $25 |
| **Claude Sonnet 4.6** | 79.6% | 1M tokens | $3 | $15 |
| Claude Haiku 4.5 | — | 1M tokens | $1 | $5 |
| Gemini 3.1 Pro | 80.6% | 2M tokens | $1.25 | $5 |
| GPT-5.4 | 75.1% (Terminal) | 200K tokens | $2.50 | $10 |

### Cost optimization stack

| Technique | Savings | How |
|-----------|---------|-----|
| Prompt caching | **90%** on repeated context | Automatic for content >1024 tokens |
| Batch API | **50%** per request | Async processing, 24h window |
| `/compact` sessions | **40–60%** per long session | Summarizes history, resets context |
| Scoped sessions | **30–50%** | One task per session, `/clear` between |
| `.claudeignore` | **20–40%** | Exclude node_modules, .git, builds |
| Disconnect unused MCPs | **18K tokens/msg** saved per server | Remove from config |
| Structured `CLAUDE.md` | **10–20%** | Cached on every turn, <60 lines |

**Combined maximum**: up to **95%** cost reduction vs. naive usage.

---

## File Structure

```
instructions/
├── README.md                              # Scenario router (read this first)
├── mcp-config-example.json                # Copy-paste MCP config
│
├── context7/                              # MCP: live library docs
│   ├── README.md                          # Install + overview
│   ├── usage-guide.md                     # Prompts, library IDs, version targeting
│   └── config-and-tools.md                # MCP tools, CLI, API key, config
│
├── sequential-thinking/                   # MCP: structured reasoning
│   ├── README.md                          # Install + when to use
│   ├── tool-reference.md                  # Parameters, revision, branching
│   └── prompt-patterns.md                 # 10 prompt examples, token economics
│
├── claude-code-optimization/              # Token saving + workflow
│   ├── README.md                          # Top 3 optimizations
│   ├── token-saving.md                    # 10 techniques ranked by impact
│   ├── commands-shortcuts.md              # Every command + shortcut
│   ├── claudemd-guide.md                  # How to write CLAUDE.md correctly
│   ├── skills-subagents.md                # Skills, subagents, hooks config
│   ├── workflow-patterns.md               # Explore→Plan→Implement→Commit
│   ├── claudeignore-template              # Ready-to-copy .claudeignore
│   └── CLAUDE-OPTIMIZED.md               # Template CLAUDE.md
│
├── claudectx/                             # Token audit CLI
│   ├── README.md                          # Install + token breakdown
│   ├── commands.md                        # All 13 commands with flags
│   ├── mcp-server.md                      # Symbol-level file reads
│   └── workflow.md                        # Daily routine
│
├── anthropic-cookbook/                     # Official Claude API patterns
│   ├── README.md                          # Index + notebook links
│   ├── prompt-caching.md                  # 90% savings guide
│   ├── json-mode.md                       # 4 techniques for reliable JSON
│   ├── sql-generation.md                  # NL → SQL + safety
│   ├── tool-use.md                        # Function calling + agentic loop
│   └── api-patterns.md                    # Streaming, errors, batch API
│
├── claude-sdks/                           # Anthropic SDKs
│   ├── README.md                          # 4-SDK comparison
│   ├── api-sdk-python.md                  # Messages, streaming, tools, batch
│   ├── agent-sdk-python.md                # Agent SDK, custom tools
│   └── choosing-sdk.md                    # Decision flowchart
│
└── mcp-servers/                           # MCP ecosystem
    └── README.md                          # Config locations, tier 1/2/3
```

**37 files** across **7 folders** — each self-contained, each <200 lines.

---

## Quick Start

### 1. Clone the repo

```bash
git clone https://github.com/David-A18/IA-Instructions.git
```

### 2. Copy `instructions/` to your project root

```bash
cp -r IA-Instructions/instructions/ /your/project/
```

### 3. Add to your `CLAUDE.md`

```markdown
## Instructions

- **Path**: `instructions/`
- **Router**: `instructions/README.md` — read this to find the right file
- **Rule**: Do NOT read all files. Only the one matching your current task.
```

### 4. Install MCP servers (optional but recommended)

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

### 5. Audit your token usage

```bash
npx claudectx analyze
```

---

## Daily Habits That Save Tokens

1. **`/clear`** every 30–45 minutes of active work
2. **`/compact`** when context grows but you need continuity
3. **`/plan`** before any feature that takes >1 hour
4. **Scoped prompts**: specify file paths, line numbers, exact function names
5. **"use context7"** when asking about specific libraries
6. **Sequential thinking** for architecture decisions and debugging

---

## Verified Resources

| Resource | URL | Stars |
|----------|-----|-------|
| Claude Code | https://github.com/anthropics/claude-code | 113k+ |
| MCP Servers | https://github.com/modelcontextprotocol/servers | 83k+ |
| Context7 | https://github.com/upstash/context7 | 52k+ |
| Best Practices Guide | https://github.com/shanraisshan/claude-code-best-practice | 43k+ |
| Anthropic Cookbook | https://github.com/anthropics/anthropic-cookbook | 40k+ |
| Anthropic Python SDK | https://github.com/anthropics/anthropic-sdk-python | 3.2k+ |
| Claude Agent SDK | https://github.com/anthropics/claude-agent-sdk-python | 6.3k+ |
| claudectx | https://github.com/Horilla/claudectx | New |

---

## How It Compares

| Approach | Tokens per task | Cost | Accuracy |
|----------|----------------|------|----------|
| No instructions (raw Claude Code) | 50K–100K+ | High | Variable |
| Single large CLAUDE.md (>200 lines) | 30K–60K | Medium | Good |
| **This project (scenario router)** | **5K–15K** | **Low** | **High** |

The key insight: Claude Code is most efficient when it reads **small, targeted context** instead of large, general-purpose files.

---

## Contributing

1. Fork the repo
2. Add or improve an instruction file (keep it <200 lines)
3. Update `instructions/README.md` scenario router if adding new files
4. PR with a clear description of what scenario the file addresses

---

## Author

**David A** — AWS/Cloud/IA/DevOps Engineer

## License

GPL-3.0 — see [LICENSE](LICENSE)
