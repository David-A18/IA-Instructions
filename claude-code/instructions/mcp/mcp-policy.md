# MCP Policy

Use MCP servers only when they materially reduce uncertainty or provide tools the model cannot get from local files.

## Default Policy

- Keep the baseline MCP set small.
- Prefer read-only or narrow-scope servers.
- Do not connect a full personal Obsidian vault by default.
- Do not commit secrets in MCP settings.
- Keep copyable settings as examples with placeholders only.

## Recommended Starter Servers

| Server | Use | Reference |
| --- | --- | --- |
| Context7 | Current library docs | [../context7/config-and-tools.md](../context7/config-and-tools.md) |
| Sequential Thinking | Structured planning and debugging | [../sequential-thinking/tool-reference.md](../sequential-thinking/tool-reference.md) |
| claudectx MCP | Symbol-level reads and token audits | [../claudectx/mcp-server.md](../claudectx/mcp-server.md) |

## Avoid

- Broad filesystem servers pointed at home directories.
- Vault-wide note servers with write access unless a task explicitly needs them.
- Servers that duplicate local repo search.
- Long server lists that add tool noise without clear value.

## Setup

Use [../mcp-config-example.json](../mcp-config-example.json) as a starter only. Copy it into the target project's local settings and remove unused servers.
