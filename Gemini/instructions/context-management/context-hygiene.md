# Gemini Context Hygiene

Use this route when a Gemini task risks loading too much repository or knowledge context.

## Rules

- Select the smallest repository, folder, file, or code range that can answer the task.
- Keep `GEMINI.md` compact and durable.
- Put task procedures in routed files instead of always-loaded memory.
- Exclude generated outputs, logs, transcripts, secrets, and local private notes from indexed context.
- Ask Gemini to cite files or selected context when accuracy matters.

## Warning Signs

- Gemini summarizes unrelated parts of the repo.
- Suggestions use stale conventions.
- The prompt depends on files that are not indexed or selected.
- Local-only private notes appear in reusable instruction examples.

## Corrective Actions

- Re-scope the selected repository or file context.
- Add a narrower custom command.
- Move stable rules into `GEMINI.md`.
- Move detailed workflows into routed instruction files.
