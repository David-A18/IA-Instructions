# Claude Code Extensibility

Use this route when configuring native Claude Code memory, skills, subagents, hooks, or rules.

## Decision Table

| Need | Use |
| --- | --- |
| Persistent project instructions | Short `CLAUDE.md` pointing to this router. |
| Reusable task procedure | Skill with a focused `SKILL.md`. |
| Isolated specialist context | Subagent with a single responsibility. |
| Enforced safety check | Hook, not prose. |
| Current syntax and examples | [../claude-code-optimization/skills-subagents.md](../claude-code-optimization/skills-subagents.md). |

## Rules

- Keep `CLAUDE.md` small because it is loaded repeatedly.
- Give subagents specific, action-oriented descriptions.
- Limit tool access for specialists.
- Treat hooks as executable security-sensitive code.
- Version project-level agents, skills, and templates when they are shared by a team.
