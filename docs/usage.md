# Usage

The skill is a thin wrapper around the
`mcp_sequential_thinking_sequentialthinking` MCP tool. You invoke the
tool in a Hermes session, and the skill's `SKILL.md` instructs Hermes
*when* and *how* to use it.

## When to use it

Use `mcp_sequential_thinking_sequentialthinking` when:

- The problem requires **multiple interconnected reasoning steps**
- The **initial scope or approach is uncertain** and you want to refine
  the estimate as you go
- You need to **filter through complexity to find core issues** —
  divide and conquer
- You may need to **backtrack or revise earlier conclusions** based
  on new evidence
- You want to **explore alternative solution paths in parallel** and
  pick the best

**Don't use it for:** simple queries, direct facts, or single-step
tasks. A sequential chain adds overhead that is not justified when
the answer is one lookup away.

## Basic usage

The Hermes MCP tool `mcp_sequential_thinking_sequentialthinking`
accepts these parameters:

### Required parameters

| Parameter | Type | Purpose |
|-----------|------|---------|
| `thought` | string | The current reasoning step — express it as a self-contained chunk of thought. |
| `nextThoughtNeeded` | boolean | Whether more reasoning is needed. Set to `false` on the final thought. |
| `thoughtNumber` | integer | Current step number. Starts at `1`. |
| `totalThoughts` | integer | Estimated total steps needed. Adjustable mid-run. |

### Optional parameters

| Parameter | Type | Purpose |
|-----------|------|---------|
| `isRevision` | boolean | Marks this thought as a correction to an earlier one. |
| `revisesThought` | integer | Which `thoughtNumber` is being reconsidered. Required when `isRevision: true`. |
| `branchFromThought` | integer | The `thoughtNumber` this branch starts from. |
| `branchId` | string | Identifier for the reasoning branch this thought belongs to. |

## Workflow pattern

```text
1. Start with thoughtNumber: 1, an initial `totalThoughts` estimate,
   and nextThoughtNeeded: true.
2. For each step:
   - Express current reasoning in `thought`.
   - Update `totalThoughts` to reflect your new estimate.
   - Set `nextThoughtNeeded: true` to continue.
3. When you reach a conclusion, set `nextThoughtNeeded: false`.
```

The estimate in `totalThoughts` is **progress visibility**, not a
contract — it is fine to grow or shrink it. What matters is that
Hermes can see roughly where you are in the chain.

## Simple example

```typescript
// First thought
{
  thought: "Problem involves optimizing database queries. Need to identify bottlenecks first.",
  thoughtNumber: 1,
  totalThoughts: 5,
  nextThoughtNeeded: true
}

// Second thought
{
  thought: "Analyzing query patterns reveals N+1 problem in user fetches.",
  thoughtNumber: 2,
  totalThoughts: 6, // Adjusted scope
  nextThoughtNeeded: true
}

// ... continue until done
```

## Advanced features

The `SKILL.md` references two companion files inside the skill
bundle:

- **[`references/advanced.md`](../skills/sequential-thinking/references/advanced.md)**
  — revision and branching patterns, dynamic scope adjustment,
  session management, best practices
- **[`references/examples.md`](../skills/sequential-thinking/references/examples.md)**
  — four end-to-end workflows: database performance, architecture
  decision with branching, debugging with revision, complex feature
  planning

### Revision in one paragraph

When new evidence contradicts an earlier conclusion, set
`isRevision: true` and point `revisesThought` at the step you want to
correct. The chain keeps the original step; the revision is
explicitly anchored to it, so future reasoning can see both the
mistake and the correction.

### Branching in one paragraph

When multiple approaches are viable from a single decision point, set
`branchFromThought` to that point and assign a `branchId` (e.g.
`"caching-approach"`, `"indexing-approach"`). Subsequent thoughts in
the same branch carry the same `branchId`. A separate
"convergence" thought can compare branches and pick a winner.

### Dynamic scope in one paragraph

`totalThoughts` is a free parameter. Start with a rough estimate;
bump it up when the problem turns out to be more complex than
expected; bump it down when you find shortcuts. The estimate guides
pacing, it does not constrain it.

## Tips for effective use

- **Start broad, narrow down** — early thoughts explore the problem
  space, later thoughts dive into specifics
- **Show your work** — document the reasoning process, not just
  conclusions
- **Revise when wrong** — don't continue down an incorrect path;
  backtrack and correct with a revision
- **Branch at crossroads** — when facing clear alternatives,
  explore each systematically in its own branch
- **Adjust dynamically** — change `totalThoughts` as understanding
  evolves; it is a progress indicator, not a budget
- **End decisively** — the final thought should summarize the
  conclusion and any next actions
