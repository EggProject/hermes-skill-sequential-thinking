# Használat

A skill egy vékony wrapper a `mcp_sequential_thinking_sequentialthinking`
MCP tool körül. A tool-t egy Hermes sessionben hívod meg, és a skill
`SKILL.md` fájlja utasítja a Hermest, hogy **mikor** és **hogyan**
használja.

## Mikor használd

Használd a `mcp_sequential_thinking_sequentialthinking` tool-t, ha:

- A probléma **több összefüggő gondolkodási lépést** igényel
- A **kiindulási scope vagy megközelítés bizonytalan**, és menet
  közben szeretnéd pontosítani a becslést
- Az **összetettségen kell szűrni**, hogy megtaláld a lényegi
  problémákat — oszd meg és uralkodj
- Előfordulhat, hogy **vissza kell lépnöd vagy felül kell vizsgálnod
  korábbi következtetéseket** új bizonyítékok alapján
- **Párhuzamosan szeretnél alternatív megoldási utakat felfedezni**,
  és kiválasztani a legjobbat

**Ne használd** egyszerű kérdésekre, közvetlen tényekre vagy
egylépéses feladatokra. A szekvenciális lánc overhead-et ad, ami nem
indokolt, ha a válasz egy keresésnyire van.

## Alapvető használat

A Hermes MCP tool `mcp_sequential_thinking_sequentialthinking` a
következő paramétereket fogadja:

### Kötelező paraméterek

| Paraméter | Típus | Cél |
|-----------|-------|-----|
| `thought` | string | Az aktuális gondolkodási lépés — fogalmazd meg önálló gondolatblokkként. |
| `nextThoughtNeeded` | boolean | Több gondolkodásra van-e még szükség. Az utolsó gondolatnál állítsd `false`-ra. |
| `thoughtNumber` | integer | Aktuális lépés száma. `1`-ről indul. |
| `totalThoughts` | integer | Becsült teljes lépésszám. Menet közben állítható. |

### Opcionális paraméterek

| Paraméter | Típus | Cél |
|-----------|-------|-----|
| `isRevision` | boolean | Jelzi, hogy ez a gondolat egy korábbi javítása. |
| `revisesThought` | integer | Melyik `thoughtNumber` felülvizsgálatáról van szó. Kötelező, ha `isRevision: true`. |
| `branchFromThought` | integer | Az a `thoughtNumber`, ahonnan ez az ág indul. |
| `branchId` | string | Azonosító a gondolati ághoz, amelybe ez a gondolat tartozik. |

## Workflow minta

```text
1. Indíts a thoughtNumber: 1, egy kezdeti `totalThoughts` becsléssel,
   és nextThoughtNeeded: true értékkel.
2. Minden lépésnél:
   - Fogalmazd meg az aktuális gondolatot a `thought` mezőben.
   - Frissítsd a `totalThoughts` értékét az új becslésed szerint.
   - Állítsd a `nextThoughtNeeded: true` értéket a folytatáshoz.
3. Amikor elérted a konklúziót, állítsd a `nextThoughtNeeded: false` értéket.
```

A `totalThoughts` becslése **haladásjelző**, nem szerződés — nyugodtan
növelheted vagy csökkentheted. Ami számít, hogy a Hermes lássa
nagyjából, hol tartasz a láncban.

## Egyszerű példa

```typescript
// Első gondolat
{
  thought: "A probléma adatbázis-lekérdezések optimalizálása. Először a szűk keresztmetszeteket kell azonosítani.",
  thoughtNumber: 1,
  totalThoughts: 5,
  nextThoughtNeeded: true
}

// Második gondolat
{
  thought: "A lekérdezési minták elemzése N+1 problémát mutat a user fetch-ekben.",
  thoughtNumber: 2,
  totalThoughts: 6, // Módosított scope
  nextThoughtNeeded: true
}

// ... folytatás a végéig
```

## Haladó funkciók

A `SKILL.md` két kísérő fájlra hivatkozik a skill bundle-ben:

- **[`references/advanced.md`](../../skills/sequential-thinking/references/advanced.md)**
  — revision és branching minták, dinamikus scope-állítás,
  session-kezelés, best practices
- **[`references/examples.md`](../../skills/sequential-thinking/references/examples.md)**
  — négy végponttól végpontig tartó workflow: adatbázis-teljesítmény,
  architektúra-döntés branchinggel, hibafelderítés revisionnal,
  komplex feature-tervezés

### Revision egy bekezdésben

Ha új bizonyíték ellentmond egy korábbi következtetésnek, állítsd az
`isRevision: true` értéket, és a `revisesThought` mezővel mutass
arra a lépésre, amelyet javítani szeretnél. A lánc megőrzi az
eredeti lépést; a revision expliciten hozzá van horgonyozva, így a
jövőbeli gondolkodás egyszerre látja a hibát és a javítást.

### Branching egy bekezdésben

Ha egy döntési pontból több megközelítés is járható, állítsd a
`branchFromThought` mezőt erre a pontra, és adj meg egy `branchId`-t
(pl. `"caching-approach"`, `"indexing-approach"`). Az azonos ágba
tartozó további gondolatok ugyanazt a `branchId`-t viselik. Egy
külön "konvergencia" gondolat összehasonlíthatja az ágakat, és
kiválaszthatja a győztest.

### Dinamikus scope egy bekezdésben

A `totalThoughts` szabad paraméter. Indulj egy durva becsléssel;
növeld, ha a probléma a vártnál összetettebbnek bizonyul; csökkentsd,
ha rövidebb utat találsz. A becslés az ütemezést segíti, nem
korlátozza.

## Tippek a hatékony használathoz

- **Indulj tágan, szűkíts** — a korai gondolatok a problémateret
  fedezik fel, a későbbiek a részletekbe mennek bele
- **Mutasd meg a munkát** — dokumentáld a gondolkodási folyamatot,
  ne csak a konklúziót
- **Revise-olj, ha tévedsz** — ne folytasd a rossz úton; lépj
  vissza, és javítsd egy revisionnal
- **Branchelj kereszteződéseknél** — ha több alternatíva is van,
  fedezd fel mindegyiket szisztematikusan a saját ágában
- **Állítsd dinamikusan** — változtasd a `totalThoughts` értéket a
  megértés mélyülésével; ez haladásjelző, nem keret
- **Zárj határozottan** — az utolsó gondolat foglalja össze a
  konklúziót és a következő lépéseket
