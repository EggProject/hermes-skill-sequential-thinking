# Kompatibilitás

A skill a Hermes Agent-hez és a Sequential Thinking MCP szerverhez
készült. Lent látható, mi támogatott, mi nem, és miért.

## Támogatott

| Felület | Státusz | Megjegyzés |
|---------|---------|------------|
| **Hermes Agent — skill hub / tap** | ✅ First-class | A `skills/sequential-thinking/` elrendezés tap-kompatibilis. |
| **Hermes Agent — skill install** | ✅ | A bundle a `~/.hermes/skills/<kategória>/sequential-thinking/` útvonalra kerül. |
| **Hermes Agent — MCP tool felfedezés** | ✅ | Stabil `sequential-thinking` szervernév → `mcp_sequential_thinking_sequentialthinking` tool-név. |
| **Sequential Thinking MCP szerver (NPX)** | ✅ | `@modelcontextprotocol/server-sequential-thinking` `npx -y ...` módon. |
| **Sequential Thinking MCP szerver (Docker)** | ✅ | `mcp/sequentialthinking` image, `--rm -i`. |
| **Claude Code** | ✅ | A Claude Code MCP-aware. Ugyanaz a `sequential-thinking` szervernév és tool-név működik; a skill `SKILL.md` Hermeshöz szól, de az MCP hívás hordozható. |
| **Bármely MCP-aware hoszt** | ✅ | Bármi, ami regisztrálni tud egy `sequential-thinking` nevű MCP szervert, és meg tudja hívni a `mcp_sequential_thinking_sequentialthinking` tool-t. |

## Nem támogatott

| Felület | Státusz | Miért |
|---------|---------|-------|
| **Anthropic `skill-creator` workflow** | ❌ Nem használt | Ez a skill kézzel portolt, nem generált. Nincs `.claude/skills/skill-creator` függőség a repository-ban. |
| **ChatGPT (web, Plus, Team, Enterprise)** | ❌ | A ChatGPT nem teszi elérhetővé a Sequential Thinking MCP szervert. Nincs pure-prompt út a tool-hoz. |
| **Claude web UI** | ❌ | A web UI-nak nincs MCP kliense. Nem tölthetsz be oda egyedi MCP szervert. |
| **Claude Cowork** | ❌ | A Cowork a standard web UI-t futtatja a motorháztető alatt; ugyanaz a korlátozás. |
| **Gemini (web, app, API)** | ❌ | A Gemini-nek nincs MCP integrációja a consumer felületeken. |
| **Más pure chat AI felületek** | ❌ | Bármi, ami nem tud csatlakozni egy MCP szerverhez, nem tudja futtatni ezt a skillt. |

## Miért nem fut ez a skill pure chat UI-n

A skill egy MCP tool köré épül — nem tartalmazza magát a
sequential-thinking logikát. A tényleges reasoning motor a Sequential
Thinking MCP szerver, amelynek futnia kell valahol, ahol elérhető
(helyben NPX-szel, vagy Docker konténerként).

Nincs pure-prompt alternatíva, mert a Sequential Thinking workflow
**stateful, strukturált hívásokra** épül — a toolnak emlékeznie kell
a gondolatláncra, követnie kell a revision-öket, és kezelnie kell az
ágakat. Ezt nem lehet one-shot prompttal reprodukálni egy chat UI-n.

Ha nem MCP felületen szeretnél sequential thinkinget használni,
másold be a `skills/sequential-thinking/references/examples.md`
tartalmát a chatbe, és dolgozd végig a lépéseket manuálisan. Ez
áll a legközelebb egy "ingyenes szintű recepthez" — de nem maga a
skill, és nem élvezi az MCP tool state-tracking előnyeit.

## MCP szerver név — miért fontos

A skill `SKILL.md` utasítja a Hermest, hogy hívja a
`mcp_sequential_thinking_sequentialthinking` tool-t. Ez a tool-név
**nem konstans** — az MCP szerver nevéből származik:

```text
mcp_{szervernév_aláhúzásjellel}_{tool_név}
```

Ha a Sequential Thinking MCP szervert nem `sequential-thinking` néven
regisztrálod, a tool neve is megváltozik, és a skill utasítása nem
fog egy valódi tool-ra illeszkedni. A Hermes ilyenkor vagy nem találja
a tool-t, vagy rosszat hív.

**A szerver nevének pontosan `sequential-thinking`-nek kell lennie** —
kisbetűs, kötőjeles, alias nélküli.

## Tesztelve ezekkel

- Hermes Agent (skill hub/tap telepítési útvonal)
- `@modelcontextprotocol/server-sequential-thinking` (legújabb npm)
- `mcp/sequentialthinking` Docker image
