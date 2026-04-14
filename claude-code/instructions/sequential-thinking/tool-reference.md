# Sequential Thinking — Tool Reference

## The `sequential_thinking` tool

Single tool exposed by the MCP server. Claude calls it iteratively to build a chain of reasoning.

### Required parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `thought` | string | The current reasoning step content |
| `thoughtNumber` | integer | Current step number (starts at 1) |
| `totalThoughts` | integer | Estimated total steps needed (adjustable) |
| `nextThoughtNeeded` | boolean | `true` if more thinking needed, `false` to finish |

### Optional parameters (revision & branching)

| Parameter | Type | Description |
|-----------|------|-------------|
| `isRevision` | boolean | This step revises a previous step |
| `revisesThought` | integer | Which thought number is being reconsidered |
| `branchFromThought` | integer | Thought number to branch from |
| `branchId` | string | Unique ID for the branch (e.g., "approach-A") |
| `needsMoreThoughts` | boolean | Request to extend the sequence beyond original estimate |

## How it works — 3 layers

### 1. Structured logging

Every step is recorded as a numbered thought:

```json
{
  "thought": "The CUR data has daily granularity. We need to aggregate by service and compare against the 7-day moving average to detect anomalies.",
  "thoughtNumber": 1,
  "totalThoughts": 5,
  "nextThoughtNeeded": true
}
```

### 2. Revision (meta-recognition)

Claude can catch its own mistakes and correct earlier steps:

```json
{
  "thought": "Revision: I assumed unblended_cost is always positive, but credits make it negative. Need to handle that in the aggregation.",
  "thoughtNumber": 4,
  "totalThoughts": 6,
  "nextThoughtNeeded": true,
  "isRevision": true,
  "revisesThought": 2
}
```

### 3. Branching (parallel exploration)

Explore multiple approaches before committing:

```json
{
  "thought": "Branch A: Use statistical z-score for anomaly detection. Pros: simple, well-understood. Cons: assumes normal distribution.",
  "thoughtNumber": 3,
  "totalThoughts": 5,
  "nextThoughtNeeded": true,
  "branchFromThought": 2,
  "branchId": "statistical-approach"
}
```

```json
{
  "thought": "Branch B: Use rule-based thresholds from YAML config. Pros: predictable, configurable. Cons: doesn't adapt to patterns.",
  "thoughtNumber": 3,
  "totalThoughts": 5,
  "nextThoughtNeeded": true,
  "branchFromThought": 2,
  "branchId": "rule-based-approach"
}
```

## Configuration options

### Disable thought logging

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"],
      "env": {
        "DISABLE_THOUGHT_LOGGING": "true"
      }
    }
  }
}
```

### Docker alternative

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "docker",
      "args": ["run", "--rm", "-i", "mcp/sequentialthinking"]
    }
  }
}
```

### MCP scope priority

| Scope | Location | Precedence |
|-------|----------|-----------|
| Managed | `/etc/claude-code/managed-mcp.json` | Highest |
| Command line | `claude mcp add` | High |
| Local | `.claude/settings.json` (project) | Medium |
| Project | `.mcp.json` (repo root) | Low |
| User | `~/.claude/settings.json` (global) | Lowest |
