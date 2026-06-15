# OpenAI API Structured Outputs And Tools

Use this route when building with OpenAI APIs, tools, structured outputs, agents, or model routing.

## Design Rules

- Define the task goal, allowed tools, and output contract explicitly.
- Use structured outputs when application code depends on response shape.
- Keep stable instructions before dynamic user input and retrieved context.
- Use tools for actions or fresh data, not for static knowledge already in the prompt.
- Log model, prompt version, tool calls, latency, errors, and token usage for production paths.

## Agent Workflow Rules

- Give each agent a single responsibility.
- Use handoffs for clear ownership changes.
- Keep tool permissions as narrow as possible.
- Add evals for repeated workflows before broad rollout.

## Prompt Governance

Every reusable prompt should define:

- owner
- purpose
- input variables
- output contract
- allowed tools
- failure modes
- validation or eval criteria

## When To Look Up Current Docs

Use official OpenAI docs for current model names, API fields, pricing, and SDK behavior. Do not rely on stale model memory for those facts.
