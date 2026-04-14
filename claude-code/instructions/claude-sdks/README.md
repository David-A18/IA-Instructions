# Claude SDKs — Official Anthropic Libraries

## Overview

Anthropic maintains **4 official SDKs** in two categories:

| Category | Python | TypeScript |
|----------|--------|-----------|
| **API SDK** (call Claude) | `anthropic` | `@anthropic-ai/sdk` |
| **Agent SDK** (build agents) | `claude-agent-sdk` | `claude-agent-sdk` |

## Files in this folder

| File | Content |
|------|---------|
| `README.md` | This index + quick comparison |
| `api-sdk-python.md` | API SDK — messages, streaming, tools, async, error handling |
| `agent-sdk-python.md` | Agent SDK — query(), ClaudeSDKClient, custom tools, MCP servers |
| `choosing-sdk.md` | Decision guide: which SDK for which use case |

## Install

```bash
# API SDK (call Claude models)
pip install anthropic

# Agent SDK (build autonomous agents)
pip install claude-agent-sdk
```

## Quick decision

| You want to... | Use |
|----------------|-----|
| Send prompts, get responses | API SDK (`anthropic`) |
| Stream responses token by token | API SDK (`anthropic`) |
| Use tool calling / function calling | API SDK (`anthropic`) |
| Build an agent that reads/writes files autonomously | Agent SDK (`claude-agent-sdk`) |
| Create custom MCP tools | Agent SDK (`claude-agent-sdk`) |
| Build a multi-turn chat with persistent context | Agent SDK (`ClaudeSDKClient`) |

## Repos

| SDK | Repo | Stars | Install |
|-----|------|-------|---------|
| API Python | https://github.com/anthropics/anthropic-sdk-python | 3.2k | `pip install anthropic` |
| API TypeScript | https://github.com/anthropics/anthropic-sdk-typescript | 1.8k | `npm install @anthropic-ai/sdk` |
| Agent Python | https://github.com/anthropics/claude-agent-sdk-python | 6.3k | `pip install claude-agent-sdk` |
| Agent TypeScript | https://github.com/anthropics/claude-agent-sdk-typescript | 1.3k | `npm install claude-agent-sdk` |

## Docs

- **API SDK docs**: https://docs.anthropic.com
- **Agent SDK docs**: https://code.claude.com/docs/en/agent-sdk/python
- **Full docs index**: https://code.claude.com/docs/llms.txt
