# Gemini Instructions

Gemini CLI and Gemini Code Assist should start at [instructions-router.md](instructions-router.md), then read only the linked task file that matches the request.

## What Is Included

| Path | Purpose |
| --- | --- |
| [instructions-router.md](instructions-router.md) | Canonical first-read router for Gemini. |
| [instructions/gemini-cli/](instructions/gemini-cli/) | `GEMINI.md`, CLI memory, and command guidance. |
| [instructions/code-assist/](instructions/code-assist/) | Gemini Code Assist repository context and custom command guidance. |
| [instructions/gemini-api/](instructions/gemini-api/) | Gemini API patterns for structured outputs and tool use. |
| [instructions/context-management/](instructions/context-management/) | Context and repository indexing hygiene. |
| [instructions/prompts/](instructions/prompts/) | Reusable Gemini task prompts. |
| [instructions/templates/](instructions/templates/) | Copyable native instruction files. |

## Install For Gemini CLI

Copy the native instruction template into a target repo:

```bash
cp Gemini/instructions/templates/GEMINI.md /your/project/GEMINI.md
```

Copy this folder into the target repo or keep it as a shared checkout:

```bash
cp -r Gemini /your/project/instructions/ia-instructions/Gemini
```

Update the `GEMINI.md` router path if needed.

## Consistency Checklist

- [instructions-router.md](instructions-router.md) exists and is the only canonical entrypoint.
- Every task file used by the router exists.
- Guidance focuses on `GEMINI.md`, CLI memory, repository context, and Code Assist customization.
- Templates contain placeholders only.
- No private planning, local state, or project-specific handoff notes are required.
