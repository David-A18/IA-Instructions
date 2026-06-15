# Claude Code Rules

## Instruction Router

- Start with `instructions/ia-instructions/claude-code/instructions-router.md`.
- Pick one row that matches the task.
- Read only the linked task file unless the user asks for broader research.
- Do not read the full instruction tree.

## Working Rules

- Read relevant local files before changing code.
- Preserve user changes.
- Keep edits scoped to the request.
- Run available validation before finalizing implementation work.
