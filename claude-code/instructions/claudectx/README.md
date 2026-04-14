# claudectx — Token Audit & Optimization CLI

## Links

- **NPM**: https://www.npmjs.com/package/claudectx (v1.1.5, MIT)
- **Repo**: https://github.com/Horilla/claudectx
- **Website**: https://claudectx.horilla.com

## Files in this folder

| File | Content |
|------|---------|
| `README.md` | This index + install + quick start |
| `commands.md` | All 13 commands with full flags and examples |
| `mcp-server.md` | Smart MCP proxy — symbol-level reads (97% fewer tokens) |
| `workflow.md` | Daily workflow: analyze → optimize → watch → compress → report |

## Install

```bash
# Try immediately (no install)
npx claudectx analyze

# Install globally
npm install -g claudectx

# Homebrew
brew tap Horilla/claudectx && brew install claudectx
```

## Quick start (3 commands)

```bash
# 1. See where your tokens go
npx claudectx analyze

# 2. Fix everything automatically
npx claudectx optimize --apply

# 3. Start smart MCP server
npx claudectx mcp --install
```

## Where tokens go (typical project)

| Component | Tokens | % | Fixable? |
|-----------|--------|---|----------|
| Claude Code system prompt | 4,200 | 14% | No |
| Tool definitions | 2,100 | 7% | No |
| MCP tool schemas | 1,980 | 7% | Partially |
| **CLAUDE.md** | **6,800-14,000** | **22-46%** | **YES — biggest win** |
| MEMORY.md | 3,300 | 11% | YES |
| Conversation history | 2,000-40,000 | 7-57% | YES |

## Expected results

| Metric | Before | After |
|--------|--------|-------|
| Tokens per request | ~18K | ~3.7K |
| Cache hit rate | 12% | 74% |
| Session compress | 8,000 tokens | 180 tokens (97.8%) |
| Smart read vs full file | 12,000 tokens | 800 tokens (93%) |
