# AGENTS.md Guidance

Use `AGENTS.md` as the repository-level instruction file for Codex and other agents that support the open format.

## What To Include

- Project purpose in one or two sentences.
- Repository boundaries and privacy rules.
- Commands for tests, lint, build, and local run.
- Coding conventions that are specific enough to follow.
- The instruction-router path for deeper guidance.

## What To Exclude

- Long architecture essays.
- Historical implementation logs.
- Private planning files.
- Secrets, tokens, local machine paths, transcripts, or generated reports.
- Generic advice like "follow best practices" unless paired with concrete commands or patterns.

## Minimal Shape

Use this structure:

- `# Agent Instructions`
- `## Project`
- `Purpose: <short project purpose>.`
- `Router: <path-to-instructions-router.md>.`
- `## Working Rules`
- `Read relevant files before editing.`
- `Preserve user changes.`
- `Keep changes scoped to the request.`
- `Run validation before finalizing implementation work.`
- `## Validation`
- `<test command>`
- `<lint command>`
- `<build command>`

## Maintenance

Update `AGENTS.md` only when a durable repository rule changes. Put task-specific details in routed instruction files.
