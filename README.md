# IA-Instructions

### Optimize your AI coding assistants — spend less, get better answers

A structured collection of instructions, configurations, and optimization guides for AI coding tools. Each AI assistant gets its own folder with curated, self-contained files that the AI reads **only when needed** — no wasted tokens on irrelevant context.

---

## Why This Exists

| Problem | Impact | This repo's solution |
|---------|--------|---------------------|
| AI reads your entire codebase on every prompt | 50K–100K tokens wasted per task | Scenario router → reads 1 targeted file |
| Outdated training data → hallucinated APIs | Wrong code, debug cycles | Context7 MCP → live docs injection |
| No structure in AI instructions | AI re-reads everything every turn | `CLAUDE.md` < 60 lines, cached every turn |
| Unused MCP servers sitting idle | 18K tokens/msg overhead per server | Config guides → only install what you use |
| Long sessions degrade quality | Accuracy drops after 60% context fill | Workflow patterns → `/compact`, `/clear`, scoped sessions |

**Result**: 5K–15K tokens per task instead of 50K–100K+. Up to **84% cost reduction**.

---

## Supported AI Tools

| Folder | AI Tool | Status | Files |
|--------|---------|--------|-------|
| [`claude-code/`](claude-code/) | Claude Code (Anthropic) | **37 files** — full coverage | Instructions, MCP configs, SDK guides, optimization |
| [`ChatGPT/`](ChatGPT/) | ChatGPT / Codex (OpenAI) | Coming soon | Planned: custom instructions, API patterns, plugins |
| [`Gemini/`](Gemini/) | Gemini Code Assist (Google) | Coming soon | Planned: context configs, extensions, API patterns |

---

## Project Structure

```
IA-Instructions/
│
├── README.md                          ← You are here
├── LICENSE                            ← GPL-3.0
│
├── claude-code/                       ← Claude Code optimization (active)
│   ├── README.md                      ← Overview + quick start for Claude Code
│   └── instructions/                  ← 37 self-contained instruction files
│       ├── router.md                  ← Scenario router: which file to read when
│       ├── mcp-config-example.json    ← Copy-paste MCP server config
│       │
│       ├── context7/                  ← MCP: live library documentation
│       │   ├── README.md              ← Install + what it does
│       │   ├── usage-guide.md         ← Prompts, library IDs, version targeting
│       │   └── config-and-tools.md    ← MCP tools, CLI, API key setup
│       │
│       ├── sequential-thinking/       ← MCP: structured step-by-step reasoning
│       │   ├── README.md              ← Install + when to use vs. not
│       │   ├── tool-reference.md      ← Parameters, revision, branching
│       │   └── prompt-patterns.md     ← 10 prompt examples + token economics
│       │
│       ├── claude-code-optimization/  ← Token saving, workflows, CLAUDE.md
│       │   ├── README.md              ← Top 3 optimizations + cost model
│       │   ├── token-saving.md        ← 10 techniques ranked by impact
│       │   ├── commands-shortcuts.md  ← Every slash command + keyboard shortcut
│       │   ├── claudemd-guide.md      ← How to write CLAUDE.md correctly
│       │   ├── skills-subagents.md    ← Skills, subagents, hooks config
│       │   ├── workflow-patterns.md   ← Explore→Plan→Implement→Commit
│       │   ├── claudeignore-template  ← Ready-to-copy .claudeignore
│       │   └── CLAUDE-OPTIMIZED.md    ← Template CLAUDE.md
│       │
│       ├── claudectx/                 ← CLI: token audit and optimization
│       │   ├── README.md              ← Install + token breakdown table
│       │   ├── commands.md            ← All 13 commands with flags
│       │   ├── mcp-server.md          ← Symbol-level file reads (97% savings)
│       │   └── workflow.md            ← Daily routine: analyze→optimize→watch
│       │
│       ├── anthropic-cookbook/         ← Official Claude API patterns
│       │   ├── README.md              ← Index + all notebook links
│       │   ├── prompt-caching.md      ← 90% savings on repeated context
│       │   ├── json-mode.md           ← 4 techniques for reliable JSON output
│       │   ├── sql-generation.md      ← Natural language → SQL + safety
│       │   ├── tool-use.md            ← Function calling + agentic loops
│       │   └── api-patterns.md        ← Streaming, errors, batch API
│       │
│       ├── claude-sdks/               ← Official Anthropic SDKs
│       │   ├── README.md              ← 4-SDK comparison table
│       │   ├── api-sdk-python.md      ← Messages, streaming, tools, batch
│       │   ├── agent-sdk-python.md    ← Agent SDK, custom tools, MCP
│       │   └── choosing-sdk.md        ← Decision flowchart
│       │
│       └── mcp-servers/               ← MCP ecosystem overview
│           └── README.md              ← Config locations, tier 1/2/3 servers
│
├── ChatGPT/                           ← ChatGPT/Codex optimization (planned)
│   └── README.md                      ← Roadmap + contribution guide
│
└── Gemini/                            ← Gemini Code Assist optimization (planned)
    └── README.md                      ← Roadmap + contribution guide
```

---

## Real Performance Data (April 2026)

### Token cost by model

| Model | SWE-bench Verified | Context | Input $/MTok | Output $/MTok |
|-------|-------------------|---------|-------------|---------------|
| **Claude Opus 4.6** | 80.8% | 1M | $5 | $25 |
| **Claude Sonnet 4.6** | 79.6% | 1M | $3 | $15 |
| Claude Haiku 4.5 | — | 1M | $1 | $5 |
| Gemini 3.1 Pro | 80.6% | 2M | $1.25 | $5 |
| GPT-5.4 | 75.1% (Terminal) | 200K | $2.50 | $10 |

*Sources: [Anthropic Pricing](https://platform.claude.com/docs/en/about-claude/pricing), [SWE-bench Leaderboard](https://www.swebench.com/), [Morph AI Costs 2026](https://www.morphllm.com/ai-coding-costs)*

### Before vs. after optimization

| Metric | Before | After | Savings |
|--------|--------|-------|---------|
| Tokens per request | ~18K | ~3.7K | **79%** |
| Daily cost (moderate use) | $20–40 | $5–12 | **60–70%** |
| Cache hit rate | 12% | 74% | **6x improvement** |
| 20-turn session cost (100K ctx) | $6.00 | $0.95 | **84%** |
| Context re-read waste | 98.5% | <20% | **80% reduction** |

*Sources: [Claude Code Cache Analysis](https://dev.to/kitaekatt/mastering-cache-hits-in-claude-code-5648), [claudectx benchmarks](https://github.com/Horilla/claudectx), [Ryan Doser Token Tips](https://ryandoser.com/claude-code-usage-tips/)*

### Optimization techniques ranked by impact

| # | Technique | Savings | Folder |
|---|-----------|---------|--------|
| 1 | Prompt caching | **90%** on repeated context | `anthropic-cookbook/prompt-caching.md` |
| 2 | Batch API | **50%** per request | `anthropic-cookbook/api-patterns.md` |
| 3 | `/compact` sessions | **40–60%** per long session | `claude-code-optimization/token-saving.md` |
| 4 | Scoped sessions | **30–50%** per session | `claude-code-optimization/workflow-patterns.md` |
| 5 | `.claudeignore` | **20–40%** on indexing | `claude-code-optimization/claudeignore-template` |
| 6 | Disconnect unused MCPs | **18K tokens/msg** per server | `mcp-servers/README.md` |
| 7 | Structured `CLAUDE.md` | **10–20%** always | `claude-code-optimization/claudemd-guide.md` |

---

## Quick Start

### For Claude Code users

```bash
# 1. Clone
git clone https://github.com/David-A18/IA-Instructions.git

# 2. Copy instructions to your project
cp -r IA-Instructions/claude-code/instructions/ /your/project/

# 3. Add to your CLAUDE.md
echo '## Instructions
- Read `instructions/router.md` to find the right file for your task
- Do NOT read all files — only the one matching your current situation' >> CLAUDE.md

# 4. Install recommended MCP servers
# Copy config from claude-code/instructions/mcp-config-example.json
```

See [`claude-code/README.md`](claude-code/README.md) for the full setup guide.

---

## How the Scenario Router Works

```
┌──────────────────────────────────────────────┐
│  AI gets a task from the user                │
│                    ↓                         │
│  Reads CLAUDE.md (< 60 lines, always cached) │
│                    ↓                         │
│  Checks router.md (scenario lookup table)    │
│                    ↓                         │
│  Opens ONLY 1 file matching the situation    │
│                    ↓                         │
│  Executes with targeted context              │
│  5K–15K tokens instead of 50K–100K+          │
└──────────────────────────────────────────────┘
```

The router contains three tables: **writing code**, **optimizing the AI**, **configuring MCP servers**. The AI scans the table, matches its situation, reads one file.

---

## Verified Resources

| Resource | URL | Stars |
|----------|-----|-------|
| Claude Code | https://github.com/anthropics/claude-code | 113k+ |
| MCP Servers | https://github.com/modelcontextprotocol/servers | 83k+ |
| Context7 | https://github.com/upstash/context7 | 52k+ |
| Claude Code Best Practices | https://github.com/shanraisshan/claude-code-best-practice | 43k+ |
| Anthropic Cookbook | https://github.com/anthropics/anthropic-cookbook | 40k+ |
| Anthropic Python SDK | https://github.com/anthropics/anthropic-sdk-python | 3.2k+ |
| Claude Agent SDK | https://github.com/anthropics/claude-agent-sdk-python | 6.3k+ |
| claudectx | https://github.com/Horilla/claudectx | New |

---

## Contributing

1. Fork the repo
2. Pick a folder (`claude-code/`, `ChatGPT/`, or `Gemini/`)
3. Add or improve instruction files (keep each < 200 lines)
4. If adding to `claude-code/`, update `router.md` with the new scenario
5. PR with a description of what situation your file addresses

---

## Author

**David A** — AWS / Cloud / IA / DevOps Engineer

## License

GPL-3.0 — see [LICENSE](LICENSE)
