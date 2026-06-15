# GEMINI.md And Gemini CLI Memory

Use this route when setting up Gemini CLI project instructions or memory behavior.

## Recommended Pattern

Use a small `GEMINI.md` at the repository root:

- State the project purpose.
- Point Gemini to `Gemini/instructions-router.md`.
- Tell Gemini to choose one routed task file.
- Keep private planning and local state out of project memory.

## Memory Rules

- Use durable memory only for stable project rules.
- Do not store secrets, credentials, transcripts, or generated reports.
- Keep task-specific procedures in routed instruction files.
- Review memory periodically and remove stale rules.

## When To Use Router Files

Use routed files for:

- Code Assist behavior.
- API usage patterns.
- Context hygiene.
- Prompt templates.
- Model/tool-specific workflows.

## Avoid

- Large `GEMINI.md` files that duplicate the full README.
- Personal preferences mixed with team-shared rules.
- Broad instructions that tell Gemini to inspect the whole repo before every answer.
