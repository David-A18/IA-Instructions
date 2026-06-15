# Claude Code Workflow

Use this route when deciding how Claude Code should approach a coding task.

## Default Flow

1. Explore relevant files.
2. State the concrete plan.
3. Implement the smallest safe change.
4. Validate with the project commands.
5. Summarize changed behavior and test results.

## Detailed References

| Need | Read |
| --- | --- |
| Explore, plan, implement, commit workflow | [../claude-code-optimization/workflow-patterns.md](../claude-code-optimization/workflow-patterns.md) |
| Writing a compact `CLAUDE.md` | [../claude-code-optimization/claudemd-guide.md](../claude-code-optimization/claudemd-guide.md) |
| `.claudeignore` starter | [../claude-code-optimization/claudeignore-template](../claude-code-optimization/claudeignore-template) |

## Rules

- Read code before proposing structural changes.
- Keep implementation scoped to the task.
- Preserve user changes unless explicitly told to revert them.
- Run validation before claiming completion when validation is available.
