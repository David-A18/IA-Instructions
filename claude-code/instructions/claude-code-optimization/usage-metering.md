# Claude Code: usage vs. the web app (why numbers do not match)

## The 44K myth

**There is no fixed "44K max" per session in Claude Code.** Plan limits are **not** the same as "tokens per message" you see in the UI.

| What people confuse | Reality |
|---------------------|---------|
| "44K" | Often: Pro's **5-hour rolling message budget** (varies by plan), **not** a context cap. See [Models, usage, and limits in Claude Code](https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code). |
| Context window | **200K** on standard paid web chat (Claude plans). **Up to 1M** for some models in Claude Code (Opus/Sonnet per [Help Center](https://support.claude.com/en/articles/8606394-how-large-is-the-context-window-on-paid-claude-plans)). |
| "I only sent 8K" | Each **turn** bills the **whole context** re-sent: chat history + `CLAUDE.md` + files already in thread + tool results — not just your new prompt. |

So: **200K+ "session" tokens in Code is normal** for a long agentic session. The web "50% for 200K" and Code "30%" are **different meters** on the same pool (subscription): chat vs. agent turns, model, and weighting.

---

## How usage is metered (official)

From Anthropic: **how you sign in** changes what you see, not how Code works.

| Sign-in | Metering | "Run out" means |
|---------|----------|-----------------|
| Claude plan (Pro/Max/Team) | Included pool, **rolling window** | "Limit reached, resets at …" |
| **API key** (Console/Bedrock/Vertex) | **Per token**, pay-as-you-go | No hard cap; `/cost` shows session spend |

Source: [Models, usage, and limits in Claude Code](https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code)

---

## What one "turn" actually sends (why small prompts still cost a lot)

Every API call includes:

1. **Full conversation so far** (grows every message).
2. **Project context** — at least `CLAUDE.md` and anything already read into the session.
3. **Your new prompt.**
4. **Tool / MCP** — system prompts, tool definitions, and **tool outputs** (file reads, shell output) add up fast.

So **5 short prompts** can move the dashboard **15–20%** if each turn still carries **hundreds of K** of context from prior turns, or if **multiple Code sessions** are open, or if **Opus** is selected vs **Sonnet** (higher per-turn plan weight).

Official line: *"A long debugging session in which Claude has read twenty files and produced fifteen diffs is carrying all of that on every subsequent message."*  
— same article as above.

---

## Web usage % vs. Claude Code session tokens

| Web (claude.ai usage) | Claude Code |
|------------------------|------------|
| Mostly **chat** turns, smaller average context. | **Agentic** loops: read file → plan → edit → test → re-read. Same "message" in UI is many **API calls**. |
| One conversation surface. | **Multiple terminals/sessions** each consume the pool. **Background sessions** still burn quota (see community reports). |
| % is **plan-specific allocation**, not raw token math you can hand-verify. | **Session** can show 200K+ **tokens** while dashboard shows a **fraction of plan** — different **units and aggregation windows**. |

**Rule:** Do not expect `8K label on last reply × N = X%` on the website. The backend counts **model-weighted** usage and **full context** per call.

---

## Cache reads and plan quota (known confusion)

**API billing:** `cache_read` is cheaper than full input (see [pricing](https://platform.claude.com/docs/en/about-claude/pricing)).

**Subscription / rate-limit side:** There are **reports** that **cache read tokens** may count **more heavily** toward some **plan** limits than users expect, making quota drop fast when context is large. See discussion: [anthropics/claude-code#45756](https://github.com/anthropics/claude-code/issues/45756) (open bug, not official confirmation — treat as *hypothesis* if your numbers look wrong).

**What to do:** If quota burns faster than "effective tokens" suggest, use `/context`, **fewer sessions**, **lighter model** for routine work, and **`/clear`** between tasks.

---

## Commands to understand your *real* spend

| Command | Use |
|---------|-----|
| `/cost` | Session token/dollar view (esp. **API key** sign-in). |
| `/context` | How full the context is (not the same as plan % on website). |
| `/model` | Sonnet (default) vs Opus (costs more per turn) vs Haiku (cheap). |
| `/compact` | Shrink history without wiping project memory. |
| `/clear` | New task → strongest savings (drops chat history, keeps `CLAUDE.md` + project). |

From: [Models, usage, and limits in Claude Code](https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code)

**Advanced:** Session logs under `~/.claude/projects/.../*.jsonl` can include `usage` with `input_tokens`, `output_tokens`, `cache_read_input_tokens`, `cache_creation_input_tokens` per response (as in #45756).

---

## Checklist: stop "mystery" high usage

1. **One task per session** — `/clear` when switching features (biggest win).
2. **Sonnet** for execution; **Opus** only for hard debug/plan; **Haiku** for tiny edits.
3. **Slim `CLAUDE.md`** — it is prepended every turn (aim **well under 200 lines**; tighter is better).
4. **Do not paste huge blobs** — put files on disk; reference path (and avoid `@` full-file inject when you want to save tokens).
5. **Disconnect unused MCP servers** — each adds schema + tool overhead.
6. **Close idle Code sessions** — parallel sessions all draw from the same pool.
7. **`/compact` before** context is ~60%+ full to avoid quality + cost drop-off.

---

## One-line mental model

**Dashboard % = plan-specific weighted usage in a time window. Code session = sum of (full context × turns × model × tools).** They will not line up with "8K × 5 prompts" arithmetic.

**Official hub:** [Models, usage, and limits in Claude Code](https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code)
