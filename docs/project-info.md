# Project info

## License

This project is licensed under the MIT License — see [`LICENSE`](../LICENSE)
for the full text. The copyright notice and permission notice must be
preserved in all copies or substantial portions of the software.

```
MIT License

Copyright (c) 2026 eggproject Kft., Hungary

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

## What the MIT License means in plain language

The MIT License grants **every person** who obtains a copy of this
software the right to **use, copy, modify, merge, publish, distribute,
sublicense, and/or sell** copies of it, free of charge. The only
condition is that the copyright notice and permission notice above
must be preserved in all copies or substantial portions of the
software. The software is provided **"as is"** without warranty of
any kind.

## Upstream attribution

The skill content in `skills/sequential-thinking/` is derived from
[`mrgoonie/claudekit-skills`](https://github.com/mrgoonie/claudekit-skills)
— original skill path
[`.claude/skills/sequential-thinking`](https://github.com/mrgoonie/claudekit-skills/tree/main/.claude/skills/sequential-thinking),
licensed MIT.

This repository is a **hand-ported adaptation**, not a verbatim copy:

- The Hermes MCP tool name is rewritten from the upstream Claude
  Code spelling to the Hermes-native
  `mcp_sequential_thinking_sequentialthinking`.
- The layout is moved from `.claude/skills/sequential-thinking/`
  into `skills/sequential-thinking/` so the repository is
  tap-installable.
- The `SKILL.md` frontmatter is tuned for Hermes discovery (`hermes`
  metadata block, MCP tool name, related skills, tags).
- The English and Hungarian documentation in `docs/` and `docs/hu/`
  is original to this repository.

The skill's reasoning patterns (revision, branching, dynamic scope)
are unchanged in spirit from the upstream — they are MIT-licensed
there and remain MIT-licensed here.

## Contributing

Contributions are welcome. The repository follows the standard GitHub
fork-and-pull-request flow:

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/your-change`).
3. Make your changes. Keep `SKILL.md` and `references/*.md` aligned
   with what the skill actually does — the frontmatter `description`
   is the discovery surface, and the references are loaded on demand.
4. Verify the skill still installs via the Hermes tap path.
5. Open a pull request against `main`.

Bug reports and documentation fixes are equally welcome. Open an
issue first if the change is non-trivial, so the approach can be
discussed before code review.

## Versioning

This repository does not currently publish tagged releases. The
upstream `@modelcontextprotocol/server-sequential-thinking` package
and the `mcp/sequentialthinking` Docker image have their own version
cycles — pin them in your deployment if you need reproducible
behavior.

## Acknowledgements

- **mrgoonie / claudekit-skills** — original Sequential Thinking skill
  content, MIT-licensed
- **Anthropic** — Model Context Protocol specification and the
  `@modelcontextprotocol/server-sequential-thinking` reference
  implementation
- **Hermes Agent** — the host this skill is targeted at
- **eggproject Kft.** — porting, documentation, ongoing maintenance

## Maintainer

eggproject Kft., Hungary — <https://github.com/EggProject>
