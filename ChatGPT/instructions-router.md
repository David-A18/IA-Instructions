# ChatGPT And Codex Instruction Router

Read this file first. Do not read the full instruction tree.

Choose the one row that matches the task, then open only that file.

## Codex Work

| Situation | Read |
| --- | --- |
| Setting up or improving Codex repository instructions | [instructions/agents-md/minimal-agents-template.md](instructions/agents-md/minimal-agents-template.md) |
| Running Codex as a coding agent on a repo task | [instructions/codex/workflow.md](instructions/codex/workflow.md) |
| Need copyable `AGENTS.md` | [instructions/templates/AGENTS.md](instructions/templates/AGENTS.md) |

## OpenAI API Work

| Situation | Read |
| --- | --- |
| Need tools, structured output, model routing, or API prompt guidance | [instructions/openai-api/structured-outputs-and-tools.md](instructions/openai-api/structured-outputs-and-tools.md) |
| Need to manage context, file reads, or session scope | [instructions/context-management/context-hygiene.md](instructions/context-management/context-hygiene.md) |

## Research And Planning

| Situation | Read |
| --- | --- |
| Need a deep research brief about instruction routing, Obsidian, and Claude Code | [instructions/prompts/deep-research-obsidian-claude-code-efficiency.md](instructions/prompts/deep-research-obsidian-claude-code-efficiency.md) |

## Rules

- Prefer `AGENTS.md` for shared Codex repository instructions.
- Keep `AGENTS.md` small and specific.
- Put detailed procedures in routed files.
- Do not copy private project state into reusable instructions.
