# Compatibility

The skill is built for Hermes Agent and the Sequential Thinking MCP
server. Below is what is supported, what is not, and why.

## Supported

| Surface | Status | Notes |
|---------|--------|-------|
| **Hermes Agent — skill hub / tap** | ✅ First-class | `skills/sequential-thinking/` layout is tap-ready. |
| **Hermes Agent — skill install** | ✅ | The bundle drops into `~/.hermes/skills/<category>/sequential-thinking/`. |
| **Hermes Agent — MCP tool discovery** | ✅ | Stable server name `sequential-thinking` → tool `mcp_sequential_thinking_sequentialthinking`. |
| **Sequential Thinking MCP server (NPX)** | ✅ | `@modelcontextprotocol/server-sequential-thinking` via `npx -y ...`. |
| **Sequential Thinking MCP server (Docker)** | ✅ | `mcp/sequentialthinking` image, `--rm -i`. |
| **Claude Code** | ✅ | Claude Code is MCP-aware. The same `sequential-thinking` server name and tool name work; the skill `SKILL.md` is written for Hermes but the MCP call is portable. |
| **Any MCP-aware host** | ✅ | Anything that can register an MCP server named `sequential-thinking` and call `mcp_sequential_thinking_sequentialthinking`. |

## Not supported

| Surface | Status | Why |
|---------|--------|-----|
| **Anthropic `skill-creator` workflow** | ❌ Not used | This skill is hand-ported, not generated. There is no `.claude/skills/skill-creator` dependency in this repository. |
| **ChatGPT (web, Plus, Team, Enterprise)** | ❌ | ChatGPT does not expose the Sequential Thinking MCP server. There is no prompt-only path to the tool. |
| **Claude web UI** | ❌ | The web UI has no MCP client. You cannot load a custom MCP server there. |
| **Claude Cowork** | ❌ | Cowork runs the standard web UI under the hood; same restriction. |
| **Gemini (web, app, API)** | ❌ | Gemini has no MCP integration in the consumer surfaces. |
| **Other chat-only AI surfaces** | ❌ | Anything that cannot connect to an MCP server cannot run this skill. |

## Why this skill cannot run on chat-only UIs

The skill is a wrapper around an MCP tool — it does not contain the
sequential-thinking logic itself. The actual reasoning engine is the
Sequential Thinking MCP server, which must be running somewhere
reachable (locally via NPX, or as a Docker container).

There is no pure-prompt alternative because the Sequential Thinking
workflow relies on **stateful, structured calls** — the tool needs to
remember the chain of thoughts, track revisions, and manage branches.
You cannot replicate that with a one-shot prompt in a chat UI.

If you need to use sequential thinking on a non-MCP surface, copy the
contents of `skills/sequential-thinking/references/examples.md` into
the chat and work through the steps manually. It is the closest
thing to a "free-tier recipe" — but it is not the skill, and it does
not benefit from the MCP tool's state tracking.

## MCP server name — why it matters

The skill's `SKILL.md` instructs Hermes to call
`mcp_sequential_thinking_sequentialthinking`. That tool name is **not
a constant** — it is derived from the MCP server name:

```text
mcp_{server_name_with_underscores}_{tool_name}
```

If you register the Sequential Thinking MCP server under any name
other than `sequential-thinking`, the tool name changes too, and the
skill's instruction no longer matches a real tool. Hermes will then
either fail to find the tool or call the wrong one.

**The server name must be exactly `sequential-thinking`** — lowercase,
hyphenated, no alias.

## Tested with

- Hermes Agent (skill hub/tap install path)
- `@modelcontextprotocol/server-sequential-thinking` (latest on npm)
- `mcp/sequentialthinking` Docker image
