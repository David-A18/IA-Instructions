# Context7 Configuration & Tools Reference

## MCP tools

Context7 provides 2 tools to Claude/Cursor when configured as an MCP server:

### `resolve-library-id`

Resolves a library name into a Context7-compatible ID.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | User's question (used to rank results) |
| `libraryName` | string | Yes | Library name to search for |

Returns: matching libraries with ID (format `/org/project`), name, description, snippet count, source reputation, benchmark score, available versions.

### `query-docs`

Fetches documentation for a resolved library.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `libraryId` | string | Yes | Context7 library ID (e.g., `/duckdb/duckdb`) |
| `query` | string | Yes | Question to get relevant docs for |
| `tokens` | number | No | Token limit for response |

Returns: up-to-date documentation and code examples.

## CLI commands

```bash
# Search for a library
ctx7 library boto3

# Get docs for a specific library
ctx7 docs /boto/boto3 "how to use S3 get_object"
```

## Configuration options

### Option 1: MCP server (recommended)

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

### Option 2: Remote MCP server

Use the Context7 server URL directly:

```
URL: https://mcp.context7.com/mcp
Header: CONTEXT7_API_KEY: your-api-key
```

### Option 3: CLI + Skills (no MCP)

```bash
npx ctx7 setup --claude
```

Installs a skill file that guides Claude to use `ctx7` CLI commands.

## Config file locations

| Environment | Config file |
|-------------|------------|
| Cursor | `.cursor/mcp.json` (project) or Cursor Settings > MCP |
| Claude Code CLI | `.claude/settings.json` (project) or `~/.claude/settings.json` (global) |
| Claude Desktop | `%APPDATA%\Claude\claude_desktop_config.json` (Windows) |
| Claude Desktop (Mac) | `~/Library/Application Support/Claude/claude_desktop_config.json` |

## API key (optional, recommended)

Free API key at https://context7.com/dashboard — gives higher rate limits and priority access.

```bash
# Set as environment variable
export CONTEXT7_API_KEY="your-key-here"
```

Or pass in MCP config:

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"],
      "env": {
        "CONTEXT7_API_KEY": "your-key-here"
      }
    }
  }
}
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "Library not found" | Try a more specific name or search at context7.com |
| Rate limited | Get free API key at context7.com/dashboard |
| MCP not connecting | Check `npx` is available and Node.js >= 18 installed |
| Stale docs | Context7 crawls sources regularly; report at context7.com if outdated |
