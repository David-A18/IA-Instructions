# Claude API Pattern Route

Use this route when the task involves Claude API calls, structured output, tool calling, prompt caching, SQL generation, streaming, or batch execution.

## Read Only What Applies

| Need | Detailed reference |
| --- | --- |
| Client setup, messages, streaming, errors, batch API, token counting | [../anthropic-cookbook/api-patterns.md](../anthropic-cookbook/api-patterns.md) |
| Reliable JSON or structured output behavior | [../anthropic-cookbook/json-mode.md](../anthropic-cookbook/json-mode.md) |
| Tool/function calling and agentic tool loops | [../anthropic-cookbook/tool-use.md](../anthropic-cookbook/tool-use.md) |
| Prompt caching and stable context ordering | [../anthropic-cookbook/prompt-caching.md](../anthropic-cookbook/prompt-caching.md) |
| Natural language to SQL patterns | [../anthropic-cookbook/sql-generation.md](../anthropic-cookbook/sql-generation.md) |
| Choosing API SDK vs Agent SDK | [../claude-sdks/choosing-sdk.md](../claude-sdks/choosing-sdk.md) |

## Rules

- Prefer the smallest API surface that satisfies the task.
- Put stable system instructions and tool schemas before dynamic user content.
- Use explicit output contracts when downstream code depends on shape.
- Use batch APIs only for offline or latency-tolerant work.
- Do not embed large examples globally; route to the specific reference instead.
