# ChatGPT / Codex — Optimization Kit

> **Status**: Planned — contributions welcome

## Ready-to-run research prompt

| File | Use |
|------|-----|
| [`PROMPT-DEEP-RESEARCH-OBSIDIAN-CLAUDE-CODE-EFFICIENCY.md`](PROMPT-DEEP-RESEARCH-OBSIDIAN-CLAUDE-CODE-EFFICIENCY.md) | **Copy-paste into ChatGPT (GPT-5.5+)** — deep research brief: Obsidian × Anthropic (Claude Code, Skills, Hooks, Subagents, MCP) × third-party tools, token economics, architectures, security, 90-day roadmap. |

## What this folder will contain

Instruction files to optimize ChatGPT and OpenAI Codex for coding tasks, following the same pattern as [`claude-code/`](../claude-code/):

- **Custom instructions** — system prompts optimized for coding efficiency
- **API patterns** — OpenAI API usage (chat completions, function calling, structured outputs)
- **GPT configuration** — custom GPT setup for development workflows
- **Token optimization** — context management, prompt compression, response control
- **Codex CLI** — configuration and workflow patterns for OpenAI's Codex CLI tool
- **Plugin/tool configs** — useful plugins and tool integrations

## Planned structure

```
ChatGPT/
├── README.md                  ← You are here
├── instructions/
│   ├── router.md              ← Scenario router (same pattern as Claude Code)
│   ├── custom-instructions/   ← System prompts for coding
│   ├── api-patterns/          ← OpenAI API: completions, tools, structured output
│   ├── codex-cli/             ← Codex CLI optimization
│   └── token-optimization/    ← Context management, prompt engineering
```

## Key differences from Claude Code

| Feature | Claude Code | ChatGPT/Codex |
|---------|------------|----------------|
| Config file | `CLAUDE.md` | Custom instructions / system prompt |
| MCP support | Native | Via plugins/tools |
| Context window | 1M tokens | 200K tokens (GPT-5.4) |
| CLI tool | Claude Code CLI | Codex CLI |
| Pricing model | Per-token API | Subscription + API |

## Contributing

1. Fork the repo
2. Add instruction files to `ChatGPT/instructions/`
3. Follow the pattern: one file per scenario, < 200 lines, self-contained
4. Update this README with actual content descriptions
5. PR with the scenario your file addresses
