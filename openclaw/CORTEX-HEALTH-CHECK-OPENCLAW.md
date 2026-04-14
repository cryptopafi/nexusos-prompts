---
type: procedure
created: 2026-03-17
status: active
slug: cortex-health-check-openclaw
tags: [procedure, nexus]
---

# Cortex Health Check from OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regulă asociată**: OPS-H-001 (Technical Autonomy), MEM-H-001 (Always Sync Sessions to Cortex)
**Scope**: Verificarea disponibilității și funcționalității Cortex (SSH tunnel + API + Qdrant) înainte ca agenții OpenClaw să execute taskuri care necesită knowledge base access.

---

## 1. Problema

Cortex rulează pe VPS (localhost:6400 via SSH tunnel). Dacă tunelul cade sau Cortex nu răspunde:
- Agenții care încearcă să acceseze Cortex primesc erori timeout sau connection refused
- Procedurile nu pot fi retrievate → agenții lucrează fără ghidaj
- Lecțiile nu se pot salva → training loop întrerupt
- Cortex MCP server returnează erori → OpenClaw logs poluate

Fără un health check proactiv, agenții descoperă că Cortex e down abia când au nevoie de el (mid-task, cel mai rău moment).

Situații acoperite:
- Pre-flight check la fiecare startup OpenClaw
- Verificare înainte de taskuri care necesită Cortex intens
- Debugging când `cortex_search` sau `cortex_store` returnează erori
- Monitorizare continuă (Genie supervisor)

---

## 2. Procedura

### Pas 1: Verificare SSH Tunnel (Layer 1)
Verifică că tunelul SSH `localhost:6400 → VPS:6400` este activ:
- Comandă de test: `curl -s --max-time 3 http://localhost:6400/health`
- Dacă returnează HTTP 200 → tunnel OK
- Dacă returnează `Connection refused` sau timeout → tunnel DOWN → mergi la Pas 4

### Pas 2: Verificare Cortex API (Layer 2)
Verifică că API-ul Cortex răspunde corect:
- Endpoint: `http://localhost:6400/health` sau `http://localhost:6400/api/collections`
- Răspuns așteptat: JSON cu status OK + lista de colecții (cel puțin: `procedures`, `shared_knowledge`)
- Dacă colecțiile lipsesc → Qdrant alive dar date pierdute → INCIDENT

### Pas 3: Verificare Qdrant (Layer 3 — dacă API e sus dar search eșuează)
Test rapid de semantic search:
- `cortex_search(query: "test", limit: 1)` via MCP tool
- Dacă returnează rezultat (orice) → Qdrant funcțional
- Dacă returnează eroare de vector search → Qdrant issue independent de API

### Pas 4: Acțiune când Cortex e Down
**Tunnel DOWN** (Pas 1 fail):
- Verifică că LaunchAgent pentru SSH tunnel e activ: `launchctl list | grep tunnel`
- Dacă agent inactiv: pornește manual tunelul
- Dacă agent activ dar tunnel tot down: verifică VPS accesibil (Tailscale conectat?)
- Retry la 30 secunde, maxim 3 încercări

**Cortex API DOWN** (Pas 2 fail, tunnel OK):
- SSH direct pe VPS și verifică procesul Cortex: `systemctl status cortex` sau `pm2 status`
- Dacă crashat: restart serviciu Cortex
- Notifică Pafi prin Telegram dacă API nu pornește în 2 minute

**Qdrant DOWN** (Pas 3 fail, API OK):
- SSH pe VPS și verifică Docker: `docker ps | grep qdrant`
- Dacă container oprit: `docker start qdrant`
- Verifică volumul de date: `docker exec qdrant ls /qdrant/storage/collections/`

### Pas 5: Fallback Mode când Cortex e Unavailable
Dacă Cortex nu poate fi restaurat rapid (>5 minute), agenții continuă în **Cortex-Offline Mode**:
- Agenții folosesc exclusiv MEMORY.md intern (per-agent)
- Nu se salvează lecții noi în Cortex (pierdere acceptată temporar)
- Taskurile critice continuă, cele care cer knowledge base complex se suspendă
- Genie anunță starea prin Telegram + notifică Richard

### Pas 6: Restore și Sync Post-Recovery
Când Cortex revine:
- Verifică integritatea colecțiilor (numărul de entries e consistent cu ultimul backup)
- Dacă există lecții care nu s-au salvat în offline mode: salvează-le acum
- Notifică agenții că Cortex e disponibil din nou
- Log incident în Cortex cu cauza și durata downtime

---

## 3. Cortex Logging

**Health check OK**:
```json
{
  "text": "Cortex health check: PASS. Tunnel OK, API OK, Qdrant OK. Colecții: [count]. Latency: [Xms].",
  "collection": "procedures",
  "metadata": {
    "type": "health_check",
    "procedure": "CORTEX-HEALTH-CHECK-OPENCLAW",
    "rule_id": "OPS-H-001",
    "result": "PASS",
    "tags": ["cortex", "health", "monitoring", "openclaw"]
  }
}
```

**Incident downtime**:
```json
{
  "text": "Cortex downtime incident. Layer afectat: [tunnel|api|qdrant]. Durata: [X] min. Cauza: [Y]. Remediat: DA/NU.",
  "collection": "procedures",
  "metadata": {
    "type": "incident_log",
    "procedure": "CORTEX-HEALTH-CHECK-OPENCLAW",
    "rule_id": "OPS-H-001",
    "result": "INCIDENT",
    "tags": ["cortex", "downtime", "incident", "openclaw"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- OPENCLAW-STARTUP.md Pas 2 (SSH tunnel) → extins cu health check complet
- Genie Supervisor daemon: verificare la fiecare 5-10 minute când OpenClaw e activ
- Orice agent înainte de task care necesită Cortex intens: `cortex_health()` via MCP

### WHEN
- La startup OpenClaw (obligatoriu, via OPENCLAW-STARTUP.md)
- La fiecare 5-10 minute (Genie supervisor, când OpenClaw activ)
- La orice eroare de tip `connection refused` sau `timeout` în MCP Cortex tools

### HOW (violation detection)
- Agent care încearcă să acceseze Cortex fără health check prealabil și primește eroare mid-task → evitabil
- Cortex down > 5 minute fără notificare Telegram → violation MEM-H-001
- Restart Cortex pe VPS fără log incident → violation OPS-H-001 (no audit trail)
- Runner: Genie supervisor LaunchAgent + MCP tool `cortex_health()` built-in în agenți

### CONNECT
- `OPS-H-001` → technical autonomy: Genie rezolvă fără să deranjeze Pafi dacă poate
- `MEM-H-001` → orice downtime înregistrat în Cortex când Cortex revine
- `OPENCLAW-STARTUP.md` → Pas 2 referențiază această procedură
- `CORTEX-MCP-SERVER-DEPLOY.md` → tool `cortex_health()` face health check automat
- `procedure-health.json` → adaugă entry `"CORTEX-HEALTH-CHECK-OPENCLAW": "active"`

### VERIFY
La finalul execuției, verifică:
- [ ] Toți 3 layers (tunnel, API, Qdrant) confirmați OK?
- [ ] Dacă down: acțiunea din Pas 4 executată + Telegram trimis?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] CORTEX-HEALTH-CHECK-OPENCLAW | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Cortex Health Check from OpenClaw" | FORGE ✓ | rule: OPS-H-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Health check routine | Script bash (determinist) | Verificare mecanică, latency importantă |
| Diagnosticare cause downtime | Genie (Sonnet) | SSH + log analysis |
| Incident post-mortem | Opus subagent | Reasoning profund dacă cauza e complexă |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| SSH tunnel | Layer 1 conectivitate | `localhost:6400` |
| Cortex API (VPS) | Layer 2 knowledge base | `http://localhost:6400/health` |
| Qdrant Docker (VPS) | Layer 3 vector DB | `docker ps qdrant` |
| LaunchAgent SSH tunnel | Menținere tunnel persistent | `com.pafi.ssh-cortex-tunnel` (sau similar) |
| `cortex_health()` MCP tool | Health check din agent | via mcp-adapter |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Cortex uptime | % timp Cortex disponibil | > 99.5% |
| Mean Time to Detect (MTTD) | Cât durează să detectăm downtime | < 10 minute (cu supervisor) |
| Mean Time to Restore (MTTR) | Cât durează restore Cortex | < 5 minute (tunnel) / < 15 minute (API/Qdrant) |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, MEM-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [ ] LaunchAgent SSH tunnel configurat cu auto-reconnect
- [ ] Salvat în Cortex cu metadata FORGE
