# Projekt info

## Licenc

Ez a projekt az MIT Licenc alatt áll — a teljes szöveget lásd a
[`LICENSE`](../../LICENSE) fájlban. A copyright értesítést és az
engedélyt meg kell őrizni minden másolatban vagy a szoftver
jelentős részében.

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

## Mit jelent az MIT Licenc egyszerű nyelven

Az MIT Licenc **minden személynek**, aki a szoftver egy példányát
megszerzi, megadja a jogot, hogy **ingyenesen** használja, másolja,
módosítsa, egyesítse, publikálja, terjessze, allicencbe adja és/vagy
eladja a szoftver másolatait. Az egyetlen feltétel, hogy a fenti
copyright értesítést és engedélyt meg kell őrizni minden másolatban
vagy a szoftver jelentős részében. A szoftvert **"ahogy van"**
biztosítjuk, bármilyen garancia nélkül.

## Upstream attribúció

A `skills/sequential-thinking/` mappa tartalma a
[`mrgoonie/claudekit-skills`](https://github.com/mrgoonie/claudekit-skills)
projektből származik — eredeti skill útvonal:
[`.claude/skills/sequential-thinking`](https://github.com/mrgoonie/claudekit-skills/tree/main/.claude/skills/sequential-thinking),
MIT licenccel.

Ez a repository egy **kézzel portolt adaptáció**, nem szó szerinti
másolat:

- A Hermes MCP tool-neve át van írva az upstream Claude Code
  írásmódból a Hermes-natív
  `mcp_sequential_thinking_sequentialthinking` névre.
- Az elrendezés a `.claude/skills/sequential-thinking/` útvonalról
  a `skills/sequential-thinking/` útvonalra került, hogy a
  repository tap-telepíthető legyen.
- A `SKILL.md` frontmatter a Hermes-felderítéshez van hangolva
  (`hermes` metaadat-blokk, MCP tool-név, related_skills, tagek).
- Az angol és magyar dokumentáció a `docs/` és `docs/hu/` mappákban
  ezen repository eredeti munkája.

A skill reasoning mintái (revision, branching, dinamikus scope)
lelkükben változatlanok az upstreamhez képest — ott MIT licenccel
állnak, és itt is MIT licenc alatt maradnak.

## Közreműködés

A közreműködés szívesen látott. A repository a standard GitHub
fork-and-pull-request flow-t követi:

1. Forkold a repository-t.
2. Hozz létre egy feature branchet (`git checkout -b feature/a-te-valtoztatasod`).
3. Vidd végig a változtatásokat. Tartsd a `SKILL.md` és a
   `references/*.md` fájlokat összhangban azzal, amit a skill
   valójában csinál — a frontmatter `description` a felfedezési
   felület, a referenciák pedig igény szerint töltődnek be.
4. Ellenőrizd, hogy a skill továbbra is telepíthető a Hermes tap
   útvonalon.
5. Nyiss pull requestet a `main` ellen.

A hibajelentések és a dokumentáció-javítások ugyanúgy szívesen
vannak. Előbb nyiss egy issue-t, ha a változtatás nem triviális,
hogy a megközelítést meg lehessen beszélni a kód-review előtt.

## Verziókezelés

Ez a repository jelenleg nem publikál tag-elt release-eket. Az
upstream `@modelcontextprotocol/server-sequential-thinking` csomag
és a `mcp/sequentialthinking` Docker image saját verzióciklussal
rendelkezik — ha reprodukálható viselkedésre van szükséged, pin-eld
őket a deploymentedben.

## Köszönetnyilvánítás

- **mrgoonie / claudekit-skills** — az eredeti Sequential Thinking
  skill tartalom, MIT licenccel
- **Anthropic** — a Model Context Protocol specifikáció és az
  `@modelcontextprotocol/server-sequential-thinking` referencia
  implementáció
- **Hermes Agent** — a hoszt, amelyhez a skill szól
- **eggproject Kft.** — portolás, dokumentáció, folyamatos
  karbantartás

## Maintainer

eggproject Kft., Magyarország — <https://github.com/EggProject>
