# Gemini Code Assist Repository Context And Custom Commands

Use this route for Gemini Code Assist in VS Code, JetBrains IDEs, Cloud Shell Editor, Cloud Workstations, or Android Studio.

## Repository Context

- Use repository context when the task depends on conventions from a specific repo or module.
- Point Gemini at the smallest relevant repository, folder, or selected file.
- Ask for sources or file references when an answer depends on indexed context.
- Do not rely on repository context for secrets or local-only files that should not be indexed.

## Custom Commands

Create custom commands for repeated IDE actions such as:

- add tests for selected code
- explain selected module
- refactor selected function while preserving public API
- review selected diff against project rules

Keep commands short and action-specific. Put durable policy in `GEMINI.md` and detailed workflows in routed files.

## Code Customization

For enterprise code customization:

- Index only repositories that are safe and relevant.
- Keep examples representative and maintained.
- Avoid indexing generated outputs, secrets, local logs, and private notes.
- Re-check repository selection when suggestions look stale or unrelated.
