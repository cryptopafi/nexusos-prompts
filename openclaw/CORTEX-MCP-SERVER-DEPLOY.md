---
type: procedure
created: 2026-03-17
status: active
slug: cortex-mcp-server-deploy
tags: [procedure, nexus]
---

# Cortex MCP Server Build & Deploy — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.2
**Regulă asociată**: DEV-H-009 (Dual Audit Loop for OpenClaw Systems), DEV-H-008 (Check Cortex Before Starting Any Project)
**Scope**: Construirea și deployment-ul serverului MCP custom care expune Cortex API ca native MCP tools pentru agenții OpenClaw.

---

## 1. Problema

Fără un MCP server Cortex, agenții OpenClaw pot accesa Cortex doar prin:
- Skill.md cu curl manual (fragil, verbose, error-prone)
- Direct HTTP (necesită agenții să cunoască URL-urile și payload-urile exact)

Un MCP server propriu înseamnă că fiecare agent apelează `cortex_search("...")` nativ, ca orice tool built-in. Deblocheaza accesul la toate cele 297 proceduri FORGE'd existente + toată memoria Cortex.

**Aceasta este procedura cu cel mai mare ROI din toate 110**: 3-4 ore de build deblochează accesul la ani de knowledge acumulat.

**Situații concrete de failure fără MCP server:**
- Richard primeste un task financiar → nu poate cauta `cortex_procedures_get("RICHARD-RESTORE")` → pierde context critic de restaurare
- Agent pornit fara MCP: orice apel `cortex_search(...)` returnează `tool not found` → agentul improvizează în loc sa citeasca procedura corecta
- Upgrade agent (nou SOUL.md) fara MCP activ → LEARNINGS.md din Cortex nu e accesibil → pierdere de memorie de sesiune

Situații acoperite:
- Build inițial al serverului MCP Cortex
- Upgrade/actualizare versiune server
- Adăugare tool nou (din 10 planificate, poate fi iterativ)
- Debugging al serverului MCP

---

## 2. Procedura

### Pas 1: Verificare Prerequisite
Înainte de build, rulează fiecare comandă și confirmă output-ul așteptat:

```bash
# 1a. Verificare SSH tunnel activ și Cortex API accesibil
curl -s http://localhost:6400/health | jq '.status'
# Output așteptat: "ok" — dacă nu → rulează OPENCLAW-STARTUP.md Pas 2 înainte de a continua

# 1b. Verificare Node.js (minim v18) SAU Bun (minim v1.0)
node --version   # trebuie >= v18.0.0
# SAU
bun --version    # trebuie >= 1.0.0
# Dacă niciunul nu e disponibil → instaleaza: brew install node (sau curl -fsSL https://bun.sh/install | bash)

# 1c. Verificare mcp-adapter plugin în OpenClaw
openclaw plugins list | grep mcp-adapter
# Output așteptat: "mcp-adapter  enabled" — dacă lipseste → STOP, contactează Pafi pentru instalare plugin
```

**Dacă oricare verificare eșuează: NU continua la Pas 2.** Rezolvă blocajul și re-porneste de la Pas 1.

### Pas 2: Definește Cele 10 Tools MCP
Serverul MCP expune aceste tool-uri (prioritate de implementare):
1. `cortex_search(query, collection?, limit?)` — semantic search în Cortex
2. `cortex_store(text, collection, metadata)` — salvare entry nou
3. `cortex_procedures_list(tags?, type?)` — listare proceduri disponibile
4. `cortex_procedures_get(id)` — preia o procedură specifică
5. `cortex_error_match(error_text)` — caută fix-uri pentru erori cunoscute
6. `cortex_feedback(id, success, notes)` — feedback pe un item (îmbunătățește retrieval)
7. `cortex_sessions_log(session_data)` — loghează sesiune agent în Cortex
8. `cortex_rules_get(rule_id?)` — preia reguli (READ ONLY)
9. `cortex_health()` — health check Cortex + Qdrant
10. `cortex_agent_learnings(agent_id)` — preia LEARNINGS.md generat din Cortex

### Pas 3: Build Serverul MCP

**3a. Creare structura de proiect:**
```bash
# Creare director și inițializare proiect
mkdir -p ~/repos/cortex-mcp-server
cd ~/repos/cortex-mcp-server
git init

# Daca re-deploy (upgrade): pastreaza backup
[ -d ~/repos/cortex-mcp-server.bak ] && rm -rf ~/repos/cortex-mcp-server.bak
cp -r ~/repos/cortex-mcp-server ~/repos/cortex-mcp-server.bak

# Inițializare package
npm init -y
# SAU: bun init -y
```

**3b. Structura minima obligatorie de fisiere:**
```
~/repos/cortex-mcp-server/
├── index.js          ← entry point MCP server (stdio transport)
├── tools/
│   ├── cortex_search.js
│   ├── cortex_store.js
│   └── ... (celelalte 8 tools)
├── package.json
├── .env.example      ← template keys, NU .env cu valori reale
└── README.md
```

**3c. Specificatii tehnice pentru Codex / implementare:**
- Transport: stdio (JSON-RPC 2.0 standard MCP)
- Auth: `CORTEX_API_KEY` din env var — NU hardcodat în cod
- `CORTEX_BASE_URL`: default `http://localhost:6400`
- Fiecare tool = fisier separat în `tools/` pentru testabilitate
- Error handling: orice eroare Cortex API → returnează MCP error response cu mesaj clar, NU crash server

**3d. Instalare dependente si primul build:**
```bash
npm install @modelcontextprotocol/sdk node-fetch dotenv
# SAU: bun add @modelcontextprotocol/sdk node-fetch dotenv

# Test ca serverul porneste fara erori
CORTEX_API_KEY=test node index.js &
sleep 2 && kill %1
# Output asteptat: "Cortex MCP Server ready on stdio" (fara stack trace)
```

**3e. Versioning obligatoriu:**
```bash
git add -A && git commit -m "v1.0.0: initial cortex-mcp-server build"
git tag v1.0.0
```

**Error recovery Pas 3:**
- Build fail (compile/dependency error): loghează eroarea cu `npm install 2>&1 | tee ~/repos/cortex-mcp-server-build.log`, nu continua la Pas 4. Revizuiește cu Codex dacă eroarea nu e evidentă.
- Runtime test fail (serverul pornit dar tools nu răspund): `cp -r ~/repos/cortex-mcp-server.bak ~/repos/cortex-mcp-server` și re-testează

### Pas 4: Configurare mcp-adapter în openclaw.json
Config-ul OpenClaw se afla la: `~/.openclaw/openclaw.json`

Adaugă blocul următor în secțiunea `plugins.mcp-adapter.servers`:
```json
{
  "plugins": {
    "mcp-adapter": {
      "servers": {
        "cortex": {
          "transport": "stdio",
          "command": "node",
          "args": ["/Users/pafi/repos/cortex-mcp-server/index.js"],
          "env": {
            "CORTEX_API_KEY": "${CORTEX_API_KEY}",
            "CORTEX_BASE_URL": "http://localhost:6400"
          }
        }
      }
    }
  }
}
```

**Verificare config salvat corect:**
```bash
cat ~/.openclaw/openclaw.json | jq '.plugins["mcp-adapter"].servers.cortex'
# Output asteptat: obiectul JSON de mai sus (nu null, nu eroare de parse)
```

**Nota**: Dacă folosești Bun în loc de Node.js, schimbă `"command": "node"` cu `"command": "bun"`.

### Pas 5: Aplicare CORTEX-WRITE-PERMISSIONS.md
Înainte de a da acces agenților la MCP:
- Aplică matricea de permisiuni din CORTEX-WRITE-PERMISSIONS.md
- Fiecare agent va folosi un token cu scope limitat
- Verifică că `rules` collection e READ ONLY pentru toți agenții

### Pas 6: Actualizare SOUL.md per Agent (referință tools)
Adaugă în SOUL.md-ul fiecărui agent secțiunea de Cortex tools disponibile:
- Ce tools poate folosi (conform matricei de permisiuni)
- Când să le folosească (ex. "înainte de orice task nou, caută proceduri relevante")
- Cum să interpreteze rezultatele

### Pas 7: Test End-to-End

**7a. Verificare tools raspund (smoke test via curl pe MCP stdio):**
```bash
# Test health tool direct
echo '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"cortex_health","arguments":{}},"id":1}' \
  | CORTEX_API_KEY=$CORTEX_API_KEY node ~/repos/cortex-mcp-server/index.js
# Output asteptat: {"result":{"status":"ok","qdrant":"connected"},...}

# Test search (cu un agent real sau direct)
echo '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"cortex_search","arguments":{"query":"financial reporting"}},"id":2}' \
  | CORTEX_API_KEY=$CORTEX_API_KEY node ~/repos/cortex-mcp-server/index.js
# Output asteptat: {"result":{"hits":[...]},...} — cel putin 1 rezultat
```

**7b. Test permisiuni (security critical):**
```bash
# Test write in rules collection — trebuie REFUZAT
echo '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"cortex_store","arguments":{"text":"test","collection":"rules","metadata":{}}},"id":3}' \
  | CORTEX_API_KEY=$CORTEX_API_KEY_AGENT_RICHARD node ~/repos/cortex-mcp-server/index.js
# Output asteptat: {"error":{"code":-32603,"message":"Permission denied: rules collection is READ ONLY"},...}
```

**7c. Test cu agenti reali în OpenClaw (dupa restart GW1):**
```bash
openclaw restart gw1
# In chat Richard: "Cauta procedura RICHARD-RESTORE in Cortex"
# Asteptat: Richard apeleaza cortex_procedures_get si returneaza continutul procedurii
```

**E2E fail → rollback**:
```bash
# Rollback config
cp ~/.openclaw/openclaw.json.bak ~/.openclaw/openclaw.json
openclaw restart gw1
# Verifica ca agentii nu mai vad tools MCP Cortex
openclaw status | grep cortex
```
- Investigheaza eroarea și re-rulează Pas 3-7 după fix

---

## 3. Cortex Logging

```json
{
  "text": "Cortex MCP Server deployed. 10 tools active. Transport: stdio. Permisiuni: matricea CORTEX-WRITE-PERMISSIONS aplicată. Test E2E: PASS.",
  "collection": "procedures",
  "metadata": {
    "type": "deployment_log",
    "procedure": "CORTEX-MCP-SERVER-DEPLOY",
    "rule_id": "DEV-H-009",
    "tags": ["cortex", "mcp", "deployment", "openclaw", "integration"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Blocant pentru: CORTEX-PROCEDURE-RETRIEVAL.md și orice altă procedură care necesită Cortex din agenți
- Pre-requisite check la OPENCLAW-STARTUP.md (Pas 2: SSH tunnel → implicit MCP server alive?)
- DEV-H-009 Dual Audit Loop: Genie audit înainte + după deployment

### WHEN
- O singură dată la setup inițial integrare Cortex-OpenClaw
- La orice upgrade al serverului MCP
- La adăugare de tool nou

### HOW (violation detection)
Detectie activa prin comenzi:
```bash
# Detectie: OpenClaw activ fara MCP Cortex conectat
openclaw status | grep -E "cortex|mcp-adapter"
# Daca output e gol sau "disconnected" → sub-optim, notifica Pafi

# Detectie: write neautorizat in rules collection (din logs)
grep -i "cortex_store.*rules" ~/.openclaw/logs/mcp-adapter.log | grep -v "Permission denied"
# Orice linie fara "Permission denied" = VIOLATION CRITICA

# Detectie: MCP server deploy fara audit DEV-H-009
ls ~/.openclaw/audit-logs/ | grep "DEV-H-009"
# Daca lipseste entry pentru data de azi si serverul e activ → violation DEV-H-009
```
- Runner: DEV-H-009 Dual Audit Loop + test E2E la fiecare deploy

### CONNECT
- `DEV-H-009` → audit obligatoriu pentru sisteme OpenClaw
- `CORTEX-WRITE-PERMISSIONS.md` → aplicat la Pasul 5 (prerequisite)
- `CORTEX-PROCEDURE-RETRIEVAL.md` → depinde de această procedură
- `CORTEX-HEALTH-CHECK-OPENCLAW.md` → verificare că serverul MCP e alive
- `procedure-health.json` → adaugă entry `"CORTEX-MCP-SERVER-DEPLOY": "active"`

### VERIFY
La finalul execuției, verifică prin comenzi concrete:
```bash
# V1: Toate 10 tools sunt înregistrate în MCP server
echo '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":0}' \
  | CORTEX_API_KEY=$CORTEX_API_KEY node ~/repos/cortex-mcp-server/index.js \
  | jq '.result.tools | length'
# Asteptat: 10

# V2: cortex_health raspunde OK
echo '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"cortex_health","arguments":{}},"id":1}' \
  | CORTEX_API_KEY=$CORTEX_API_KEY node ~/repos/cortex-mcp-server/index.js \
  | jq '.result.status'
# Asteptat: "ok"

# V3: Permission rejection activa
echo '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"cortex_store","arguments":{"text":"t","collection":"rules","metadata":{}}},"id":2}' \
  | CORTEX_API_KEY=$CORTEX_API_KEY node ~/repos/cortex-mcp-server/index.js \
  | jq '.error.message'
# Asteptat: mesaj cu "Permission denied" sau "READ ONLY"

# V4: procedure-health.json actualizat
cat ~/.nexus/procedures/openclaw/procedure-health.json | jq '.procedures["CORTEX-MCP-SERVER-DEPLOY"].status'
# Asteptat: "active"
```
- [ ] V1: 10 tools inregistrate
- [ ] V2: cortex_health = "ok"
- [ ] V3: permission rejection confirmat
- [ ] V4: procedure-health.json actualizat
- [ ] VK emis în sesiune
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] CORTEX-MCP-SERVER-DEPLOY | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Cortex MCP Server Deploy" | FORGE ✓ | rule: DEV-H-009 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Build server MCP | Codex (gpt-5.3-codex) | Cod 200+ linii, integrare API |
| Audit pre-deploy | Opus subagent | DEV-H-009 Dual Audit Loop |
| Test E2E | Genie (Sonnet) | Orchestrare teste |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint exact |
|-----------|-----|---------------------|
| Cortex API (VPS) | Backend pe care MCP îl expune | `http://localhost:6400` (SSH tunnel → VPS 89.116.229.189) |
| mcp-adapter plugin | Plugin OpenClaw care conectează MCP | `~/.openclaw/plugins/mcp-adapter/` — verificat cu `openclaw plugins list` |
| openclaw.json | Config principal OpenClaw | `~/.openclaw/openclaw.json` — secțiunea `plugins.mcp-adapter.servers.cortex` |
| Node.js | Runtime server MCP (primar) | `/usr/local/bin/node` sau `/opt/homebrew/bin/node` — minim v18.0.0 |
| Bun | Runtime server MCP (alternativ) | `~/.bun/bin/bun` — minim v1.0.0 |
| cortex-mcp-server | Codul serverului MCP | `~/repos/cortex-mcp-server/index.js` |
| cortex-mcp-server.bak | Fallback la rollback | `~/repos/cortex-mcp-server.bak/` |
| `CORTEX-WRITE-PERMISSIONS.md` | Matrice permisiuni de aplicat la Pasul 5 | `~/.nexus/procedures/openclaw/CORTEX-WRITE-PERMISSIONS.md` |
| SSH tunnel LaunchAgent | Mentine conectivitatea VPS | `~/Library/LaunchAgents/com.pafi.ssh-tunnel.plist` |
| procedure-health.json | Registry stare proceduri | `~/.nexus/procedures/openclaw/procedure-health.json` |
| CORTEX_API_KEY | Auth pentru Cortex API | Env var — verifică `~/.nexus/memory/env-template.md` pentru naming |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Cum se măsoară |
|---------|-----------|--------|----------------|
| MCP tool latency | Timp răspuns `cortex_search` | < 300ms | `time echo '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"cortex_search","arguments":{"query":"test"}},"id":1}' \| node ~/repos/cortex-mcp-server/index.js` — real time < 0.3s |
| Tool success rate | % apeluri MCP fără eroare | > 99% | `grep -c "error" ~/.openclaw/logs/mcp-adapter.log` / total apeluri din log — verificat zilnic |
| Permission rejection rate | % apeluri blocate corect | 100% pentru cazurile interzise | Test manual Pas 7b la fiecare deploy nou + verificare log `grep "Permission denied" ~/.openclaw/logs/mcp-adapter.log` |
| Tools count | Nr. tools înregistrate în MCP | = 10 | `echo '{"jsonrpc":"2.0","method":"tools/list","params":{},"id":0}' \| node ~/repos/cortex-mcp-server/index.js \| jq '.result.tools \| length'` |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (DEV-H-009, DEV-H-008)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu comenzi concrete
- [x] Situații concrete de failure documentate (D1)
- [x] Pași executabili cu comenzi exacte (D2)
- [x] Violation detection prin comenzi (D3/HOW)
- [x] Metrics cu comenzi de măsurare (D4)
- [x] Dependencies cu path-uri absolute și versiuni minime (D5)
- [x] VK format specificat
- [x] Changelog prezent
- [x] Entry actualizat în `procedure-health.json` (`~/.nexus/procedures/openclaw/procedure-health.json`)
- [ ] Salvat în Cortex cu metadata FORGE (post-deploy)

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-03 | Creare initials |
| 1.1 | 2026-03-03 | Adaugare error recovery Pas 3, rollback E2E, model routing table |
| 1.2 | 2026-03-04 | FORGE-AUDIT fix: situatii concrete failure (D1), comenzi executabile Pas 1/3/4/7 (D2), violation detection comenzi (D3/HOW), VERIFY cu comenzi concrete (D3/VERIFY), metrics cu masurare concreta (D4), dependencies cu path-uri absolute si versiuni minime (D5), changelog + checklist complet (D6) |
