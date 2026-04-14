---
type: procedure
created: 2026-03-17
status: active
slug: agent-health-monitoring
tags: [procedure, nexus]
---

# Agent Health Monitoring — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.1
**Regulă asociată**: OPS-H-001 (Technical Autonomy), SEC-H-006 (Richard Autonomy — Genie as Guardian)
**Scope**: Protocolul de monitorizare continuă a sănătății agenților OpenClaw — detectarea proactivă a problemelor înainte ca Pafi să le observe.

---

## 1. Problema

Fără monitorizare continuă, problemele rămân nedetectate până când:
- Pafi trimite un mesaj și nu primește răspuns (downtime vizibil)
- Un agent produce output degradat timp de ore (silent degradation)
- Portul 18789 e expus public fără să știe nimeni
- SOUL.md unui agent e modificat subtil (drift nedetectat)
- Cortex tunelul cade și agenții salvează în vid

Monitorizarea proactivă → detectare în minute, nu ore.

Situații acoperite:
- Monitorizare continuă în timp ce OpenClaw rulează
- Detectare downtime agent individual
- Detectare comportament aberant (loop, output degradat)
- Detectare modificări neautorizate (SOUL.md, config)

---

## 2. Procedura

### Pas 1: Health Check de Bază (la fiecare 5 minute — LaunchAgent)
Verificare minimă că sistemul e în viață:
- WebSocket RPC: `{"method":"gateway.health","id":1}` pe ws://127.0.0.1:18791
- Așteptat: `{"status":"ok","agents":6}` sau similar
- Dacă timeout sau error → loghează + alertă Telegram după 2 eșecuri consecutive
- Dacă GW1 down confirmat (3 eșecuri) → declanșează OPENCLAW-STARTUP.md automat (dacă configurat) sau alertă Pafi

### Pas 2: Agent Responsiveness Check (la fiecare 5 minute)
Verifică că fiecare agent e în stare activă:
- RPC: `{"method":"sessions.list","id":2}` — verifică că toate 6 sesiuni sunt listed
- Dacă un agent lipsește din lista de sesiuni → alertă specifică (ex: "Finance agent missing from session list")
- Nu trimite mesaj test la fiecare 5 min (prea costisitor) — verifică doar prezența sesiunii

### Pas 3: Security Scan (la fiecare 15 minute — LaunchAgent)
Verificări de securitate:
- Port audit: `lsof -i :18789` → confirmă că e bound doar pe localhost/127.0.0.1
- SOUL.md spot check: hash-ul lui Richard trebuie să fie stabil (comparat cu `~/.nexus/soul-hashes.json`)
- Dacă port 18789 e public → INCIDENT CRITIC → alertă Telegram imediată + execută PORT-18789-LOCALHOST-ENFORCEMENT.md
- Dacă hash Richard modificat → INCIDENT → execută SOUL-MD-IMMUTABILITY.md Pas 5

**Gap cunoscut — agenți non-Richard**: SOUL.md pentru Finance, Marketing, Legal, Ops, Insights este verificat doar de NIGHTLY-AUDIT-OPENCLAW Pas 1 (la 01:00) — potențial gap de până la 24h. Justificare acceptată: Richard e orchestratorul cel mai critic. Dacă se dorește reducerea gap-ului, extinde Pas 3 cu hash check pe toți 6 (cost minor, verificare mai robustă).

### Pas 4: Performance Monitoring (la fiecare oră)
Colectează metrici de performanță:
- **Token usage**: RPC `{"method":"sessions.history","params":{"agentId":"<id>","limit":10}}` → numără `total_tokens` din ultimele 10 sesiuni per agent. Agent cu >2x media echipei → marcat pentru investigare.
- **Response time**: log GW1 la `~/.openclaw/logs/gw1-*.log` → caută linii `[RESPONSE_TIME]`. Dacă formatul nu e parsabil → skip cu notă.
- **Task completion**: RPC `{"method":"sessions.list"}` → sesiuni cu status `abandoned` sau `error` în ultimele 60 minute → count. Dacă > 3 abandonuri de același agent → alertă WARN.
- Salvează metrici în Cortex: `collection="sessions", type="performance_snapshot"`

**Notă**: response quality spot check (evaluare calitate output) = responsabilitatea NIGHTLY-AUDIT Pas 3, rulat de Genie la 09:00 — nu de script bash la fiecare oră.

### Pas 5: Escalation și Alertare
Praguri de alertare:
- **WARN** (Telegram): agent neresponsiv > 2 minute, port check fail o dată
- **CRITICAL** (Telegram + auto-acțiune): GW1 down > 3 minute, port 18789 public, SOUL.md modificat
- **INFO** (Cortex log only): metrici normale, health check OK

Format alertă Telegram:
```
🔴 [OPENCLAW ALERT] <tip>
Agent: <agentId sau "GW1">
Cauza: <descriere scurtă>
Acțiune luată: <ce a făcut Genie>
Timestamp: <HH:MM>
```

### Pas 6: Recovery Automat
Dacă monitoring detectează downtime și recovery e posibil fără intervenție Pafi:
- Richard down → execută RICHARD-RESTORE.md (Genie guardian)
- Agent individual down → execută AGENT-RESTART-STATE-PRESERVATION.md
- Cortex tunnel down → încearcă restart tunnel, notifică Pafi dacă eșuează
- Port expus → kill procesul conflictual (dacă e instanță veche OpenClaw), altfel alertă Pafi

**Circuit breaker — recovery loops**: dacă aceeași procedură de recovery (ex: RICHARD-RESTORE) eșuează de 3 ori consecutiv în < 15 minute → STOP recovery automat, alertă CRITICAL Telegram, așteaptă intervenție Pafi. Nu intra în loop infinit.

---

## 3. Cortex Logging

```json
{
  "text": "Health monitoring snapshot: GW1 [OK/DOWN], agents [6/6 active], port 18789 [OK/EXPOSED], SOUL.md hashes [OK/MODIFIED]. Issues: [none/descriere].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "health_snapshot",
    "procedure": "AGENT-HEALTH-MONITORING",
    "rule_id": "OPS-H-001",
    "tags": ["monitoring", "health", "openclaw", "genie", "supervisor"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- LaunchAgent daemon pe MacIntel (rulează continuu când sistemul e pornit)
- Declanșat manual de Genie la session start (Pas 1-2 din GENIE-SUPERVISOR-SESSION-CHECKLIST)

### WHEN
- Pas 1-2: la fiecare 5 minute (LaunchAgent)
- Pas 3: la fiecare 15 minute (LaunchAgent)
- Pas 4: la fiecare oră (LaunchAgent)
- Pas 5-6: event-driven (când un check eșuează)

### HOW (violation detection)
- LaunchAgent oprit fără a fi repornit → monitorizare lipsă → violation OPS-H-001
- Port 18789 expus și Genie nu alertează → violation SEC-H-002
- SOUL.md modificat nedetectat > 15 minute → violation SEC-H-006
- Runner: LaunchAgent daemon + Genie self-check la session start

### CONNECT
- `OPS-H-001` → autonomie tehnică — Genie monitorizează fără să aștepte Pafi
- `SEC-H-006` → Genie guardian Richard — monitorizarea lui e obligatorie
- `SEC-H-002` → port 18789 NEVER public — verificat activ la Pas 3
- `RICHARD-RESTORE.md` → apelat automat când Richard e down
- `PORT-18789-LOCALHOST-ENFORCEMENT.md` → apelat la Pas 3 dacă port expus
- `SOUL-MD-IMMUTABILITY.md` → apelat la Pas 3 dacă hash modificat
- `NIGHTLY-AUDIT-OPENCLAW.md` → audit mai complet decât monitoring continuu
- `procedure-health.json` → adaugă entry `"AGENT-HEALTH-MONITORING": "active"`

### VERIFY
La verificarea protocolului (audit săptămânal), confirmă:
- [ ] LaunchAgent-urile de monitoring rulează pe MacIntel?
- [ ] Ultimul health check log e < 10 minute vechi?
- [ ] Nicio alertă critică neadresată în ultimele 24h?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → protocol NU e activ

**VK-uri obligatorii**:
1. `✅ [PROC] AGENT-HEALTH-MONITORING | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Agent Health Monitoring" | FORGE ✓ | rule: OPS-H-001 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Health checks automate | Script bash (fără LLM) | Costisitor tokenuri pentru checks simple |
| Diagnosticare issue detectat | Genie (Sonnet) | Decizie de escalare |
| Root cause analiză complexă | Opus subagent | Pattern analysis profund |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| LaunchAgent daemon | Rulare periodică checks | `~/Library/LaunchAgents/com.openclaw.monitor.plist` |
| GW1 control port | Health + sessions | `ws://127.0.0.1:18791` |
| `soul-hashes.json` | Baseline pentru hash check | `~/.nexus/soul-hashes.json` |
| `@Claudeautomationbot` | Alertare Telegram | MacM4 relay |
| Cortex API | Salvare metrici | `http://localhost:6400` via SSH tunnel |
| RICHARD-RESTORE.md | Auto-recovery Richard | `~/.nexus/procedures/openclaw/` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Mean Time to Detect (MTTD) | De la apariția problemei la detectare | < 5 minute |
| False positive rate | Alerte fără problemă reală | < 5% |
| Auto-recovery rate | % probleme rezolvate fără intervenție Pafi | > 70% |
| Health check uptime | % timp cu monitoring activ | > 99% |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-03 | Creat inițial |
| 1.1 | 2026-03-04 | FORGE-AUDIT fix: Pas 4 concretizat cu RPC exact + log path; quality spot check mutat la NIGHTLY-AUDIT; circuit breaker recovery loops; gap SOUL.md non-Richard documentat explicit |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, SEC-H-006)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] LaunchAgent creat și testat (`com.genie.openclaw-monitor.plist` + `com.genie.openclaw-security.plist` cu SOUL.md hash check adăugat)
- [x] Salvat în Cortex cu metadata FORGE
