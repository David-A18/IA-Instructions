# Gemini Code Assist — Optimization Kit

> **Status**: Planned — contributions welcome

## What this folder will contain

Instruction files to optimize Google's Gemini Code Assist and Gemini API for coding tasks, following the same pattern as [`claude-code/`](../claude-code/):

- **Context configuration** — `.gemini/` config files, style guides, code customization
- **API patterns** — Gemini API (generateContent, function calling, structured output, grounding)
- **Extensions** — useful Gemini extensions for development
- **Token optimization** — context management across Gemini's 2M token window
- **Code Assist settings** — IDE-specific configs for VS Code, JetBrains, Cloud Shell

## Planned structure

```
Gemini/
├── README.md                  ← You are here
├── instructions/
│   ├── router.md              ← Scenario router (same pattern as Claude Code)
│   ├── context-config/        ← .gemini/ files, style guides
│   ├── api-patterns/          ← Gemini API: content generation, tools, grounding
│   ├── code-assist/           ← IDE settings, code customization
│   └── token-optimization/    ← Managing the 2M context window
```

## Key differences from Claude Code

| Feature | Claude Code | Gemini Code Assist |
|---------|------------|-------------------|
| Config file | `CLAUDE.md` | `.gemini/styleguide.md` + `.gemini/settings.json` |
| MCP support | Native | Via extensions |
| Context window | 1M tokens | **2M tokens** (Gemini 3.1 Pro) |
| CLI tool | Claude Code CLI | `gemini` CLI |
| Pricing model | Per-token API | Per-token API (cheaper per MTok) |
| Coding benchmark | 80.8% SWE-bench | 80.6% SWE-bench |

## Contributing

1. Fork the repo
2. Add instruction files to `Gemini/instructions/`
3. Follow the pattern: one file per scenario, < 200 lines, self-contained
4. Update this README with actual content descriptions
5. PR with the scenario your file addresses
