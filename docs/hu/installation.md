# Telepítés

A skill kétféleképpen telepíthető. Válassz egyet, majd konfiguráld a
Sequential Thinking MCP szervert, és ellenőrizd a telepítést.

> **Előfeltétel:** A Sequential Thinking MCP szervernek elérhetőnek
> kell lennie onnan, ahol Hermes fut. Ez a skill **megköveteli** az
> MCP-t futásidőben — nincs pure-prompt alternatíva. Az NPX és a
> Docker receptekért lásd lentebb a [Sequential Thinking MCP szerver
> konfigurálása](#a-sequential-thinking-mcp-szerver-konfigurálása)
> szekciót.

## 1. opció — Telepítés Hermes tap-pal

```bash
hermes skills tap add EggProject/hermes-skill-sequential-thinking
hermes skills search sequential-thinking
hermes skills install EggProject/hermes-skill-sequential-thinking/skills/sequential-thinking --category software-development --yes
```

A `tap add` lépés regisztrálja a GitHub repository-t felfedezhető
Hermes skill-forrásként. A `search` megerősíti, hogy indexelve van,
az `install` pedig átmásolja a `skills/sequential-thinking/` mappát
(mindkét `references/*.md` fájllal együtt) a
`~/.hermes/skills/software-development/sequential-thinking/` útvonalra.

## 2. opció — Telepítés helyi checkoutból

```bash
git clone https://github.com/EggProject/hermes-skill-sequential-thinking.git
cd hermes-skill-sequential-thinking
mkdir -p ~/.hermes/skills/software-development
cp -R skills/sequential-thinking ~/.hermes/skills/software-development/sequential-thinking
```

Akkor érdemes így telepíteni, ha a telepítés előtt szerkeszteni
akarsz a skillen, vagy ha a Hermes hosztodon nincs hálózati hozzáférés
a GitHub tap-okhoz.

## A Sequential Thinking MCP szerver konfigurálása

A skill feltételezi, hogy Hermes a Sequential Thinking MCP szervert a
stabil `sequential-thinking` szervernéven éri el. Ezzel a szervernével
a Hermes a tool-t `mcp_sequential_thinking_sequentialthinking` néven
regisztrálja — mivel az MCP tool-nevek a
`mcp_{server_name}_{tool_name}` mintát követik, és a kötőjelek
aláhúzásjelekké alakulnak.

Ha megváltoztatod a szervernevet (pl. `seq-think`), a tool neve is
megváltozik (`mcp_seq_think_sequentialthinking`), és a skill tool
hivatkozásai nem fognak feloldódni. **Tartsd meg a
`sequential-thinking` szervernevet.**

### NPX

```yaml
mcp_servers:
  sequential-thinking:
    command: "npx"
    args: ["-y", "@modelcontextprotocol/server-sequential-thinking"]
```

Ez a hivatalos upstream csomag:
[`@modelcontextprotocol/server-sequential-thinking`](https://www.npmjs.com/package/@modelcontextprotocol/server-sequential-thinking).
Az NPX az első használatkor letölti és futtatja; a későbbi hívások
cache-elve vannak.

### Docker

```yaml
mcp_servers:
  sequential-thinking:
    command: "docker"
    args: ["run", "--rm", "-i", "mcp/sequentialthinking"]
```

Akkor érdemes használni, ha sandbox-olt, image-alapú futtatókörnyezetet
szeretnél, vagy ha az NPX nem elérhető a környezetedben.

## MCP újratöltése

Az MCP konfiguráció megváltoztatása után az új tool-okat a jelenlegi
session nem veszi fel. Töltsd újra a számodra elérhető mechanizmussal:

- `/reload-mcp` slash parancs (ha elérhető)
- Hermes újraindítása
- Friss session indítása

## Ellenőrzés

```bash
hermes mcp list
hermes mcp test sequential-thinking
```

Friss Hermes sessionben a hívható MCP tool neve:

```text
mcp_sequential_thinking_sequentialthinking
```

Ha a `hermes mcp list` nem mutatja a `sequential-thinking` szervert,
ellenőrizd, hogy a szerver neve az MCP konfigurációban **pontosan**
`sequential-thinking` (kisbetűs, kötőjeles, alias nélküli).

## Hibakeresés

| Tünet | Valószínű ok | Megoldás |
|-------|--------------|----------|
| `hermes mcp list` nem mutat `sequential-thinking` szervert | Az MCP konfiguráció nem lett újratöltve | `/reload-mcp` vagy indítsd újra a Hermest |
| A tool neve `mcp_<más>_sequentialthinking` | A szerver nevét megváltoztatták | Nevezd vissza a szervert `sequential-thinking` névre |
| `hermes mcp test sequential-thinking` `npx: command not found` hibát ad | Node / NPX nincs telepítve | Telepítsd a Node 18+ verziót, vagy válts a Docker konfigurációra |
| `hermes mcp test sequential-thinking` `docker: command not found` hibát ad | Docker nincs telepítve | Telepítsd a Dockert, vagy válts az NPX konfigurációra |
| A tool felfedezhető, de minden hívás hibát ad | Az MCP szerver az első futáskor összeomlott | Nézd meg a `hermes mcp logs sequential-thinking` naplót; a hivatalos csomagnak hálózati hozzáférésre van szüksége az első NPX futáskor |
