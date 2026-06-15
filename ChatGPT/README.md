# ChatGPT / Codex Instructions

ChatGPT and OpenAI Codex should start at [instructions-router.md](instructions-router.md), then read only the task-specific file that matches the request.

## What Is Included

| Path | Purpose |
| --- | --- |
| [instructions-router.md](instructions-router.md) | Canonical first-read router for ChatGPT and Codex. |
| [instructions/codex/](instructions/codex/) | Codex workflow and operating rules. |
| [instructions/agents-md/](instructions/agents-md/) | `AGENTS.md` guidance and templates. |
| [instructions/openai-api/](instructions/openai-api/) | OpenAI API patterns for tools and structured outputs. |
| [instructions/context-management/](instructions/context-management/) | Context, file-reading, and session hygiene. |
| [instructions/prompts/](instructions/prompts/) | Reusable prompts for research and planning. |
| [instructions/templates/](instructions/templates/) | Copyable native instruction files. |

## Install For Codex

Copy the native instruction template into a target repo:

```bash
cp ChatGPT/instructions/templates/AGENTS.md /your/project/AGENTS.md
```

Copy this folder into the target repo or keep it as a shared checkout:

```bash
cp -r ChatGPT /your/project/instructions/ia-instructions/ChatGPT
```

Update the `AGENTS.md` router path if needed.

## Consistency Checklist

- [instructions-router.md](instructions-router.md) exists and is the only canonical entrypoint.
- Every task file used by the router exists.
- The deleted legacy research prompt has been moved under `instructions/prompts/`.
- Templates contain placeholders only.
- No private planning, local state, or project-specific handoff notes are required.
