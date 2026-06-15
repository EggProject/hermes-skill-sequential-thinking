# Installation

The skill can be installed in two ways. Pick one, then configure the
Sequential Thinking MCP server and verify.

> **Prerequisite:** The Sequential Thinking MCP server must be reachable
> from wherever Hermes runs. This skill **requires** MCP at runtime —
> it has no prompt-only fallback. See [Configure the Sequential
> Thinking MCP server](#configure-the-sequential-thinking-mcp-server)
> below for NPX and Docker recipes.

## Option 1 — Install via Hermes tap

```bash
hermes skills tap add EggProject/hermes-skill-sequential-thinking
hermes skills search sequential-thinking
hermes skills install EggProject/hermes-skill-sequential-thinking/skills/sequential-thinking --category software-development --yes
```

The `tap add` step registers the GitHub repository as a discoverable
Hermes skill source. `search` confirms it is indexed, and `install`
copies `skills/sequential-thinking/` (including both `references/*.md`)
into `~/.hermes/skills/software-development/sequential-thinking/`.

## Option 2 — Install from a local checkout

```bash
git clone https://github.com/EggProject/hermes-skill-sequential-thinking.git
cd hermes-skill-sequential-thinking
mkdir -p ~/.hermes/skills/software-development
cp -R skills/sequential-thinking ~/.hermes/skills/software-development/sequential-thinking
```

Use this if you want to edit the skill before installing, or if your
Hermes host has no network access to GitHub taps.

## Configure the Sequential Thinking MCP server

The skill expects Hermes to expose the Sequential Thinking MCP server
using the stable server name `sequential-thinking`. With that server
name, Hermes registers the MCP tool as
`mcp_sequential_thinking_sequentialthinking` — because MCP tool names
follow `mcp_{server_name}_{tool_name}` and hyphens are converted to
underscores.

If you change the server name (e.g. `seq-think`), the resulting tool
name will change too (`mcp_seq_think_sequentialthinking`), and the
skill's tool references will not resolve. **Keep the server name
`sequential-thinking`.**

### NPX

```yaml
mcp_servers:
  sequential-thinking:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-sequential-thinking"]
```

This is the official upstream package:
[`@modelcontextprotocol/server-sequential-thinking`](https://www.npmjs.com/package/@modelcontextprotocol/server-sequential-thinking).
NPX pulls and runs it on first use; subsequent invocations are cached.

### Docker

```yaml
mcp_servers:
  sequential-thinking:
    command: "docker"
    args: ["run", "--rm", "-i", "mcp/sequentialthinking"]
```

Use this if you prefer a sandboxed, image-based runtime, or if NPX is
not available in your environment.

## Reload MCP

After changing the MCP config, the new tools are not picked up by the
current session. Reload with whichever mechanism your Hermes host
exposes:

- `/reload-mcp` slash command (if available)
- Restart Hermes
- Start a fresh session

## Verify

```bash
hermes mcp list
hermes mcp test sequential-thinking
```

In a fresh Hermes session, the callable MCP tool should be named:

```text
mcp_sequential_thinking_sequentialthinking
```

If `hermes mcp list` does not show `sequential-thinking`, double-check
that the server name in your MCP config is **exactly**
`sequential-thinking` (lowercase, hyphenated, no alias).

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `hermes mcp list` shows no `sequential-thinking` server | MCP config not reloaded | `/reload-mcp` or restart Hermes |
| Tool is named `mcp_<other>_sequentialthinking` | Server name was changed | Rename server back to `sequential-thinking` |
| `hermes mcp test sequential-thinking` fails with `npx: command not found` | Node / NPX not installed | Install Node ≥ 18, or switch to the Docker config |
| `hermes mcp test sequential-thinking` fails with `docker: command not found` | Docker not installed | Install Docker, or switch to the NPX config |
| Tool is discovered but every call returns an error | MCP server crashed on first run | Check `hermes mcp logs sequential-thinking`; the official package needs network access on first NPX run |
