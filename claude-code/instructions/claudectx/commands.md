# claudectx — All 13 Commands

## 1. `analyze` — See where tokens go

```bash
claudectx analyze                        # Current directory
claudectx analyze --path /path/to/proj   # Specific path
claudectx analyze --json                 # Raw JSON output
claudectx analyze --model sonnet         # Calculate for specific model
claudectx analyze --watch                # Re-run on file changes
```

Shows: tokens per component, estimated cost per request, optimization opportunities.

## 2. `optimize` — Auto-fix token waste

```bash
claudectx optimize                    # Interactive (confirm each change)
claudectx optimize --apply            # Apply all fixes without prompting
claudectx optimize --dry-run          # Preview only
claudectx optimize --claudemd         # Only split CLAUDE.md
claudectx optimize --ignorefile       # Only generate .claudeignore
claudectx optimize --cache            # Only fix cache-busting content
claudectx optimize --hooks            # Only install tracking hooks
```

What it does:
- **CLAUDE.md splitter**: keeps core < 2K tokens, moves reference docs to `.claude/` with `@file` refs
- **.claudeignore generator**: detects project type (Node/Python/Go/Rust), generates defaults
- **Cache advisor**: comments out timestamps and patterns that break prompt caching
- **Hooks installer**: adds PostToolUse hook for `claudectx watch` tracking

## 3. `watch` — Live token dashboard

```bash
claudectx watch                    # Launch dashboard
claudectx watch --session <id>     # Watch specific session
claudectx watch --clear            # Clear read log
```

Real-time terminal UI: token burn rate, cache hit rate, most-read files. Requires hooks from `optimize --hooks`.

## 4. `mcp` — Smart MCP server (symbol-level reads)

```bash
claudectx mcp                  # Start MCP server (stdio)
claudectx mcp --install        # Auto-add to .claude/settings.json
```

See `mcp-server.md` for full details.

## 5. `compress` — Session memory compression

```bash
claudectx compress                        # Most recent session
claudectx compress --session <id>         # Specific session
claudectx compress --auto                 # Non-interactive (for hooks)
claudectx compress --prune --days 30      # Also prune old entries
claudectx compress --api-key <key>        # Explicit API key
```

8,000-token session → ~180 tokens (97.8% reduction). Uses claude-haiku if API key set, else heuristic.

## 6. `report` — Usage analytics

```bash
claudectx report                # Last 7 days
claudectx report --days 30      # Last 30 days
claudectx report --json         # JSON output
claudectx report --markdown     # Markdown output
claudectx report --model opus   # Cost estimate for different model
```

Shows: sessions, requests, tokens, cache hits, daily usage bars, cost estimates.

## 7. `budget` — Know cost before you start

```bash
claudectx budget "**/*.py"                        # All Python files
claudectx budget "src/**/*.ts" --threshold 20000  # Warn if > 20K tokens
claudectx budget "src/**" --model opus --json     # JSON for scripting
```

Shows: per-file token counts, cache hit likelihood, total cost, .claudeignore recommendations.

## 8. `warmup` — Pre-warm prompt cache

```bash
claudectx warmup --api-key $ANTHROPIC_API_KEY           # 5-min TTL
claudectx warmup --ttl 60 --api-key $ANTHROPIC_API_KEY  # 60-min TTL (2x cost)
claudectx warmup --cron "0 9 * * 1-5"                   # Weekday 9am cron
claudectx warmup --json                                  # JSON output
```

First request gets a cache hit instead of full miss. Cron reads `ANTHROPIC_API_KEY` from env.

## 9. `drift` — Find stale CLAUDE.md content

```bash
claudectx drift                      # Scan current project
claudectx drift --days 14            # 14-day read window
claudectx drift --fix                # Interactively remove flagged lines
claudectx drift --json               # JSON output
```

Detects: dead `@file` refs, git-deleted mentions, zero-read sections, dead inline paths.

## 10. `convert` — Export CLAUDE.md to other formats

```bash
claudectx convert --to cursor         # → .cursor/rules/<section>.mdc
claudectx convert --to copilot        # → .github/copilot-instructions.md
claudectx convert --to windsurf       # → .windsurfrules
claudectx convert --to cursor --dry-run
```

Each `##` section → separate Cursor `.mdc` file with `alwaysApply: true`.

## 11. `hooks` — Hook marketplace

```bash
claudectx hooks list                                        # All available
claudectx hooks add auto-compress                           # Install
claudectx hooks add auto-compress --config threshold=30000  # Custom config
claudectx hooks add slack-digest --config webhookUrl=https://...
claudectx hooks remove slack-digest
claudectx hooks status                                      # Show installed
```

Built-in hooks:

| Hook | Trigger | What it does |
|------|---------|-------------|
| `auto-compress` | PostToolUse (Read) | Runs compress after session |
| `daily-budget` | PreToolUse | Reports today's spend before tool calls |
| `slack-digest` | Stop | Posts session report to Slack |
| `session-warmup` | PostToolUse (Read) | Re-warms cache |

## 12. `teams` — Multi-developer cost attribution

```bash
# Each developer:
claudectx teams export                           # Export last 30 days
claudectx teams export --days 7 --model haiku

# Team lead aggregates:
claudectx teams aggregate --dir ./reports/
claudectx teams aggregate --dir ./reports/ --anonymize
claudectx teams aggregate --dir ./reports/ --json
```

Per-developer spend, cache hit rate, avg request size. No session content shared.

## 13. `revert` — Restore any backup

```bash
claudectx revert --list                # See all backups
claudectx revert --id <id>            # Restore specific backup
claudectx revert                       # Interactive picker
claudectx revert --file CLAUDE.md     # Filter to one file
```

Every modify command backs up automatically. Backups kept for 50 most recent operations.
