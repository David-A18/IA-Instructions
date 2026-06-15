# Token And Session Hygiene

Use this route when Claude Code feels expensive, slow, repetitive, or confused by stale context.

## First Checks

1. Inspect context size before adding more files.
2. Route to one instruction file, not the whole folder.
3. Clear or compact when the task boundary changes.
4. Switch models only after context is sane.

## Detailed References

| Need | Read |
| --- | --- |
| Token-saving techniques | [../claude-code-optimization/token-saving.md](../claude-code-optimization/token-saving.md) |
| Usage bar vs session token confusion | [../claude-code-optimization/usage-metering.md](../claude-code-optimization/usage-metering.md) |
| Command and shortcut reference | [../claude-code-optimization/commands-shortcuts.md](../claude-code-optimization/commands-shortcuts.md) |
| Token audit commands | [../claudectx/commands.md](../claudectx/commands.md) |
| Daily optimization routine | [../claudectx/workflow.md](../claudectx/workflow.md) |

## Rules

- Do not solve bloat by adding more always-loaded instructions.
- Prefer specific file references over pasted file content.
- Stop and reassess after two failed correction loops.
- Keep long-running work split by task boundary.
