# claudectx Daily Workflow

## First time setup (once)

```bash
# 1. Install
npm install -g claudectx

# 2. Analyze your project
claudectx analyze

# 3. Fix everything
claudectx optimize --apply

# 4. Install MCP server
claudectx mcp --install

# 5. Install auto-compress hook
claudectx hooks add auto-compress
```

## Start of day

```bash
# Pre-warm prompt cache (saves ~$0.02/session on first request)
claudectx warmup --api-key $ANTHROPIC_API_KEY
```

Or set up a cron:
```bash
claudectx warmup --cron "0 9 * * 1-5"
```

## During work

```bash
# Open live dashboard in a separate terminal
claudectx watch
```

The dashboard shows token burn in real-time. If you see a file being read repeatedly, consider adding it to `.claudeignore` or using `smart_read`.

## End of session

```bash
# Compress session into MEMORY.md (auto if hook installed)
claudectx compress

# Check what you spent
claudectx report --days 1
```

## Weekly review

```bash
# Full analytics
claudectx report --days 7

# Check for stale CLAUDE.md content
claudectx drift

# Re-analyze (your codebase changes over time)
claudectx analyze
```

## Before a big task

```bash
# Estimate cost before running
claudectx budget "finops/**/*.py" "tests/**/*.py"
```

## Team workflow (weekly)

```bash
# Each dev exports
claudectx teams export

# Lead aggregates
claudectx teams aggregate --dir ./reports/
```

## If something goes wrong

```bash
# See all backups
claudectx revert --list

# Restore any file
claudectx revert --id <id>
```
