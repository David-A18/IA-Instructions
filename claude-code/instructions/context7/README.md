# Context7 — Live Documentation MCP Server

## Links

- **Repo**: https://github.com/upstash/context7 (52k+ stars, MIT)
- **Website**: https://context7.com
- **Dashboard**: https://context7.com/dashboard (free API key for higher rate limits)
- **NPM**: `@upstash/context7-mcp` | CLI: `ctx7`
- **Docs**: https://context7.com/docs/installation

## Files in this folder

| File | Content |
|------|---------|
| `README.md` | This index + install + quick start |
| `usage-guide.md` | How to use in prompts, library IDs, version targeting, tips |
| `config-and-tools.md` | MCP tools reference, CLI commands, manual config, rules |

## What it does

Context7 fetches **up-to-date, version-specific documentation** for any library and injects it directly into your AI context. No more hallucinated APIs, deprecated methods, or outdated code examples.

| Without Context7 | With Context7 |
|-------------------|--------------|
| Code examples from year-old training data | Current docs from official sources |
| Hallucinated APIs that don't exist | Real APIs, version-specific |
| Generic answers for old package versions | Exact version you're using |
| Follow-up corrections waste tokens | Right answer first time |

## Install (one command)

```bash
npx ctx7 setup
```

Auto-detects your environment (Cursor, Claude Code, Claude Desktop) and configures everything. Authenticates via OAuth and generates an API key.

Use `--cursor`, `--claude`, or `--opencode` to target a specific agent.

## Manual MCP config

Add to `.cursor/mcp.json`, `.claude/settings.json`, or `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    }
  }
}
```

For higher rate limits, set `CONTEXT7_API_KEY` env variable (free at context7.com/dashboard).
