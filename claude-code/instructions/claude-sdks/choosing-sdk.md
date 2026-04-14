# Choosing the Right SDK

## Decision flowchart

```
Do you need Claude to autonomously read/write files?
├── YES → Agent SDK (claude-agent-sdk)
│   ├── One-off task? → query()
│   └── Multi-turn chat? → ClaudeSDKClient
└── NO → API SDK (anthropic)
    ├── Need streaming? → client.messages.stream()
    ├── Need tool calling? → client.messages.create(tools=[...])
    ├── Batch processing? → client.messages.batches.create()
    └── Simple request? → client.messages.create()
```

## Comparison

| Feature | API SDK (`anthropic`) | Agent SDK (`claude-agent-sdk`) |
|---------|----------------------|-------------------------------|
| Send messages, get responses | Yes | Yes |
| Streaming | Yes | Yes |
| Tool calling (function calling) | Yes (you execute) | Yes (agent executes) |
| File read/write | No (you code it) | Built-in (Read, Write, Edit) |
| Run shell commands | No | Built-in (Bash) |
| Search files | No | Built-in (Grep, Glob) |
| Custom MCP tools | No | Yes (@tool decorator) |
| Multi-turn context | Manual (you manage messages[]) | Automatic (ClaudeSDKClient) |
| Requires Claude Code CLI | No | Yes |
| Pricing | Pay per API call | Uses Claude Code subscription |

## For our FinOps project

| Phase | SDK | Why |
|-------|-----|-----|
| **Week 1-6 (CLI tool)** | Neither — we use Claude Code directly | Building the tool, not calling Claude API |
| **Future: API endpoints** | API SDK | Expose cost queries via REST API |
| **Future: Chat interface** | Agent SDK | "What's driving our cost increase?" → agent queries DuckDB |
| **Future: Batch reports** | API SDK (Batch) | Process monthly CUR reports at 50% cost |
| **Future: Automated fixes** | Agent SDK | Agent detects anomaly → creates PR → sends alert |
