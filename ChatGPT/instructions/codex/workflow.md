# Codex Workflow

Use this route when Codex is asked to inspect, implement, review, or validate code in a repository.

## Default Flow

1. Read `AGENTS.md`.
2. Inspect the relevant files before planning.
3. Identify the smallest safe change.
4. Edit only files needed for the task.
5. Run targeted validation.
6. Summarize changed behavior, files touched, and validation result.

## Ask Mode

Use ask mode for architecture review, refactor suggestions, debugging analysis, and planning. Do not modify files in ask mode.

## Code Mode

Use code mode when the user explicitly wants implementation. Keep changes scoped and validate before finalizing.

## Review Mode

Lead with findings ordered by severity. Include file and line references. Mention missing tests and residual risks.

## Safety Rules

- Do not rewrite history or force-push unless explicitly requested.
- Do not expose secrets, local state, transcripts, or private project paths.
- Do not install packages or update lockfiles unless the task requires it.
- Prefer local repo truth over model memory.
