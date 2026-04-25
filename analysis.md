# Analysis — Obsidian × Claude Code × IA-Instructions

**Purpose:** Single interpretation of the decision brief you supplied, plus a **concrete repo refactoring plan** for this repository (`IA-Instructions`).  
**Audience:** you (owner), future contributors, and Claude Code working in `project-02/`.

---

## 1. Interpretation — what the brief is really saying

### 1.1 North star (non-negotiable)

- **Cockpit / executor split:** humans navigate and connect knowledge in **Obsidian**; **Claude Code executes** against the **git repo** (`claude-code/instructions/` + your app repos). Obsidian is **not** a second always-on context brain for the agent.
- **Anthropic’s own lever:** always-on context must stay **lean** (`CLAUDE.md` short, reference loaded **on demand**). That matches your **router-first** design.

### 1.2 Three layers of “truth” (clarified)

| Layer | Holds | Loaded when | Token posture |
|-------|--------|----------------|---------------|
| **Invariant rules** | Short `CLAUDE.md` at workspace root (or project) | Every turn | Minimize; only what must always hold |
| **Manual skills** | `SKILL.md` bodies, `disable-model-invocation: true` | Human or explicit `/skill` — not eager listing | Best for deploy-like or sensitive workflows |
| **Plain markdown** | `router.md`, topic folders, `_MOC.md` | Only when router (or human) opens **one** path | Your main ROI today |

The brief says: **do not** promote `_MOC`/topic notes into `CLAUDE.md`; that would tax **every** request for human graph scaffolding.

### 1.3 Why the usage bar feels “wrong”

- The **subscription bar** is a **window-relative capacity meter** (5h + weekly patterns per Anthropic help articles), **not** a 1:1 raw-token odometer.
- Each **turn** resends **conversation + project context + new prompt**. Project context includes **`CLAUDE.md` + files already read** into the thread. So a “tiny” new prompt can still sit on a **large** payload → bar jumps that look irrational vs. “8K shown somewhere.”
- **Cache reads** are **not free** in API pricing (0.1× input); whether the **subscription bar** discounts them like API billing is **not publicly specified** in sources cited in the brief — treat as **unknown weighting**, not “free.”

### 1.4 Slash-command hygiene (order of operations)

Canonical sequence from the brief (aligned with Anthropic support messaging):

1. **`/context`** — see what’s fat (history vs `CLAUDE.md` vs MCP vs skills).
2. **`/compact`** — **same task**, worth preserving compressed history; know **skill re-attach limits** (5K per invoked skill, 25K combined after compaction).
3. **`/clear`** — **task changed**; strongest reset without losing repo files / `CLAUDE.md`.
4. **Model switch** — **after** context is sane; switching mid-bloat can force **re-read of full history without cached context** (per brief — verify in-session with `/context` + `/cost`).

### 1.5 MCP idle cost (nuanced)

- With **tool search** on, idle MCP cost is mostly **names + short descriptions**; **full JSON schemas** deferred until needed.
- If tool search is **off** or **broken on a host** (e.g. some Vertex/proxy setups), upfront tool definitions can blow context — brief cites **~10–20K for ~50 tools** loaded eagerly as order-of-magnitude from Anthropic’s tool-search docs.
- **Implication for this repo:** default **no Obsidian vault MCP** in committed project config; add servers only when measured.

### 1.6 Hooks that pay rent

High-ROI set from the brief:

| Hook | Role |
|------|------|
| **SessionStart** | Reinforce router discipline (informational unless you inject minimal text). |
| **PreToolUse (Read)** | Block huge / wrong-path reads (lockfiles, `.git`, exported vaults). |
| **PreToolUse (Bash)** | Hard safety on destructive commands. |
| **UserPromptSubmit** (optional) | Terse concision / scope reminder. |

**Caveat:** implement blocking hooks with **tests** and avoid **hook → Claude → hook** recursion; log to `.claude/logs/` paths that the router never tells the agent to read wholesale.

### 1.7 Obsidian topology choice

- **MOC + evergreen instruction notes** wins for **this** repo (you already have `_MOC.md` + `router.md`).
- **PARA** is a poor **primary** fit for a tight instruction corpus (invites broad folder reads).
- **Avoid** Smart Connections / vault-wide copilots / LLM sidebars as **default** — they fight the cockpit/executor split ([COMMUNITY] / analyst judgment in brief).

### 1.8 Integration ladder (safest → riskiest)

1. **Repo-as-vault** — Obsidian opens the **same folder** as git; no MCP; filesystem tools only. **Default.**
2. **Read-only handoff folder** + **manual skill** (`disable-model-invocation: true`, optional `context: fork`) — Obsidian content enters only when invoked.
3. **Local REST + MCP wrapper** — only after **A/B** shows material task success gain vs token cost; prefer **maintained** wrappers per brief’s matrix.
4. **Full-vault MCP** — **not** default; high drift + security surface.

### 1.9 Subagents vs forked skills (how to read the brief)

- **Subagent:** isolated context; good when only **summary** should return to parent.
- **`context: fork` on skill:** isolated run; good for **read-heavy** self-contained probes; bad for “static policy only” skills that return nothing useful.
- **Path allowlist** in subagent YAML: brief flags **no verified native field** — enforce scope with **prompt + hooks + repo layout**, not fantasy YAML.

---

## 2. Current repo reality (`IA-Instructions`)

| Asset | Role today | Brief alignment |
|-------|------------|-----------------|
| `claude-code/instructions/router.md` | Agent entry, one-file discipline | **Core** — keep as gatekeeper |
| `_MOC.md` + `.obsidian/` | Human graph, wikilinks | **Cockpit** — keep; keep out of `CLAUDE.md` |
| Topic `*.md` under `instructions/` | On-demand reference | **Correct** — plain markdown behind router |
| `mcp-config-example.json` | Sample Context7 + Sequential Thinking | OK as **example**; document “not all on by default” |
| **Missing** | `.claude/settings.json` with hooks, optional `.claude/skills/` | **Gap** — refactoring should add **versioned** project hooks/skills templates here |

**Monorepo note:** `c:\ia\CLAUDE.md` points at `c:\ia\instructions\README.md` as router (different filename than `router.md`). That’s a **second shape**. Refactoring plan below includes **convergence options** so you don’t maintain two mental models forever.

---

## 3. Refactoring plan — repo structure

### 3.1 Target layout (this repo after refactor)

```text
IA-Instructions/
├── analysis.md                         ← this file (decision + plan)
├── README.md                           ← link to analysis; keep high-level
├── LICENSE
│
├── claude-code/
│   ├── README.md
│   ├── templates/                      ← NEW: copy into consumer repos
│   │   ├── CLAUDE.md.minimal.md       ← invariant-only template
│   │   ├── settings.hooks.example.json
│   │   └── skills/                    ← example SKILL.md files (manual-only)
│   │       ├── obsidian-handoff-reader/
│   │       │   └── SKILL.md
│   │       └── ... (optional)
│   └── instructions/                   ← unchanged role: router + MOC + guides
│       ├── router.md
│       ├── _MOC.md
│       ├── OBSIDIAN.md
│       └── ...
│
├── ChatGPT/
├── Gemini/
└── docs/                               ← OPTIONAL NEW: ADRs
    └── ADR-001-cockpit-executor.md
```

**Principles:**

- **No** wholesale move of markdown into `CLAUDE.md`.
- **Templates** live under `claude-code/templates/` so consumers copy **hooks + skills + minimal CLAUDE** without forking your entire instruction tree.
- **`instructions/`** remains the **canonical instruction corpus** and Obsidian vault root for humans editing this repo.

### 3.2 Content splits (what moves where)

| Content type | Today (typical) | After refactor |
|--------------|-----------------|----------------|
| “Always on” coding style | Root `CLAUDE.md` in `c:\ia` | **Split:** (1) consumer’s short `CLAUDE.md` (2) long style stays out of always-on or becomes a **manual skill** |
| Router rules | `router.md` | Keep; add **one line** pointing to `analysis.md` for humans only (“do not load in agent unless asked”) |
| MCP policy | scattered in READMEs | Single **`instructions/mcp-policy.md`** (short): default servers, when to add Obsidian bridge, `ENABLE_TOOL_SEARCH` note |
| Hooks | none in repo | **`templates/settings.hooks.example.json`** + documented shell scripts |
| Skills | none in repo | **`templates/skills/**`** per brief (handoff reader, etc.) |

### 3.3 Router and `_MOC` hygiene

- **`router.md`:** keep tables tight; any new file = **one new row** + **one line in `_MOC`**.
- **`_MOC.md`:** for humans + Obsidian graph; add frontmatter `excludeFromRouter: true` is **not** a real Obsidian field — instead add a **comment line** at top: “Claude: do not read unless user asks for full map.”
- **Avoid** embedding huge embeds `![[...]]` in routed notes — agent sees raw syntax; humans get value, agent may get noise.

### 3.4 MCP configuration policy (committed vs local)

| File | Committed? | Purpose |
|------|-------------|---------|
| `mcp-config-example.json` | Yes | **Starter** only |
| `instructions/mcp-policy.md` | Yes | Rules: tool search, when to disable servers, no default vault MCP |
| `.claude/settings.json` (real) | **Prefer gitignored** in personal clones OR committed **without secrets** in this instructional repo | If committed here, keep **minimal** MCP entries only |

**Recommendation for `IA-Instructions`:** add **`mcp-policy.md`** + keep **`mcp-config-example.json`** as “copy me”; do **not** commit personal API keys.

### 3.5 Hooks and scripts (concrete backlog)

1. **`block-large-read.sh`** — as in brief (threshold env `CLAUDE_MAX_READ_BYTES`).
2. **`block-dangerous-bash.sh`** — deny-list `rm -rf`, `mkfs`, `dd`, etc. (extend carefully).
3. **`sessionstart-router-reminder.sh`** — echo one-line reminder to stdout (only if Anthropic documents that SessionStart hook output is safe/desired — **verify** against current hooks doc before relying on behavior).

**Deliverable:** `claude-code/templates/hooks/` + README “how to install into `$PROJECT/.claude/`”.

### 3.6 Skills backlog (from brief)

| Skill | `disable-model-invocation` | `context: fork` | Purpose |
|-------|------------------------------|------------------|---------|
| `obsidian-handoff-reader` | `true` | optional `true` | Read `<OBSIDIAN_HANDOFF_DIR>/` only |
| `instructions-explorer` | `true` or `false` | — | Read-only narrow tree for ambiguous tasks |
| Side-effect workflows (deploy, etc.) | `true` | per task | Prevent auto-invoke |

**Note:** Skill frontmatter must match **current** Anthropic docs when you implement (field names drift).

### 3.7 README changes (this repo)

- Root **`README.md`**: add **“Architecture”** section with link to **`analysis.md`**; one diagram (cockpit vs executor).
- **`claude-code/README.md`**: link **`analysis.md`**; “install templates” section.

### 3.8 Monorepo convergence (`c:\ia` vs `project-02`)

Pick **one** long-term story:

| Option | Pros | Cons |
|--------|------|------|
| **A — Rename router** | Make `c:\ia\instructions/README.md` → **`router.md`** symlink or duplicate + single name in `CLAUDE.md` | One-time churn |
| **B — CLAUDE points to GitHub** | `CLAUDE.md` says “canonical instructions: clone `IA-Instructions` subfolder” | Extra clone for agents |
| **C — Symlink `instructions` → submodule** | Single source | Submodule friction |

**Recommendation:** **Option A** — align on **`router.md`** everywhere; keep `README.md` in GitHub as **human** landing only **or** make `README.md` a thin redirect to `router.md` content (duplication risk — prefer one file).

---

## 4. Phased execution roadmap (merged with brief’s 90 days)

| Phase | Weeks | Outcomes | Done when |
|-------|-------|-----------|-----------|
| **P0 — Docs in repo** | 1 | Land `analysis.md`, `mcp-policy.md`, template `CLAUDE.md.minimal.md` | Merged to `main` |
| **P1 — Hooks template** | 1–2 | `block-large-read`, `block-dangerous-bash`, documented install | CI-less manual test checklist passes |
| **P2 — Skills template** | 2–3 | `obsidian-handoff-reader` skeleton + `instructions-explorer` | Copied into a real project and invoked successfully |
| **P3 — CLAUDE split experiment** | 3–5 | Trim consumer `CLAUDE.md`; measure `/context` at first work turn | Median starting context ↓ without quality regression |
| **P4 — A/B manual-only skills** | 5–7 | Flip skills to `disable-model-invocation: true` where applicable | Median `/context` ↓; failure rate stable |
| **P5 — Optional Obsidian bridge** | 8–10 | **Only if** P4 shows real need for non-repo notes | A/B: repo-only vs handoff wins on **usage AND accuracy** |
| **P6 — Consolidation** | 11–13 | Remove failed experiments; document ADRs | Single architecture doc |

---

## 5. Risks & mitigations (repo-specific)

| Risk | Mitigation |
|------|------------|
| Contributors add “just one more” bullet to `CLAUDE.md` | PR review rule + `CLAUDE.md.minimal.md` template as gold standard |
| Hooks block legitimate reads | Start **high** threshold; log allows/denies; tune with real traces |
| `.obsidian/workspace` accidentally committed | Keep **`.gitignore`** (already in `instructions/`) — extend if new noise appears |
| Two routers (`README` vs `router`) diverge | Convergence milestone (§3.8) |
| Vault MCP enabled “for demo” | **Do not** commit full-vault MCP in `mcp-config-example.json`; document in `mcp-policy.md` |

---

## 6. “Could not verify” — carry-forward queries

Use these exact searches when you want primary-source closure (from brief):

- `site:code.claude.com/docs/en "system prompt" "Claude Code" input tokens`
- `site:support.claude.com "usage bar" tokens "Claude Code"`
- `site:support.claude.com cache_read subscription usage limits Claude`
- `site:code.claude.com/docs/en subagents path allowlist skills frontmatter`
- `site:code.claude.com/docs/en "~/.claude/projects" jsonl Claude Code`
- `site:github.com/modelcontextprotocol filesystem MCP server releases`

---

## 7. One-page decision record

| Decision | Status |
|----------|--------|
| Default architecture | **Cockpit (Obsidian) / Executor (repo + Claude Code)** |
| Vault MCP in default path | **No** |
| Topology | **MOC + evergreen** instructions; `router.md` gatekeeper |
| `CLAUDE.md` | **Short**, invariant + router discipline only |
| Bulky / side-effect procedures | **Manual skills** (`disable-model-invocation: true`) |
| Slash-command order | **`/context` → `/compact` (same task) → `/clear` (new task)`** |
| Model switch | **After** context hygiene |
| Obsidian bridge | **Last**; handoff folder + manual skill before any REST/MCP vault |

---

## 8. Source classification in this analysis

| Tag | Meaning |
|-----|---------|
| **Anthropic primary** | Docs / Help Center — prefer for behavior claims |
| **USER-PROVIDED** | Your brief’s explicit choices for this repo |
| **ANALYST JUDGMENT** / **[COMMUNITY]** | As in your brief — not re-verified line-by-line in this write-up |

---

*Last updated: generated as part of repo planning. Update this file when ADRs or experiments change outcomes.*
