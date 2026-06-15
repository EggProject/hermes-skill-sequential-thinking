# Sequential Thinking Skill for Hermes Agent

Hermes Agent skill for systematic problem-solving through iterative reasoning with revision and branching capabilities.

This repository ports the upstream sequential-thinking skill for Hermes Agent's native MCP naming convention and skill hub/tap layout.

## Original Source

- Original skill repository: [mrgoonie/claudekit-skills](https://github.com/mrgoonie/claudekit-skills)
- Original skill path: [`.claude/skills/sequential-thinking`](https://github.com/mrgoonie/claudekit-skills/tree/main/.claude/skills/sequential-thinking)
- License: MIT

## Skill Layout

```text
skills/
└── sequential-thinking/
    ├── SKILL.md
    └── references/
        ├── advanced.md
        └── examples.md
```

The `skills/sequential-thinking/` directory is the installable Hermes skill bundle. Keeping the skill inside `skills/<skill-name>/` lets `hermes skills tap add EggProject/hermes-skill-sequential-thinking` index the repository and install the full bundle, including reference files.

## Installation

### Option 1: Install via Hermes tap

```bash
hermes skills tap add EggProject/hermes-skill-sequential-thinking
hermes skills search sequential-thinking
hermes skills install EggProject/hermes-skill-sequential-thinking/skills/sequential-thinking --category software-development --yes
```

### Option 2: Install from a local checkout

```bash
mkdir -p ~/.hermes/skills/software-development
cp -R skills/sequential-thinking ~/.hermes/skills/software-development/sequential-thinking
```

## Configure the Sequential Thinking MCP Server

This skill expects Hermes to expose the Sequential Thinking MCP server using the stable server name `sequential-thinking`. With that server name, Hermes registers the MCP tool as `mcp_sequential_thinking_sequentialthinking` because MCP tool names follow `mcp_{server_name}_{tool_name}` and hyphens are converted to underscores.

### NPX

```yaml
mcp_servers:
  sequential-thinking:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-sequential-thinking"]
```

### Docker

```yaml
mcp_servers:
  sequential-thinking:
    command: "docker"
    args: ["run", "--rm", "-i", "mcp/sequentialthinking"]
```

After config changes, use `/reload-mcp` if available, or restart Hermes/start a fresh session so the MCP tools are discovered.

## Verify

```bash
hermes mcp list
hermes mcp test sequential-thinking
```

In a fresh Hermes session, the callable MCP tool should be named:

```text
mcp_sequential_thinking_sequentialthinking
```

## License

MIT
