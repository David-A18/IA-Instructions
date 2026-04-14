# MCP Servers — Extending Claude Code Capabilities

## What is MCP

Model Context Protocol — how Claude Code connects to external tools. Servers give Claude new abilities without custom code.

## Links

- **Official servers**: https://github.com/modelcontextprotocol/servers (83k+ stars)
- **TypeScript SDK**: https://github.com/modelcontextprotocol/typescript-sdk
- **Directory**: https://www.claudemcp.org
- **Awesome list**: https://github.com/whitmorelabs/awesome-mcp-servers-1

## Config file locations

| Environment | File |
|-------------|------|
| Cursor IDE | `.cursor/mcp.json` (project root) |
| Claude Code CLI | `.claude/settings.json` (project) or `~/.claude/settings.json` (global) |
| Claude Desktop (Win) | `%APPDATA%\Claude\claude_desktop_config.json` |
| Claude Desktop (Mac) | `~/Library/Application Support/Claude/claude_desktop_config.json` |

## Our MCP stack

### Tier 1: Installed

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

See `context7/` and `sequential-thinking/` folders for full usage docs.

### Tier 2: Recommended for our projects

| Server | Purpose | Package |
|--------|---------|---------|
| claudectx MCP | Symbol-level file reads (97% fewer tokens) | `claudectx mcp --install` |
| GitHub MCP | PR creation, code review, issues | `@modelcontextprotocol/server-github` |
| PostgreSQL MCP | Database querying, schema inspection | `@modelcontextprotocol/server-postgres` |

### Tier 3: Consider later

| Server | Purpose |
|--------|---------|
| Playwright | Browser automation for testing |
| Docker MCP | Container management |
| Filesystem MCP | Controlled file access outside workspace |

## Verify servers are working

```
/mcp
```

Shows all connected servers and available tools.

## Add a new server

1. Find it at https://www.claudemcp.org
2. Add config to your MCP JSON file
3. Restart Claude Code / Cursor
4. Tools appear automatically
