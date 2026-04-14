# Agent SDK Python — `claude-agent-sdk`

Repo: https://github.com/anthropics/claude-agent-sdk-python
Docs: https://code.claude.com/docs/en/agent-sdk/python

## Install

```bash
pip install claude-agent-sdk
```

Requires Claude Code CLI installed on the machine.

## Two interfaces

| Interface | Session | Use case |
|-----------|---------|----------|
| `query()` | New each time | One-off tasks, scripts, automation |
| `ClaudeSDKClient` | Persistent | Multi-turn conversations, chat UIs |

## query() — one-shot tasks

```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    async for message in query(
        prompt="Find and fix the bug in finops/ingestion/cur_parser.py",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "Edit", "Bash", "Glob"],
            permission_mode="acceptEdits",
            cwd="/path/to/project-01",
        ),
    ):
        print(message)

asyncio.run(main())
```

### ClaudeAgentOptions

| Option | Type | Description |
|--------|------|-------------|
| `system_prompt` | `str` | Custom system prompt for the agent |
| `allowed_tools` | `list[str]` | Tools auto-approved without user permission |
| `disallowed_tools` | `list[str]` | Tools explicitly blocked |
| `permission_mode` | `str` | `"default"`, `"acceptEdits"`, `"fullAuto"` |
| `cwd` | `str` | Working directory |
| `max_turns` | `int` | Maximum conversation turns |
| `model` | `str` | Model to use |

### Built-in tools

`Read`, `Write`, `Edit`, `Bash`, `Grep`, `Glob`, `WebSearch`

### Tool permissions

- `allowed_tools` → auto-approved (no user prompt)
- `disallowed_tools` → blocked entirely
- Unlisted tools → fall through to `permission_mode`

## ClaudeSDKClient — multi-turn conversations

```python
import asyncio
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions

async def main():
    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Bash"],
        permission_mode="acceptEdits",
    )
    
    async with ClaudeSDKClient(options=options) as client:
        # First turn
        async for msg in client.send_message("What files are in finops/analysis/?"):
            print(msg)
        
        # Follow-up (same context)
        async for msg in client.send_message("Now implement anomaly_detector.py"):
            print(msg)

asyncio.run(main())
```

## Custom MCP tools

Define tools that the agent can call:

```python
from claude_agent_sdk import tool, query, ClaudeAgentOptions, create_sdk_mcp_server

@tool("query_costs", "Query cost data from DuckDB", {"sql": str})
async def query_costs(args):
    from finops.ingestion.local_store import LocalStore
    store = LocalStore("finops.duckdb")
    results = store.query(args["sql"])
    store.close()
    return {"content": [{"type": "text", "text": str(results)}]}

@tool("get_anomalies", "Detect cost anomalies", {"start_date": str, "end_date": str})
async def get_anomalies(args):
    # your anomaly detection logic
    return {"content": [{"type": "text", "text": "No anomalies found"}]}

# Create MCP server with your tools
server = create_sdk_mcp_server(
    name="finops-tools",
    version="1.0.0",
    tools=[query_costs, get_anomalies],
)

# Use the agent with your custom tools
async def main():
    async for msg in query(
        prompt="What services had the highest cost increase last week?",
        options=ClaudeAgentOptions(
            allowed_tools=["Read", "query_costs", "get_anomalies"],
            mcp_servers={"finops": server},
        ),
    ):
        print(msg)

asyncio.run(main())
```

### @tool decorator

```python
@tool(
    name="tool_name",
    description="What the tool does",
    input_schema={"param": str, "count": int},  # simple types
    annotations=ToolAnnotations(readOnlyHint=True),  # optional hints
)
async def my_tool(args: dict) -> dict:
    return {"content": [{"type": "text", "text": "result"}]}
```

**Input schema options:**
- Simple: `{"text": str, "count": int, "enabled": bool}`
- JSON Schema: `{"type": "object", "properties": {...}, "required": [...]}`

## Relevance to our project

The Agent SDK enables building a FinOps agent that:
- Reads CUR data from DuckDB via custom `query_costs` tool
- Detects anomalies via `get_anomalies` tool
- Generates rightsizing recommendations
- Creates PRs with Terraform fixes
- Responds to natural language: "What's driving our cost increase?"

This is the path to making FinOps Autopilot conversational.
