# Claude Code Optimization — Spend Less, Get Better Answers

## Key references

- **Usage & limits (official)**: https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code
- **Best practices repo**: https://github.com/shanraisshan/claude-code-best-practice (43k+ stars)
- **Claude Code repo**: https://github.com/anthropics/claude-code (113k+ stars)
- **Blueprint**: https://github.com/faizkhairi/claude-code-blueprint
- **Cheatsheet**: https://cc.bruniaux.com/guide/cheatsheet/
- **Cost guide**: https://maxtechera.dev/en/courses/claude-code-mastery/06-production/03-cost-optimization

## Files in this folder

| File | Content |
|------|---------|
| `README.md` | This index + quick reference |
| `token-saving.md` | Token cost model, 10 techniques, common mistakes |
| `usage-metering.md` | Web vs Code usage, why % ≠ token math, 44K myth, /cost |
| `commands-shortcuts.md` | All commands, keyboard shortcuts, slash commands |
| `claudemd-guide.md` | How to write an effective CLAUDE.md (rules, structure, limits) |
| `skills-subagents.md` | Skills, subagents, hooks, commands, rules — configuration guide |
| `workflow-patterns.md` | Explore → Plan → Implement → Commit + debugging patterns |
| `claudeignore-template` | Ready-to-copy .claudeignore file |
| `CLAUDE-OPTIMIZED.md` | Template CLAUDE.md (legacy, see actual CLAUDE.md at repo root) |

## Token cost model

| Type | Cost | Ratio |
|------|------|-------|
| Input tokens | $3 / 1M | 1x |
| Output tokens | $15 / 1M | **5x** |
| Cache write | $3.75 / 1M | 1.25x |
| Cache read | $0.30 / 1M | **0.10x** |

Strategy: minimize verbose output, maximize prompt caching, use `.claudeignore`.

## The 3 highest-impact optimizations

1. **Keep CLAUDE.md under 60 lines** — it loads on every single request
2. **`/compact` at 50% context** — performance degrades sharply after 60%
3. **Use Plan Mode** for anything > 30 min — avoids wasted code generation
