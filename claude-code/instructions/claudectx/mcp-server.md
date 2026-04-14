# claudectx MCP Server — Symbol-Level File Reads

## Problem

When Claude reads a file to find one function, it gets the **entire file**. A 500-line Django model = 12,000+ tokens. The actual class definition = 800 tokens.

## Solution

`claudectx mcp` is a local MCP server that intercepts file reads and returns only the requested symbol (function, class, method). **Up to 97% fewer tokens per read.**

## Install

```bash
# Start the MCP server
claudectx mcp

# Auto-add to Claude Code config
claudectx mcp --install
```

## Manual config

Add to `.claude/settings.json`:

```json
{
  "mcpServers": {
    "claudectx": {
      "command": "claudectx",
      "args": ["mcp"],
      "type": "stdio"
    }
  }
}
```

## Tools provided to Claude

### `smart_read`

Read a specific symbol by name, or a line range:

```
smart_read(file="finops/ingestion/local_store.py", symbol="LocalStore")
smart_read(file="finops/ingestion/cur_parser.py", symbol="parse_cur_csv")
smart_read(file="finops/cli.py", lines="22-51")
```

Returns only the requested symbol's code, not the entire file.

### `search_symbols`

Find where a symbol is defined without reading any files:

```
search_symbols(query="anomaly", scope="finops/")
```

Returns: file path, line number, symbol type (class/function/method).

### `index_project`

Build the symbol index for the current project:

```
index_project(path=".")
```

Run once after significant codebase changes. The index is cached locally.

## Token savings example

| Scenario | Full file read | smart_read | Savings |
|----------|---------------|------------|---------|
| Read `LocalStore` class from `local_store.py` | 3,200 tokens | 420 tokens | 87% |
| Read `parse_cur_csv` from `cur_parser.py` | 1,800 tokens | 280 tokens | 84% |
| Read one Terraform resource from `main.tf` | 2,100 tokens | 350 tokens | 83% |

## Relevance to our project

FinOps Autopilot has many files with multiple classes/functions. Smart reads for:
- Specific DuckDB queries from `local_store.py` (125 lines)
- Individual analysis functions from `anomaly_detector.py`
- Single Terraform resources from `main.tf` (80 lines)
