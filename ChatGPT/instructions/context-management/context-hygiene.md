# Codex Context Hygiene

Use this route when the task risks loading too many files or mixing project state with reusable instructions.

## Rules

- Read the smallest set of files that can answer the question.
- Prefer `rg` and file-specific reads over opening broad folders.
- Keep `AGENTS.md` compact; route details to task files.
- Do not paste large files into prompts when a path reference is enough.
- Treat generated reports, transcripts, logs, and local state as private unless explicitly approved.

## Router Pattern

1. Native file: `AGENTS.md`.
2. First route: `ChatGPT/instructions-router.md`.
3. Task file: one file from `instructions/**`.
4. Optional source docs: only when the task file links to them.

## Warning Signs

- The agent reads every Markdown file before acting.
- Repeated prompts include large copied context.
- Private planning notes are linked from public instructions.
- Validation commands are missing or vague.
