# Sequential Thinking Skill for Hermes Agent

Hermes Agent skill for systematic problem-solving through iterative reasoning with revision and branching capabilities.

## What This Skill Does

Enables Hermes to break down complex problems into sequential thought steps, revise conclusions when needed, and explore alternative solution paths while maintaining context throughout the reasoning process.

## Installation

This skill uses Hermes Agent's native MCP client. Configure the Sequential Thinking MCP server in `~/.hermes/config.yaml`, then install this skill into your Hermes skills directory or via your preferred Hermes skill-distribution workflow.

### Step 1: Configure the MCP Server

Add the server under `mcp_servers` using a stable server name. With the server name `sequential-thinking`, Hermes registers the MCP tool as `mcp_sequential_thinking_sequentialthinking` because MCP tool names follow `mcp_{server_name}_{tool_name}` and hyphens are converted to underscores.

#### NPX

```yaml
mcp_servers:
  sequential-thinking:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-sequential-thinking"]
```

#### Docker

```yaml
mcp_servers:
  sequential-thinking:
    command: "docker"
    args: ["run", "--rm", "-i", "mcp/sequentialthinking"]
```

### Step 2: Expose the MCP Tool to Hermes

Make sure the `sequential-thinking` MCP server is included in the active Hermes `toolsets:` list when your Hermes profile uses explicit toolsets:

```yaml
toolsets:
  - hermes-cli
  - sequential-thinking
```

After config changes, use `/reload-mcp` if available, or restart Hermes/start a fresh session so the MCP tools are discovered and injected into the current session.

### Step 3: Add the Skill

For a local checkout, copy this skill folder into the active Hermes profile's skills tree:

```bash
mkdir -p ~/.hermes/skills/software-development
cp -R sequential-thinking ~/.hermes/skills/software-development/sequential-thinking
```

If you are packaging this repository as a remote Hermes skill source, keep `SKILL.md` at the repository root and preserve the `references/` directory next to it.

### Step 4: Verify Installation

Check that Hermes can see the MCP server and tool:

```bash
hermes mcp list
hermes mcp test sequential-thinking
```

In a fresh Hermes session, the callable MCP tool should be named:

```text
mcp_sequential_thinking_sequentialthinking
```

## Usage

Once installed, Hermes can use this skill when:

- Facing complex multi-step problems
- Needing to revise earlier conclusions
- Exploring alternative solution approaches
- Working through uncertain or evolving scopes

You can also explicitly request it:

```text
Think through this step-by-step using the sequential-thinking skill.
```

## Configuration

### Disable Thought Logging (Optional)

To suppress thought information logging, set the environment variable on the MCP server:

```yaml
mcp_servers:
  sequential-thinking:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    env:
      DISABLE_THOUGHT_LOGGING: "true"
```

## Skill Structure

```text
sequential-thinking/
├── SKILL.md              # Main skill definition
├── README.md             # This file
└── references/
    ├── advanced.md       # Revision and branching patterns
    └── examples.md       # Real-world use cases
```

## When Hermes Uses This Skill

The skill activates for:

- **Complex analysis**: Breaking down multi-faceted problems
- **Design decisions**: Exploring and comparing alternatives
- **Debugging**: Systematic investigation with course correction
- **Planning**: Multi-stage project planning with evolving scope
- **Architecture**: Evaluating trade-offs across approaches

## Learn More

- [MCP Sequential Thinking Server](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Hermes Agent Skills Catalog](https://hermes-agent.nousresearch.com/docs/reference/skills-catalog)

## License

MIT
