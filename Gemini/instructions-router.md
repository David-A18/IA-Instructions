# Gemini Instruction Router

Read this file first. Do not read the full instruction tree.

Choose the one row that matches the task, then open only that file.

## Gemini CLI

| Situation | Read |
| --- | --- |
| Setting up `GEMINI.md` or project memory | [instructions/gemini-cli/gemini-md-memory.md](instructions/gemini-cli/gemini-md-memory.md) |
| Need copyable `GEMINI.md` | [instructions/templates/GEMINI.md](instructions/templates/GEMINI.md) |

## Gemini Code Assist

| Situation | Read |
| --- | --- |
| Need IDE repository context, custom commands, or code customization guidance | [instructions/code-assist/repository-context-and-custom-commands.md](instructions/code-assist/repository-context-and-custom-commands.md) |

## Gemini API

| Situation | Read |
| --- | --- |
| Need structured output, tools, or API prompt guidance | [instructions/gemini-api/structured-output-and-tools.md](instructions/gemini-api/structured-output-and-tools.md) |
| Need context, repository indexing, or file-selection hygiene | [instructions/context-management/context-hygiene.md](instructions/context-management/context-hygiene.md) |

## Prompts

| Situation | Read |
| --- | --- |
| Need a reusable review prompt for Gemini | [instructions/prompts/code-review-prompt.md](instructions/prompts/code-review-prompt.md) |

## Rules

- Prefer `GEMINI.md` for Gemini CLI project memory.
- Keep `GEMINI.md` compact and point it at this router.
- Use Code Assist repository context for focused repo-aware IDE tasks.
- Do not copy private project state into reusable instructions.
