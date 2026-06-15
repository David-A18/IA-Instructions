# Context Route

Use this route when the task depends on current library, SDK, framework, or provider documentation.

## Decision

| Situation | Action |
| --- | --- |
| Need current docs for a package, cloud SDK, framework, or API | Read [../context7/usage-guide.md](../context7/usage-guide.md). |
| Need to configure Context7 or inspect tool names | Read [../context7/config-and-tools.md](../context7/config-and-tools.md). |
| Need to understand what Context7 is before installing it | Read [../context7/README.md](../context7/README.md). |

## Rules

- Ask for exact library IDs or versions when ambiguity changes implementation.
- Prefer current docs over model memory for fast-moving APIs.
- Do not query docs for stable repo facts that can be read locally.
- Do not leave unused MCP servers enabled in projects where they are not needed.
