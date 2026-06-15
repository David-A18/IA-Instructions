# Agent Instructions

## Instruction Router

- Start with `instructions/ia-instructions/ChatGPT/instructions-router.md`.
- Pick one row that matches the task.
- Read only the linked task file unless the user asks for broader research.
- Do not read the full instruction tree.

## Working Rules

- Read relevant local files before editing.
- Preserve user changes.
- Keep changes scoped to the request.
- Run available validation before finalizing implementation work.

## Validation

Replace these placeholders with real project commands:

```bash
<test command>
<lint command>
<build command>
```
