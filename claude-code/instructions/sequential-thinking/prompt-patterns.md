# Sequential Thinking — Prompt Patterns & Token Economics

## Trigger phrases

Any of these tell Claude to use the sequential thinking tool:

```
Use sequential thinking to...
Think through this step by step using sequential thinking...
Break this down using sequential thinking...
Plan this with sequential thinking before implementing...
```

## Architecture & design prompts

```
Use sequential thinking to design the anomaly detection engine for FinOps Autopilot.
Consider: data flow from DuckDB, rule evaluation, alert generation, and PR creation.
```

```
Use sequential thinking to plan the Terraform module structure for multi-account
AWS CUR ingestion. Consider: S3 buckets, Athena workgroups, IAM roles, and cost.
```

```
Use sequential thinking to design how the CLI should orchestrate:
ingest → analyze → recommend → report → alert.
Consider error handling, partial failures, and resumability.
```

## Debugging prompts

```
Use sequential thinking to debug this intermittent issue:
DuckDB queries return empty results when usage_date contains timezone offsets.
Walk through the data flow from CUR parse → enrich → store → query.
```

```
Use sequential thinking to analyze why cache hit rate is low.
Consider: CLAUDE.md size, dynamic content, conversation length, MCP tool schemas.
```

## Planning prompts

```
Use sequential thinking to plan Week 3 of FinOps Autopilot:
anomaly detection + top movers. I need: data model, algorithm choice,
config schema, CLI commands, and test strategy.
```

```
Use sequential thinking to evaluate two approaches for rightsizing recommendations:
A) CloudWatch metrics only, B) CloudWatch + Kubernetes metrics.
Compare: complexity, accuracy, AWS cost, implementation time.
```

## Refactoring prompts

```
Use sequential thinking to plan refactoring cur_enricher.py to support
custom category maps loaded from YAML instead of hardcoded dict.
Consider backwards compatibility and test impact.
```

## Best practices

### Set `totalThoughts` limits

For architecture: 5-8 steps. For debugging: 3-5 steps. For simple decisions: 2-3 steps.

```
Use sequential thinking (limit to 5 steps) to design the alert sender module.
```

### Combine with Plan Mode

```
/plan
Use sequential thinking to design the savings_tracker module.
Then outline the implementation plan with file changes.
```

Review the plan, then `/execute` to implement.

### Combine with Context7

```
Use sequential thinking to design the K8s rightsizer.
For the kubernetes Python client API, use context7.
```

Sequential thinking plans the architecture, Context7 provides correct API calls.

## Token economics

Sequential thinking uses **more tokens per task** but **fewer total tokens** by avoiding rework.

| Task complexity | Without seq. thinking | With seq. thinking | Net savings |
|----------------|----------------------|-------------------|-------------|
| Simple (< 30 min) | 1 iteration, 2K tokens | Overkill | Don't use it |
| Medium (1-2 hours) | 2-3 iterations, 15K tokens | 1 iteration, 8K tokens | ~47% |
| Complex (4+ hours) | 3-5 iterations, 40K tokens | 1-2 iterations, 15K tokens | ~63% |

### Token overhead per step

| Activity | Extra tokens | Mitigation |
|----------|-------------|------------|
| Each thought step | 200-500 tokens | Limit totalThoughts |
| Revision steps | 300-600 tokens | Worth it for correctness |
| Branching (2 paths) | 400-800 tokens | Worth it for complex decisions |
| Full 8-step chain | 2,000-4,000 tokens | Still cheaper than one wrong implementation |

### When NOT to use (wastes tokens)

- Simple bug fixes → just paste error + "fix"
- Known implementations → just describe what you want
- Single-file changes → native Claude Code is faster
- Tasks < 30 minutes → overhead isn't justified

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| "Client closed" | Wrong path in config | Use absolute paths to npx |
| "No tools found" | Protocol version mismatch | Update: `npm install -g @modelcontextprotocol/server-sequential-thinking` |
| Endless thinking | Vague prompt | Add step limit: "limit to 5 steps" |
| Context overload | Too many active MCP servers | Move low-priority servers to project-level config |
| Instructions ignored | Context window > 180k | `/clear` and restart session |
