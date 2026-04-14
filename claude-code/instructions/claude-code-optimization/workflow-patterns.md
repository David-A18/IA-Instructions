# Workflow Patterns — How to Use Claude Code Effectively

## The golden workflow: Explore → Plan → Implement → Commit

### 1. Explore

Let Claude read the codebase and understand the problem.

```
Read finops/analysis/ and explain what's implemented vs what's still TODO.
```

### 2. Plan (`/plan` or Shift+Tab × 2)

Validate the approach before any code generation. Claude plans without writing.

```
/plan
Design the anomaly detection engine. Use rules from config/rules/anomaly_rules.yaml.
Show me the classes, methods, and data flow.
```

Review the plan. Adjust if needed. This is cheap (only input tokens for your corrections).

### 3. Implement (Shift+Tab back to Agent mode)

Execute the confirmed plan.

```
/execute
Implement the plan. Start with anomaly_detector.py, then top_movers.py.
Write tests for each.
```

### 4. Commit

```
Commit all changes: "feat: anomaly detection engine + top movers analysis (Week 3)"
Push to origin main.
```

## Debugging: paste + "fix"

Don't micromanage. Claude's debugging is stronger than most expect.

```
# Paste the error
TypeError: unsupported operand type(s) for +: 'NoneType' and 'float'
at finops/analysis/anomaly_detector.py:42

fix
```

**80% success rate** with just "fix". The more context you add about *how* to fix it, the more likely you lead Claude astray.

### If fix fails twice

1. `Esc Esc` — rollback to checkpoint
2. `/clear` — reset context  
3. Try a different approach with fresh context

Don't correct within broken context. The wrong reasoning is still in the window.

## Let Claude interview you first

For new features, let Claude ask clarifying questions:

```
I want to add Slack alerting to the FinOps CLI. Interview me about requirements.
```

Claude will ask about edge cases you didn't consider. Then:

1. Answer all questions
2. `/clear` (start fresh — interview context is noise for implementation)
3. Paste the spec from the interview as a concise bullet list

## Demand a rewrite for mediocre solutions

When code works but isn't clean:

```
Knowing everything you know now, scrap this and implement the elegant solution.
```

Claude will redesign based on full understanding. The rewrite is consistently better than patches.

## Use subagents for parallel work

```
Use subagents to:
1. Refactor cur_enricher.py to support custom category maps
2. Add tests for the new category mapping
3. Update the CLI help text
```

Claude spawns focused agents with minimal context. Main session stays clean.

## Small tasks: just do it

Don't over-engineer workflows for simple tasks:

- Renaming a variable → just say it
- Adding a docstring → just say it
- Fixing a typo → just say it

Complex workflows (Plan Mode, subagents, skills) are for tasks > 30 minutes.

## Session hygiene checklist

| When | Do |
|------|-----|
| Every 30-45 min | `/clear` or `/compact` |
| At 50% context | `/compact` (mandatory) |
| Before big feature | `/plan` |
| After implementing | `/simplify` to check for over-engineering |
| End of day | `/cost` to review spending |
| Weekly | `/insights` for optimization report |
| Failed fix × 2 | `Esc Esc` → `/clear` → fresh approach |
