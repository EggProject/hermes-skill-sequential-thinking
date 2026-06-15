# Skill structure

The skill bundle lives in `skills/sequential-thinking/` inside this
repository. That directory **is** the installable Hermes skill — when
you `cp -R` it into `~/.hermes/skills/<category>/sequential-thinking/`
(or when a Hermes tap installs it for you), it drops in complete.

## On-disk layout

```text
skills/
└── sequential-thinking/
    ├── SKILL.md
    └── references/
        ├── advanced.md
        └── examples.md
```

| Path | Role |
|------|------|
| `SKILL.md` | The skill's own system prompt. Frontmatter declares the MCP tool name and metadata; the body teaches Hermes *when* and *how* to use the tool. |
| `references/advanced.md` | Loaded on demand by the SKILL — revision and branching patterns, dynamic scope adjustment, session management, best practices. |
| `references/examples.md` | Loaded on demand by the SKILL — four end-to-end workflows showing the tool in action. |

The `references/` folder is **part of the installable bundle**. The
skill frontmatter does not pin a specific reference file — Hermes
loads it on demand based on the conversational context. Do not delete
or rename these files; the skill assumes they exist.

## Why `skills/<name>/` and not the repo root?

The `skills/sequential-thinking/` placement is what makes this
repository **tap-installable**. `hermes skills tap add
EggProject/hermes-skill-sequential-thinking` indexes the repository
and treats the contents of `skills/sequential-thinking/` as the
installable bundle. If the SKILL.md were at the repository root, the
tap would still work, but having a dedicated `skills/` subfolder
makes the bundle boundary explicit and leaves room for sibling
skills in the same repository later.

## `SKILL.md` frontmatter

```yaml
---
name: sequential-thinking
description: Use when complex problems require systematic step-by-step reasoning with Hermes MCP's sequential-thinking tool, including revisions, branches, and dynamic scope adjustment. Ideal for multi-stage analysis, design planning, problem decomposition, root-cause debugging, and tasks with initially unclear scope.
license: MIT
metadata:
  source: https://github.com/mrgoonie/claudekit-skills/tree/main/.claude/skills/sequential-thinking
  port: Hermes Agent native MCP tool name mcp_sequential_thinking_sequentialthinking
  hermes:
    tags: [reasoning, mcp, sequential-thinking, planning, debugging]
    related_skills: [native-mcp, software-development]
---
```

| Field | Why it matters |
|-------|----------------|
| `name` | Stable skill identifier — used by Hermes for tool routing and discovery. |
| `description` | The single most important field for skill matching. Hermes uses it to decide **when** to load the skill. Be specific; "use when X" phrasing is recommended. |
| `license` | MIT — matches the upstream and the project license. |
| `metadata.source` | Pointer to the upstream repository, so the provenance is explicit. |
| `metadata.port` | Documents the Hermes-specific MCP tool name that the skill depends on. |
| `metadata.hermes.tags` | Discovery tags for the Hermes skill hub. |
| `metadata.hermes.related_skills` | Skills that commonly pair with this one. |

## How Hermes loads the skill

1. User prompt arrives.
2. Hermes scores each installed skill's `description` against the
   prompt.
3. If `sequential-thinking` matches, Hermes loads `SKILL.md` into
   context.
4. The skill's `SKILL.md` body tells Hermes to call
   `mcp_sequential_thinking_sequentialthinking`, and references
   `references/advanced.md` and `references/examples.md` for
   deeper patterns.
5. Hermes makes the MCP tool calls as instructed.

This is why both the `description` and the frontmatter matter: the
first decides **whether** the skill is invoked, the second decides
**how** it is invoked.

## Editing the skill

If you fork this repository and modify the skill:

- Keep the `name` and `description` in sync with what the skill
  actually does — they are the discovery surface.
- Keep the `references/` files. Removing them breaks the "see
  references/advanced.md" instructions in `SKILL.md`.
- Keep the MCP tool name `mcp_sequential_thinking_sequentialthinking`
  in any code samples. Changing it requires renaming the MCP server
  too.
