# CLAUDE.md Guide — Write It Once, Write It Right

## Why it matters

CLAUDE.md loads on **every single request**. It's the highest-cost persistent content in your project. A bloated CLAUDE.md silently wastes tokens and causes Claude to ignore rules.

## Hard limits

| Metric | Target | Hard limit |
|--------|--------|-----------|
| Lines | < 60 | 300 max |
| Tokens | < 2,000 | After ~2,000 Claude starts dropping rules |
| Dynamic content | None | Timestamps/session notes break prompt caching |

## What to include

1. **Response style rules** (3-5 lines) — conciseness, format preferences
2. **Active project pointer** — which spec to read, where code lives
3. **Git config** — repo URL, branch, commit conventions, auto push/pull permissions
4. **File structure** — compact overview (not full tree, just key folders)
5. **Implementation rules** — stack, testing, linting, security (one line each)
6. **Ignore reference** — pointer to `.claudeignore`

## What NOT to include

- API documentation (put in skills or reference files)
- Full file trees (use compact 4-5 line overview)
- Session notes or timestamps (breaks caching)
- Things Claude can infer from reading code
- Long explanations (use bullet points)

## Structure template

```markdown
# Instrucciones para Claude Code

## Estilo de respuesta
- Concise. Bullet points. No paragraphs.
- Code and essential comments only.
- Don't repeat what I said. Don't ask obvious confirmations.

## Active project
- **Spec**: `docs/PROJECT-X.md`
- **Code**: `project-folder/`
- **Stack**: Python 3.12+, boto3, Typer, DuckDB

## Git (auto pull/push allowed)
- **Repo**: `https://github.com/user/repo.git`
- **Branch**: `main`
- Conventional commits: feat:, fix:, chore:, docs:, test:

## File structure
- `docs/` — specs and architecture
- `project-folder/` — all code here (git repo)
- `instructions/` — optimization guides

## Implementation rules
1. Type hints everywhere. Docstrings one line max.
2. Tests: pytest + moto. Test each module.
3. Linting: ruff check + ruff format before commit.
4. Files < 200 lines. Split if larger.
5. No secrets hardcoded. OIDC, least privilege.
6. See `.claudeignore` for ignored files.
```

## Hierarchical CLAUDE.md (multi-level)

CLAUDE.md files stack by directory:

```
~/.claude/CLAUDE.md           → global (all projects)
project-root/CLAUDE.md        → project level
project-root/src/CLAUDE.md    → subdirectory override
```

Lower-level files add to (don't replace) higher-level ones.

## Split large configs with .claude/rules/

If you have too many rules for CLAUDE.md, use rule files:

```
.claude/
├── rules/
│   ├── python-style.md      ← loads when Python files are in context
│   ├── terraform-rules.md   ← loads when .tf files are in context
│   └── testing-rules.md     ← loads when test files are in context
```

Rules load on demand, not on every request. Much cheaper than CLAUDE.md.

## Critical rules: use XML tags

Wrap must-never-violate rules in tags so Claude doesn't skip them:

```markdown
<critical>
Never commit .env files or credentials.
Never use `console.log` — use `finops/utils/logger.py` instead.
</critical>
```

## Prohibitions need alternatives

Bad: "Never use print statements"
Good: "Never use print → use `rich.console.Console().print()` instead"

Without alternatives, Claude pauses and asks permission repeatedly.
