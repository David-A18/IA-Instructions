# Claude Code Instruction Router

Read this file first. Do not read the full instruction tree.

Choose the one row that matches the current task, then open only that file. If the task changes, return here and choose again.

## Coding Tasks

| Situation | Read |
| --- | --- |
| Need Claude API, tool use, JSON, SQL, streaming, or batch guidance | [instructions/coding/claude-api-patterns.md](instructions/coding/claude-api-patterns.md) |
| Need current library/framework documentation | [instructions/context/context7.md](instructions/context/context7.md) |
| Need a planning, debugging, or review prompt pattern | [instructions/prompts/sequential-thinking.md](instructions/prompts/sequential-thinking.md) |

## Claude Code Operations

| Situation | Read |
| --- | --- |
| Session is expensive, slow, bloated, or drifting | [instructions/optimization/token-and-session-hygiene.md](instructions/optimization/token-and-session-hygiene.md) |
| Need workflow rules for explore, plan, implement, review, or commit | [instructions/optimization/workflow.md](instructions/optimization/workflow.md) |
| Need skills, subagents, hooks, or native Claude Code memory guidance | [instructions/optimization/extensibility.md](instructions/optimization/extensibility.md) |

## MCP And Templates

| Situation | Read |
| --- | --- |
| Need MCP server policy or setup | [instructions/mcp/mcp-policy.md](instructions/mcp/mcp-policy.md) |
| Need copyable `CLAUDE.md` or settings examples | [instructions/templates/README.md](instructions/templates/README.md) |

## Compatibility

Older detailed references remain under folders such as `anthropic-cookbook/`, `claude-code-optimization/`, `context7/`, and `sequential-thinking/`. Open them only when a routed task file sends you there.
