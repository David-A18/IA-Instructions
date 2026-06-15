# Claude Code Instructions

Claude Code should start at [instructions-router.md](instructions-router.md), then read only the linked task file that matches the current work.

## What Is Included

| Path | Purpose |
| --- | --- |
| [instructions-router.md](instructions-router.md) | Canonical first-read router for Claude Code. |
| [instructions/coding/](instructions/coding/) | Coding and Claude API task guidance. |
| [instructions/context/](instructions/context/) | Context retrieval and context hygiene guidance. |
| [instructions/mcp/](instructions/mcp/) | MCP server policy and setup guidance. |
| [instructions/optimization/](instructions/optimization/) | Token, session, workflow, and Claude Code optimization guidance. |
| [instructions/prompts/](instructions/prompts/) | Prompt patterns for planning, debugging, and review. |
| [instructions/templates/](instructions/templates/) | Copyable `CLAUDE.md` and settings templates. |

The existing detailed reference folders under `instructions/` remain available as source material. The public entrypoint is the top-level router.

## Install

Copy the minimal native instruction file into a target repo:

```bash
cp claude-code/instructions/templates/CLAUDE.md.minimal.md /your/project/CLAUDE.md
```

Copy this folder into the target repo or keep it as a shared checkout:

```bash
cp -r claude-code /your/project/instructions/ia-instructions/claude-code
```

Update the `CLAUDE.md` router path if needed.

## Consistency Checklist

- [instructions-router.md](instructions-router.md) exists and is the only canonical entrypoint.
- Every task file used by the router exists.
- Detailed files are reached through the router, not loaded globally.
- Templates contain placeholders only.
- No private planning, local state, or project-specific handoff notes are required.
