# Gemini Code Review Prompt

Use this prompt when asking Gemini to review code or a diff.

```text
Review the selected code or diff against the repository rules.

Focus on:

- correctness
- security
- maintainability
- performance
- edge cases
- test coverage
- consistency with existing patterns

Output:

1. Findings first, ordered by severity.
2. For each finding, include the file or selected code reference, the risk, and the concrete fix.
3. Then list missing validation or residual risk.
4. Keep summary brief.

Do not nitpick formatting unless it affects clarity or project conventions.
```
