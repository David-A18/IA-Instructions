# Gemini Instructions

## Instruction Router

- Start with `instructions/ia-instructions/Gemini/instructions-router.md`.
- Pick one row that matches the task.
- Read only the linked task file unless the user asks for broader research.
- Do not read the full instruction tree.

## Working Rules

- Read relevant local files before editing.
- Preserve user changes.
- Keep changes scoped to the request.
- Run available validation before finalizing implementation work.

## Context Rules

- Do not store secrets, transcripts, logs, generated reports, or local planning notes in Gemini memory.
- Use repository context or selected files only when they are relevant to the task.
