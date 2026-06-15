# Sequential Thinking — Hermes Agent Skill

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Hermes Agent](https://img.shields.io/badge/Hermes_Agent-Skill-blueviolet)](https://hermes-agent.dev)
[![MCP Server](https://img.shields.io/badge/Backend-MCP_sequential--thinking-36c)](https://modelcontextprotocol.io)
[![Skill Format: SKILL.md](https://img.shields.io/badge/Format-SKILL.md-success)](https://agentskills.io/specification)
[![Hermes Tap](https://img.shields.io/badge/Hermes_Tap-installable-orange)](https://hermes-agent.dev)
[![English](https://img.shields.io/badge/Docs-English-blue)](README.md)
[![Magyar](https://img.shields.io/badge/Docs-Magyar-green)](README.hu.md)

> Egy Hermes Agent skill, amellyel szisztematikus, lépésről lépésre
> haladó problémamegoldást végezhetsz a natív
> `mcp_sequential_thinking_sequentialthinking` eszközzel — beépített
> **revision**, **branching** és **dinamikus scope-állítás**
> lehetőségekkel. Az upstream Sequential Thinking skill Hermes-kompatibilis
> portja, amely a Hermes MCP elnevezési konvencióját és skill hub/tap
> elrendezését követi.

[🇬🇧 **English version here** →](README.md)

---

## 📖 Dokumentáció

A teljes dokumentáció szét van bontva erre a hub-ra és egy `docs/hu/`
mappára. A hub fókuszált marad; a mélyebb részletekre kattints át.

| Oldal | Mi van benne |
|-------|--------------|
| **[📦 Telepítés](docs/hu/installation.md)** | Két telepítési útvonal: Hermes tap, lokális checkout. MCP konfiguráció (NPX, Docker), reload, ellenőrzés. |
| **[💼 Használat](docs/hu/usage.md)** | Paraméterek, workflow minta, egyszerű példa, haladó funkciók (revision, branching, dinamikus scope), tippek. |
| **[📁 A skill struktúrája](docs/hu/skill-format.md)** | Lemezen lévő elrendezés, `SKILL.md` frontmatter, az egyes `references/*.md` szerepe. |
| **[📋 Kompatibilitás](docs/hu/compatibility.md)** | Hermes Agent, MCP szerverek, MCP-aware hosztok. Miért nem fut ez a skill pure chat UI-n. |
| **[ℹ️ Projekt info](docs/hu/project-info.md)** | Licenc (teljes szöveg), upstream attribúció, köszönetnyilvánítás, közreműködés. |

---

## 💡 Mi ez?

A `sequential-thinking` egy [Hermes Agent skill](https://hermes-agent.dev),
amely a Sequential Thinking MCP szervert első osztályú Hermes eszközként
teszi elérhetővé. Olyan összetett problémákhoz való, ahol a gondolatmenet
nem oldható meg egyetlen lépésben — többlépcsős analízis, tervezés,
probléma-dekompozíció, root-cause hibafelderítés, és olyan feladatok,
ahol a kiindulási scope eleinte nem tiszta.

A motorháztető alatt a skill a
`mcp_sequential_thinking_sequentialthinking` eszközt hívja — ez a
kanonikus Hermes MCP tool-név, amelyet a hivatalos Sequential Thinking
MCP szerver `sequential-thinking` stabil szervernéven történő
regisztrálásával kapunk (a kötőjeleket az MCP tool registry aláhúzásjelekké
alakítja).

A repository az upstream skill **kézzel portolt** változata, amely a
[`mrgoonie/claudekit-skills`](https://github.com/mrgoonie/claudekit-skills)
projektben található. **NEM** az Anthropic `skill-creator` eszközével
készült — a struktúra a Hermes skill hub/tap konvenciókhoz van
átírva, a `SKILL.md` frontmatter pedig a Hermes-felderítéshez van
hangolva (tagek, related_skills, MCP tool-név).

## 🎯 Miért jó ez a skill?

| Probléma | Hogyan oldja meg a skill |
|----------|--------------------------|
| "Megoldáshoz ugrom, mielőtt megérteném a problémát." | Kényszeríti a szekvenciális `thought` láncot, explicit `thoughtNumber` / `totalThoughts` ütemezéssel. |
| "Vissza kellene lépnem, de a gondolatmenet nem emlékszik, hol tévedtem." | `isRevision` + `revisesThought` — a javítás az eredeti lépéshez horgonyzott marad. |
| "Több út is járhatónak tűnik — szeretném összehasonlítani, nem vakon elköteleződni." | `branchFromThought` + `branchId` — párhuzamos gondolati ágak bármely döntési pontból. |
| "A kezdeti scope-becslésem mindig rossz." | `totalThoughts` futás közben szabadon állítható — nem szerződés, hanem haladásjelző. |
| "A Sequential Thinking MCP eszközt nehéz megtalálni Hermesben." | Stabil `sequential-thinking` szervernév → kiszámítható `mcp_sequential_thinking_sequentialthinking` tool-név. |
| "Hermes tap-ként akarom telepíteni, nem fájlokat másolgatni." | `skills/sequential-thinking/` elrendezés — egy parancs, és a teljes bundle települ. |

## ✨ Funkciók

- **Hermes-natív MCP tool-név** — `mcp_sequential_thinking_sequentialthinking`, a stabil `sequential-thinking` szervernévből származtatva
- **Két referencia fájl** — `references/advanced.md` (revision és branching minták) és `references/examples.md` (négy valós workflow)
- **Iteratív gondolkodás** — szekvenciális `thought` lépések explicit `thoughtNumber` / `totalThoughts` ütemezéssel
- **Revision követés** — `isRevision` / `revisesThought`, amellyel korábbi következtetések javíthatók a lánc elvesztése nélkül
- **Branch felfedezés** — `branchFromThought` / `branchId` alternatív utak párhuzamos bejárásához
- **Dinamikus scope-állítás** — `totalThoughts` szabadon módosítható a megértés mélyülésével
- **Kézzel portolva Hermes-re** — hangolt SKILL.md frontmatter, tap-kompatibilis elrendezés, MIT licencű upstream

## 🚀 Gyors indulás

1. **Telepítsd a skillt** — lásd [📦 Telepítés](docs/hu/installation.md).
2. **Konfiguráld a Sequential Thinking MCP szervert** — ugyanott.
3. **Ellenőrizd** — ugyanott. Friss Hermes sessionben a tool neve
   `mcp_sequential_thinking_sequentialthinking`.

Ennyi — indítsd a következő összetett problémát a
*"gondold végig lépésről lépésre"* kéréssel, és a skill átveszi.

## 📄 Licenc

Ez a projekt az [MIT Licenc](LICENSE) alatt áll (Copyright © 2026
eggproject Kft., Magyarország). A teljes szöveg és az upstream
attribúció: [ℹ️ Projekt info](docs/hu/project-info.md).

## 🇬🇧 English version (angol nyelv)

The English documentation is here: [README.md](README.md).
