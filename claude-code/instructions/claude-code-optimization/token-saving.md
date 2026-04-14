# Token Saving — 10 Techniques + Common Mistakes

## The math

| Type | Cost per 1M | Example: 100k tokens |
|------|------------|---------------------|
| Input | $3 | $0.30 |
| Output | $15 | $1.50 |
| Cache read | $0.30 | $0.03 |

Output is **5x more expensive**. A verbose 500-line explanation costs 5x what a concise 100-line code block costs.

## 10 techniques ranked by impact

### 1. `/compact` at 50% context (highest impact)

Claude has an "agent dumb zone" — when context exceeds 60-70%, it starts ignoring instructions and making basic errors.

```
/compact         # summarize history, free tokens
/context         # check current usage percentage
```

Rule: `/compact` at 50%. Don't wait for auto-compaction.

### 2. `/clear` every 30-45 minutes

Every message accumulates in context. A 4-hour session can hit 150k tokens, tripling per-message cost.

```
/clear           # full reset (new session)
```

### 3. CLAUDE.md under 60 lines

CLAUDE.md loads on **every request**. A 10k token CLAUDE.md × 60 requests/day = 600k wasted tokens/day. See `claudemd-guide.md` for rules.

### 4. Add "be concise" to CLAUDE.md

Without this, Claude generates long explanations you didn't ask for. Output tokens = expensive.

```markdown
## Response style
- Be concise. Only code and essential comments.
- Bullet points, not paragraphs.
```

### 5. Use Plan Mode before big features

```
/plan            # enter Plan Mode (read-only)
/execute         # exit Plan Mode (apply changes)
```

Without Plan Mode: Claude generates 500 lines in wrong direction → wasted tokens.
With Plan Mode: validate approach first → correct code first time.

### 6. Concise prompts (40% fewer tokens)

Bad (50 tokens):
```
Could you please create a React component that displays a list of users? 
I need it to be responsive and have pagination. Oh, and also a search bar.
```

Good (30 tokens):
```
Create UserList component: user list, responsive, pagination, search bar. 
Match existing project style.
```

### 7. Use subagents for isolated tasks

Subagents get separate, focused context. Main context stays clean.

```
"Use subagents to refactor the ingestion module and update tests in parallel"
```

Main context: 100k tokens. Subagent: only relevant files (20k tokens).

### 8. .claudeignore to block unnecessary files

Claude reads files into context. Exclude everything it doesn't need:

```
node_modules/
dist/
.terraform/
*.tfstate
__pycache__/
*.lock
```

See `claudeignore-template` in this folder.

### 9. Reference files, don't embed content

Bad: pasting a 200-line file into CLAUDE.md
Good: `"For API patterns, see finops/utils/aws_client.py"`

### 10. Stop after 2 failed correction attempts

If Claude fails twice on the same fix:
1. Press `Esc Esc` to rollback
2. `/clear` to reset context
3. Try a different approach

Correcting within broken context makes it worse — the wrong reasoning is still in the window.

## Common mistakes that waste tokens

| Mistake | Cost impact | Fix |
|---------|-----------|-----|
| Not using `/clear` | Context grows → 3x cost per msg | Clear every 30-45 min |
| Letting Claude be verbose | 2x output tokens | "Be concise" in CLAUDE.md |
| Large files in context | Inflated input per message | .claudeignore + split files |
| Repeating instructions | 50+ tokens per message | CLAUDE.md once |
| No Plan Mode | Misfocused code → redo | `/plan` for features > 30m |
| Micromanaging bug fixes | Leads Claude astray | Paste error + "fix" |
| Not compacting at 50% | "Dumb zone" after 60% | `/compact` at 50% |
| Over-engineering workflows | Overkill for small tasks | Native Claude for < 5 min tasks |

## Monitor your spending

```
/cost            # token usage + estimated cost for session
/context         # detailed token breakdown by source
/insights        # usage analytics + optimization report
```
