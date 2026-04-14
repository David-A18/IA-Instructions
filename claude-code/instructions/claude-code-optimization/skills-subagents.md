# Skills, Subagents, Hooks & Rules — Configuration Guide

## Overview

| Component | What it does | Loads when |
|-----------|-------------|------------|
| CLAUDE.md | Global project rules | Every request (always-on) |
| Skills | Task-specific expertise | Semantic match to prompt |
| Subagents | Isolated execution context | Explicitly invoked |
| Hooks | Event-driven automation | Lifecycle events (auto) |
| Rules | File-type-specific guidance | Matching files in context |

## Skills

Skills are on-demand knowledge modules. They load only when Claude detects relevance, saving context.

### Create a skill

```
.claude/skills/
└── finops-analysis/
    ├── SKILL.md              ← core rules + index (< 500 lines)
    ├── references/
    │   ├── cur-schema.md     ← CUR column definitions
    │   └── anomaly-rules.md  ← detection rule reference
    ├── examples/
    │   └── sample-report.md  ← example output
    └── scripts/
        └── validate.sh       ← helper scripts
```

### SKILL.md format

```markdown
---
name: finops-analysis
description: "Analyze AWS cost data, detect anomalies, generate rightsizing recommendations"
allowed-tools: ["Read", "Edit", "Bash", "Glob"]
model: sonnet
---

# FinOps Analysis Skill

## When to use
When asked about cost analysis, anomaly detection, or rightsizing.

## Rules
1. Always query DuckDB via `finops/ingestion/local_store.py`
2. Use YAML rules from `finops/config/rules/`
3. Output reports in JSON and Markdown

## Gotchas
- CUR dates are strings "YYYY-MM-DD", not datetime objects
- unblended_cost can be negative (credits)
- Always check `line_item_type` before aggregating
```

The `description` field is critical — it drives semantic matching.

### Progressive disclosure

Keep SKILL.md lean. Put detailed docs in `references/`. Claude only reads subdirectories when it needs them.

### Gotchas section (high-signal)

Document every failure mode. Over time, this becomes the most valuable content:

```markdown
## Gotchas
- Bug: enricher returns "unknown" environment when tag_environment has trailing spaces
- Bug: DuckDB doesn't auto-cast VARCHAR to DATE in WHERE clauses
- Convention: always use `usage_date` not `usage_start` for daily grouping
```

## Subagents

Subagents run in isolated context. They don't pollute the main session.

### Create a subagent

```
.claude/agents/
└── test-runner.md
```

```markdown
---
name: test-runner
description: "Run and fix failing tests"
allowed-tools: ["Read", "Edit", "Bash", "Glob"]
disallowed-tools: ["WebSearch"]
model: sonnet
---

# Test Runner Agent

Run pytest, analyze failures, fix them. Don't modify test expectations 
unless the implementation is correct and the test is wrong.
```

### Invoke a subagent

In your prompt:
```
"Use subagents to refactor ingestion/ and update tests in parallel"
```

Or reference directly:
```
@test-runner run all tests and fix failures
```

### Best practices

- Make subagents **feature-specific** ("test-runner", "terraform-planner"), not generic ("QA agent")
- Subagents cannot invoke other subagents
- Use for: code review, refactoring, test fixing, documentation generation

## Hooks

Hooks are scripts that fire on Claude Code lifecycle events. Defined in `.claude/settings.json`.

### Common hooks

#### Auto-format on file write

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "ruff format \"$CLAUDE_TOOL_INPUT_FILE_PATH\" 2>/dev/null; ruff check --fix \"$CLAUDE_TOOL_INPUT_FILE_PATH\" 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

#### Block writes to protected files

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo \"$CLAUDE_TOOL_INPUT_FILE_PATH\" | grep -qE '(\\.env|credentials|secrets)' && exit 2 || exit 0"
          }
        ]
      }
    ]
  }
}
```

#### Auto git-add after writes

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "git add \"$CLAUDE_TOOL_INPUT_FILE_PATH\" 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

### Hook lifecycle events

| Event | When | Can block? |
|-------|------|-----------|
| `PreToolUse` | Before any tool execution | Yes (exit 2) |
| `PostToolUse` | After tool completion | No |
| `UserPromptSubmit` | When user sends a message | Yes (exit 2) |
| `Stop` | When Claude finishes responding | Yes (exit 2) |
| `SessionStart` | New session starts | No |
| `Notification` | Claude sends a notification | No |
| `SubagentStart` | Subagent spawns | No |
| `SubagentStop` | Subagent finishes | No |

### Exit codes

- `0` — success, continue
- `2` — block the operation (PreToolUse, Stop, UserPromptSubmit only)
- Other — error (logged, operation continues)

## Rules (file-type-specific)

```
.claude/rules/
├── python.md         ← loads when .py files are in context
├── terraform.md      ← loads when .tf files are in context
└── yaml-config.md    ← loads when .yaml files are in context
```

Rules load on demand, unlike CLAUDE.md which loads always. Use rules for language/framework-specific guidance that doesn't need to be in every request.
