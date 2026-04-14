# Commands & Keyboard Shortcuts — Full Reference

## Slash commands

### Context management

| Command | Action | When to use |
|---------|--------|-------------|
| `/clear` | Reset conversation context | Every 30-45 min of active work |
| `/compact` | Summarize history, free tokens | At 50% context usage |
| `/context` | Detailed token breakdown | Diagnose expensive messages |
| `/cost` | Session token usage + cost | Check periodically |

### Modes

| Command | Action | When to use |
|---------|--------|-------------|
| `/plan` | Enter Plan Mode (read-only) | Before features > 30 min |
| `/execute` | Exit Plan Mode, apply changes | After plan is confirmed |
| `/model` | Switch model (sonnet/opus) | When you need more/less intelligence |
| `/fast` | Toggle fast mode (2.5x speed, 6x cost) | Quick throwaway tasks |

### Tools

| Command | Action | When to use |
|---------|--------|-------------|
| `/simplify` | Detect over-engineering + auto-fix | After implementing a feature |
| `/batch` | Parallel worktree agents | Large refactors across many files |
| `/insights` | Usage analytics + optimization | Weekly review |
| `/debug` | Systematic troubleshooting mode | When stuck on a bug |

### Session

| Command | Action |
|---------|--------|
| `/help` | Contextual help |
| `/status` | Session state + context usage |
| `/tasks` | Monitor background tasks |
| `/rename [name]` | Name the current session |
| `/copy` | Copy a code block or full response |
| `/exit` or `Ctrl+D` | Quit |

### Advanced

| Command | Action |
|---------|--------|
| `/btw [question]` | Side question — no history pollution, no tools |
| `/loop [interval] [prompt]` | Run prompt on repeat (e.g., `/loop 5m check deploy`) |
| `/voice` | Toggle voice input (hold Space to speak) |
| `/mcp` | Show connected MCP servers and tools |
| `/stats` | Usage graph, favorite model, streak |

## Keyboard shortcuts

| Shortcut | Action |
|----------|--------|
| `Shift+Tab` | Cycle permission modes (auto → ask → plan) |
| `Esc` × 2 | Rewind/undo last action (rollback checkpoint) |
| `Ctrl+C` | Interrupt current operation |
| `Ctrl+R` | Search command history |
| `Ctrl+L` | Clear screen (keeps context) |
| `Tab` | Autocomplete |
| `Shift+Enter` | New line in prompt |
| `Ctrl+B` | Background tasks |
| `Ctrl+F` × 2 | Kill all background agents |
| `Alt+T` | Toggle thinking visibility |
| `Space` (hold) | Voice input (requires `/voice` enabled) |
| `Ctrl+D` | Exit |

## File references in prompts

```
@path/to/file.ts    → Reference a file
@agent-name          → Call a subagent
!shell-command       → Run shell command inline
```

## IDE shortcuts

| IDE | Open Claude Code |
|-----|-----------------|
| VS Code / Cursor | `Alt+K` |
| JetBrains | `Cmd+Option+K` |
