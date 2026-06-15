# A skill struktúrája

A skill bundle a repository-n belül a `skills/sequential-thinking/`
mappában található. Ez a mappa **maga** a telepíthető Hermes skill —
amikor `cp -R` másolod a `~/.hermes/skills/<kategória>/sequential-thinking/`
útvonalra (vagy amikor egy Hermes tap telepíti helyetted), a skill
teljes egészében a helyére kerül.

## Lemezen lévő elrendezés

```text
skills/
└── sequential-thinking/
    ├── SKILL.md
    └── references/
        ├── advanced.md
        └── examples.md
```

| Útvonal | Szerep |
|---------|--------|
| `SKILL.md` | A skill saját rendszer-promptja. A frontmatter deklarálja az MCP tool nevét és a metaadatokat; a törzs megtanítja a Hermesnek, hogy **mikor** és **hogyan** használja a tool-t. |
| `references/advanced.md` | A SKILL kérésére töltődik be — revision és branching minták, dinamikus scope-állítás, session-kezelés, best practices. |
| `references/examples.md` | A SKILL kérésére töltődik be — négy végponttól végpontig tartó workflow, amely megmutatja a tool használatát. |

A `references/` mappa **a telepíthető bundle része**. A skill
frontmatter nem rögzít egy konkrét referencia fájlt — a Hermes a
beszélgetési kontextus alapján igény szerint tölti be. Ne töröld
vagy nevezd át ezeket a fájlokat; a skill feltételezi, hogy
léteznek.

## Miért `skills/<név>/` és nem a repository gyökerében?

A `skills/sequential-thinking/` elhelyezés teszi a repository-t
**tap-telepíthetővé**. A `hermes skills tap add
EggProject/hermes-skill-sequential-thinking` parancs indexeli a
repository-t, és a `skills/sequential-thinking/` tartalmát tekinti a
telepíthető bundle-nek. Ha a SKILL.md a repository gyökerében lenne,
a tap akkor is működne, de a dedikált `skills/` almappa egyértelművé
teszi a bundle határát, és helyet hagy későbbi testvér-skilleknek
ugyanabban a repository-ban.

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

| Mező | Miért fontos |
|------|--------------|
| `name` | Stabil skill-azonosító — a Hermes ezt használja a tool routing-hoz és a felfedezéshez. |
| `description` | A legfontosabb mező a skill-matchinghez. A Hermes ez alapján dönti el, **mikor** töltse be a skillt. Legyél specifikus; a "use when X" megfogalmazás ajánlott. |
| `license` | MIT — megegyezik az upstream és a projekt licenccel. |
| `metadata.source` | Mutató az upstream repository-ra, hogy a származás explicit legyen. |
| `metadata.port` | Dokumentálja a Hermes-specifikus MCP tool-nevet, amelytől a skill függ. |
| `metadata.hermes.tags` | Felfedezési tagek a Hermes skill hub-hoz. |
| `metadata.hermes.related_skills` | Skillek, amelyeket gyakran ezzel együtt használnak. |

## Hogyan tölti be a Hermes a skillt

1. Beérkezik a felhasználói prompt.
2. A Hermes pontozza az egyes telepített skillek `description` mezőjét
   a prompt alapján.
3. Ha `sequential-thinking` illeszkedik, a Hermes betölti a `SKILL.md`-t
   a kontextusba.
4. A skill `SKILL.md` törzse utasítja a Hermest, hogy hívja a
   `mcp_sequential_thinking_sequentialthinking` tool-t, és a mélyebb
   mintákhoz a `references/advanced.md` és `references/examples.md`
   fájlokra hivatkozik.
5. A Hermes az utasításnak megfelelően hívja az MCP tool-t.

Ezért fontos mind a `description`, mind a frontmatter: az első
dönti el, **hogy** a skill egyáltalán meghívódik-e, a második
dönti el, **hogyan** hívódik meg.

## A skill szerkesztése

Ha forkolod a repository-t, és módosítod a skillt:

- Tartsd szinkronban a `name` és `description` mezőket azzal, amit
  a skill valójában csinál — ezek a felfedezési felület.
- Tartsd meg a `references/` fájlokat. Eltávolításuk töri a SKILL.md
  "lásd references/advanced.md" utasításait.
- Tartsd meg az `mcp_sequential_thinking_sequentialthinking` MCP
  tool-nevet minden kódmintában. Megváltoztatásához az MCP szervert
  is át kell nevezni.
