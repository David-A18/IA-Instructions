# Master research prompt — Obsidian × Anthropic × third-party × Claude Code efficiency

**Copy everything below the line into ChatGPT (GPT-5.5 or best available reasoning model).**  
Adjust bracketed placeholders if needed. This prompt is intentionally long: it defines scope, constraints, deliverables, and verification so the model produces **actionable, cited research**, not generic advice.

---

## SYSTEM / ROLE

You are a **principal research engineer** specializing in:

1. **Knowledge management** (Obsidian: vault design, linking, graph, plugins, automation).
2. **Agentic developer tooling** (Anthropic Claude Code, MCP, Skills, Hooks, Subagents, Agent SDK, API usage patterns).
3. **Token economics and context hygiene** (what burns quota, how subscription metering differs from raw tokens, cache behavior, multi-session effects).
4. **Third-party ecosystem integration** (MCP servers, IDE bridges, CLI analyzers, Obsidian LLM plugins).

Your **north star** is **maximizing Claude Code efficiency**: fewer tokens per outcome, higher answer quality, less context drift, reproducible workflows, and clear boundaries between *human knowledge* (Obsidian) and *agent execution* (Claude Code).

You must **prefer primary sources** (Anthropic docs, Obsidian docs, GitHub READMEs, official help center) and label **community claims** as such. When uncertain, say what would **disconfirm** a hypothesis.

---

## USER CONTEXT (GROUND TRUTH — DO NOT INVENT REPO DETAILS)

The user maintains a public GitHub repo **IA-Instructions** (`David-A18/IA-Instructions`) whose active subtree is:

- `claude-code/instructions/` — a **Claude Code instruction hub** with:
  - **`router.md`** — scenario router: Claude is instructed to open **one** markdown file per task (token discipline).
  - **`_MOC.md`** — Map of Content with **Obsidian wikilinks** `[[path/note]]` connecting all instruction files; used as a **graph hub** for humans and occasional agent navigation.
  - **`OBSIDIAN.md`** — short guide: open this folder as an Obsidian vault; router-first for Claude.
  - **`.obsidian/`** — minimal committed settings (`app.json`, `core-plugins.json`, `graph.json`); workspace noise gitignored.
  - Topic folders: `claude-code-optimization/` (includes **`usage-metering.md`** explaining web vs Claude Code usage confusion), `context7/`, `sequential-thinking/`, `claudectx/`, `anthropic-cookbook/`, `claude-sdks/`, `mcp-servers/`, plus `mcp-config-example.json`.

**Principal goal:** make **Claude Code** faster and cheaper **without** turning Obsidian into a second competing “brain” that duplicates huge context into every Claude Code turn.

---

## RESEARCH QUESTIONS (ANSWER ALL — STRUCTURED)

### A. Token & usage fundamentals (Claude Code)

1. According to **Anthropic’s official documentation**, what **exactly** counts as input on each Claude Code turn (conversation, `CLAUDE.md`, tool outputs, MCP tool schemas, etc.)? Cite the article(s).
2. Explain **why** a user might see **small per-message token counts** but **large plan-usage percentage jumps** on claude.ai — and how that differs from **session token totals** in Claude Code. Tie to official “models, usage, and limits” guidance where possible.
3. What is the **recommended hygiene** for long sessions (`/clear`, `/compact`, `/context`, `/cost`, `/usage`, model selection Sonnet vs Opus vs Haiku)? Cite **Claude Help Center** cheatsheet or equivalent.
4. Summarize **open community issues** about **cache_read** vs **subscription quota** (e.g. GitHub discussions). What experiments would prove or disprove the claim?

### B. Obsidian as an *external* structured layer (not a token bomb)

5. What **vault topologies** best support **progressive disclosure** for an AI coding agent?
   - Examples to evaluate: flat `instructions/` vs PARA vs MOC + “evergreen” notes vs dedicated `20-Projects/` vs `00-Inbox/`.
6. Which **Obsidian core features** most improve **human→agent handoff** with **minimal** risk of the agent reading the whole vault?
   - Graph, backlinks, aliases, properties/YAML, embeds (`![[...]]`), block references, tags, search.
7. Which **Obsidian plugin categories** are worth it vs harmful for **agent-adjacent** workflows?
   - LLM sidebar plugins, Dataview/DataviewJS, Templater, QuickAdd, Periodic Notes, Linter, Git, Obsidian Sync, etc.
   - For each category: **benefit**, **token risk if synced into agent context**, **operational complexity**, **failure modes**.

### C. Anthropic-native extension points (prioritize these)

8. **Skills** (`SKILL.md`, progressive disclosure): how should the user structure skills so Claude Code loads **only** relevant skill bodies? Cite Anthropic Claude Code docs on Skills.
9. **Hooks**: which hook events give the best ROI for **token safety** and **quality** (e.g. `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `SessionStart`)? Give **2–3 concrete hook recipes** (pseudo-config + shell outline) aligned with: format-on-save, block dangerous `rm`, warn on huge file reads, inject “be concise” reminders — without infinite recursion.
10. **Subagents**: when do Explore/Plan built-ins beat custom subagents? Give **decision criteria** and **3 example custom subagent definitions** (YAML/markdown as per current docs) for: (i) repo-wide search only, (ii) docs-only writer, (iii) test runner isolation.
11. **MCP**: inventory **Anthropic-first** MCP patterns: stdio vs remote, scoping servers per project, minimizing tool-schema token overhead. Cite official MCP docs where useful.

### D. Third-party tools — deep integration matrix

Produce a **comparison table** (≥12 rows) of tools with columns:

| Tool | Primary purpose | Integrates with | Token impact | Setup cost | Best for | Anti-patterns | Official link |

**Must include** (research each; add more if valuable):

- **Context7** (`@upstash/context7-mcp`) — live library docs.
- **Sequential Thinking** (`@modelcontextprotocol/server-sequential-thinking`) — structured reasoning.
- **claudectx** — analyze/optimize/MCP smart read.
- **GitHub MCP** (`@modelcontextprotocol/server-github`).
- **Filesystem MCP** (controlled paths).
- **PostgreSQL / SQLite MCP** (if applicable to dev workflows).
- **Obsidian MCP bridges** — research multiple implementations, e.g. community projects such as **`iansinnott/obsidian-claude-code-mcp`**, **`shivamsharmxa/obsidian-mcp-server`** / npm **`obsidian-claude-mcp`**, and **`Roasbeef/obsidian-claude-code`** (plugin). Compare **security model** (vault read/write scope), **transport** (stdio/WebSocket/HTTP), **concurrency**, **tool surface area**.
- **Obsidian LLM plugins** — e.g. **LLMSider**, **Large Language Models** plugin family, **Vault LLM Assistant** — compare to “keep LLM in Claude Code only” strategy.

### E. “Split brain” architecture — recommended patterns

12. Propose **3 reference architectures** (diagrams in ASCII or Mermaid) for:

- **Architecture 1 — Obsidian = human cockpit, Claude Code = executor** (strict separation; Obsidian never auto-ingested into Code).
- **Architecture 2 — Obsidian vault as read-mostly MCP knowledge** (narrow allowlist paths; nightly sync).
- **Architecture 3 — Monorepo instructions as source of truth** (current IA-Instructions style) + Obsidian graph for discovery only.

For each: **data flow**, **trust boundaries**, **where CLAUDE.md lives**, **what must never be duplicated**.

### F. Measurement & continuous improvement

13. Define a **weekly metrics ritual** for the user:
    - What to log (tokens, cache hit %, #tool calls, session length, model mix).
    - Which commands to run (`/cost`, `/usage`, `claudectx analyze`, etc.).
    - How to parse `~/.claude/projects/**.jsonl` **safely** (privacy cautions).
14. Propose **A/B experiments** (1 week each) with **hypothesis → metric → stop condition**.

### G. Security, privacy, compliance

15. Enumerate **top 10 risks** when connecting Claude Code to an Obsidian vault via MCP (path traversal, secret leakage in notes, plugin exfiltration, accidental commit of API keys, `.obsidian` leakage, etc.) and mitigations.
16. Provide a **red-team checklist** before enabling write tools against vault notes.

### H. Deliverables (OUTPUT FORMAT — STRICT)

Produce a single response with:

1. **Executive summary** (≤15 bullets).
2. **Findings by section A–G** with **citations** (URL + publisher + date if visible).
3. **Ranked recommendation list** (Top 20) with **impact / effort / risk** scores (1–5 each).
4. **Anti-patterns list** (≥15) specific to “Obsidian + Claude Code + MCP”.
5. **90-day implementation roadmap** (week-by-week milestones).
6. **Copy-paste artifacts**:
   - Example `.claude/settings.json` **snippet** for MCP servers (placeholders only).
   - Example **Skill** skeleton for “Obsidian handoff reader” that loads only an allowlisted note path.
   - Example **Hook** skeleton for “warn on reads >N KB”.
7. **FAQ** (≥12 Q&A) addressing misconceptions (e.g. “44K token cap”, “why 5 small prompts burned 20% quota”).
8. **Research gaps** — what you could not verify from primary sources; suggest **exact queries** the user should run next.

**Style constraints for your answer:**

- Prefer **tables** and **numbered lists** over prose.
- Every major claim should have a **citation** or be explicitly labeled **speculation/community report**.
- Do not tell the user to “just use best practices”; give **checklists** and **criteria**.

---

## OPTIONAL FOLLOW-UP (IF YOU HAVE CAPACITY)

- Simulate **3 day-in-the-life workflows** (solo dev, team lead reviewing PRs, OSS maintainer) showing **which tools fire when** and **expected token curve**.
- Propose **automation** using **Git hooks** + **Obsidian Git** plugin to keep `instructions/` and vault in sync without duplication.

---

## SUCCESS CRITERIA

Your answer is successful if a senior staff engineer could **execute the roadmap** next Monday **without asking clarifying questions**, and if the token-usage guidance is **consistent with Anthropic’s official metering explanations**.

---

**END OF PROMPT — paste from “## SYSTEM / ROLE” through here into ChatGPT.**
