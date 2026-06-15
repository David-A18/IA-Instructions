# IA-Instructions

Reusable Markdown instruction routers for AI coding tools.

The goal is simple: give each AI model a small first-read router, then send it to one task-specific instruction file instead of loading a whole knowledge base on every prompt.

## Supported Tools

| Folder | Tool family | First-read file | Status |
| --- | --- | --- | --- |
| [claude-code/](claude-code/) | Claude Code | [claude-code/instructions-router.md](claude-code/instructions-router.md) | Active |
| [ChatGPT/](ChatGPT/) | ChatGPT and OpenAI Codex | [ChatGPT/instructions-router.md](ChatGPT/instructions-router.md) | Active starter |
| [Gemini/](Gemini/) | Gemini CLI and Gemini Code Assist | [Gemini/instructions-router.md](Gemini/instructions-router.md) | Active starter |

## Repository Contract

Each tool folder follows the same structure:

```text
tool-folder/
  README.md                 # Human usage guide
  instructions-router.md    # First file the model should read
  instructions/             # Task-scoped instruction files
  instructions/templates/   # Copyable native instruction files
```

Keep the always-loaded native file small:

- Claude Code: copy from `claude-code/instructions/templates/CLAUDE.md.minimal.md`.
- Codex: copy from `ChatGPT/instructions/templates/AGENTS.md`.
- Gemini: copy from `Gemini/instructions/templates/GEMINI.md`.

The native file should point to the router and tell the agent to load only the one file that matches the task.

## Router Pattern

1. The model reads its native instruction file.
2. The native file points to `instructions-router.md`.
3. The router maps the current situation to one instruction file.
4. The model reads only that task file.
5. Detailed reference files stay out of always-loaded context.

## Publication Boundary

This public repo should contain only reusable instruction mechanics:

- Root and model `README.md` files.
- Model routers.
- Task-scoped instruction files.
- Safe templates and examples with placeholders only.

Do not publish local planning notes, state files, transcripts, generated reports, secrets, private project paths, or project-specific handoff memory. Local planning material belongs under ignored `private/`.

## Quick Start

### Claude Code

```bash
cp claude-code/instructions/templates/CLAUDE.md.minimal.md /your/project/CLAUDE.md
cp -r claude-code /your/project/instructions/ia-instructions/claude-code
```

Then adjust the router path inside `CLAUDE.md` if you place the folder somewhere else.

### OpenAI Codex

```bash
cp ChatGPT/instructions/templates/AGENTS.md /your/project/AGENTS.md
cp -r ChatGPT /your/project/instructions/ia-instructions/ChatGPT
```

### Gemini

```bash
cp Gemini/instructions/templates/GEMINI.md /your/project/GEMINI.md
cp -r Gemini /your/project/instructions/ia-instructions/Gemini
```

## Maintenance Rules

- Add one router row for every new task file.
- Keep each instruction file focused on one situation.
- Prefer specific commands, paths, and decision rules over generic advice.
- Keep private planning and project state out of tracked public files.
- Validate links and publication boundary before pushing.

## License

GPL-3.0. See [LICENSE](LICENSE).
