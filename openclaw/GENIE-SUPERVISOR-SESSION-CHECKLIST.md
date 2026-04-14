---
type: procedure
created: 2026-03-17
status: active
slug: genie-supervisor-session-checklist
tags: [procedure, nexus]
---

# Genie Supervisor — Session Start Checklist OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.2
**Regulă asociată**: SEC-H-006 (Richard Autonomy — Genie as Guardian), COM-H-001 (Session Start Checklist Mandatory)
**Scope**: Ce verifică Genie la fiecare sesiune nouă când OpenClaw este activ — starea sistemului înainte de a prelua taskuri de la Pafi.

---

## 1. Problema

Fără un checklist de sesiune, Genie poate prelua taskuri în timp ce:
- Richard e down (niciun orchestrator activ)
- GW1/GW2 sunt căzute (agenții nu răspund)
- Cortex tunelul e rupt (knowledge base inaccesibil)
- Nightly audit-ul nu a rulat (probleme nedetectate peste noapte)
- Un agent e în stare degradată (răspunde dar produce output aberant)

SEC-H-006 impune ca Genie să fie Guardian activ — nu reactiv.

Situații acoperite:
- Sesiune Genie dimineața (după noapte cu OpenClaw activ)
- Sesiune Genie după o pauză > 2 ore
- Sesiune Genie după orice restart de sistem

---

## 2. Procedura

### Pas 1: Verificare Rapidă GW1 + GW2
Confirmă că ambele gateway-uri rulează:
- GW1: WebSocket RPC `{"method":"gateway.health","id":1}` pe ws://127.0.0.1:18791 — așteptat: status OK, 6 agenți listed
- GW2: verificare port 19001 activ (`lsof -i :19001` sau RPC pe ws://127.0.0.1:19003)
- Dacă GW1 down → execută OPENCLAW-STARTUP.md înainte de orice altceva
- Dacă GW2 down dar GW1 OK → loghează warning (watchdog lipsă), continuă sesiunea, notifică Pafi

**Continue-on-error**: dacă Pas 1 parțial eșuează (GW2 down dar GW1 OK), continuă cu pașii următori — nu bloca sesiunea complet. Raportează statusul parțial la Pas 6.

### Pas 2: Ping Richard + Verificare Prezență Agenți
Verifică că orchestratorul și toți agenții sunt prezenți:
- Ping Richard: RPC `{"method":"agent.send","params":{"agentId":"main","message":"session check"}}` — timeout 30 secunde
- Verificare prezență toți 6: RPC `{"method":"sessions.list"}` — trebuie să apară: `main`, `finance`, `marketing`, `legal`, `ops`, `insights`
- Dacă Richard nu răspunde → execută RICHARD-RESTORE.md imediat (înainte de Pas 3-6)
- Dacă un agent specialist lipsește din sessions.list → loghează warning în `~/.openclaw/session-warnings.log` (format: `[WARN][<timestamp>] agent <name> missing from sessions.list`) + menționează la Pas 6 (nu bloca sesiunea)

**Continue-on-error**: agent specialist lipsă = warning, nu blocker. Richard lipsă = blocker — se execută RICHARD-RESTORE înainte de continuare.

### Pas 3: Verificare Cortex Tunnel
Confirmă că knowledge base-ul e accesibil:
- `curl -s --max-time 3 http://localhost:6400/api/health` — răspuns așteptat: HTTP 200 cu body `{"status":"ok"}`
- Dacă down → execută `~/.nexus/procedures/openclaw/CORTEX-HEALTH-CHECK-OPENCLAW.md`
- Dacă down și nu se poate repara rapid → continuă sesiunea în fallback mode: anunță Pafi explicit că knowledge base e inaccesibil și că search în Cortex nu e disponibil

### Pas 4: Verificare Nightly Audit
Confirmă că auditul de noapte a rulat:
- Verifică timestamp: `stat -f "%Sm" ~/.openclaw/nightly-audit-last.log` (macOS) — trebuie să fie < 24 ore față de ora curentă
- Alternativ: `head -1 ~/.openclaw/nightly-audit-last.log` — prima linie conține timestamp ISO 8601
- Dacă fișierul lipsește sau timestamp > 24 ore → menționează în briefing (audit ratat)
- Dacă există findings critice în raport → caută secțiunea `[CRITICAL]` sau `severity: critical` — escalat imediat la Pafi înainte de Pas 5-6

### Pas 5: Verificare SOUL.md Integritate (spot check)
Rapid hash check pe Richard (cel mai critic):
- Calculează hash curent: `sha256sum ~/.openclaw/workspaces/rich-new/SOUL.md | awk '{print $1}'`
- Compară cu valoarea stocată: `cat ~/.nexus/soul-hashes.json | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('richard','NOT_FOUND'))"`
- Dacă diferă → INCIDENT — execută `~/.nexus/procedures/openclaw/SOUL-MD-IMMUTABILITY.md` Pas 5 (incident response)
- Dacă `soul-hashes.json` nu există → creează-l cu: `echo '{"richard":"'$(sha256sum ~/.openclaw/workspaces/rich-new/SOUL.md | awk '{print $1}')'"}'  > ~/.nexus/soul-hashes.json`

### Pas 6: Briefing Pafi
Raportează starea sistemului:
- Dacă totul OK: scurt (1 linie) — "Sistem OpenClaw: OK. Richard activ, 6 agenți online, Cortex OK."
- Dacă există issues: folosește formatul:
  ```
  [ISSUE] <componentă>: <status>
  [ACȚIUNE] <ce s-a executat>: <rezultat>
  [IMPACT] <ce nu funcționează până la remediere>
  ```
  Listează issues în ordine de severitate: INCIDENT > blocker > warning.
- Include orice findings `[CRITICAL]` din nightly audit relevante pentru sesiunea curentă
- Menționează explicit agenții lipsă din sessions.list (dacă există)

---

## 3. Cortex Logging

```json
{
  "text": "Genie session checklist completat. GW1: [OK/DOWN]. Richard: [OK/DOWN]. Cortex: [OK/DOWN]. Nightly audit: [OK/RATAT]. SOUL.md hash: [OK/INCIDENT].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "session_checklist",
    "procedure": "GENIE-SUPERVISOR-SESSION-CHECKLIST",
    "rule_id": "SEC-H-006",
    "tags": ["genie", "supervisor", "session", "checklist", "openclaw"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- La FIECARE sesiune Genie nouă când OpenClaw e menționat sau relevant
- Automat ca Step 6 din Session Start Checklist din CLAUDE.md (dacă OpenClaw activ)

### WHEN
- Dimineața la prima sesiune a zilei
- După orice pauză > 2 ore cu OpenClaw activ
- După orice restart de sistem (MacIntel reboot sau crash recovery)

### HOW (violation detection)
- Genie preia taskuri de la Pafi fără să verifice Richard → violation SEC-H-006
- Richard down neverificat pentru > 30 minute → violation COM-H-005
- Pas 4 sărit (nightly audit neconfirmat) → issues de noapte rămân nedetectate
- Checklist omis complet → detectat de nightly audit prin absența entry-ului din `~/.openclaw/checklist-stats.json` pentru ziua respectivă → Pafi alertat în raportul de noapte următor
- Runner: Genie self-check la fiecare session start (COM-H-001); nightly audit verifică completeness retrospectiv

### CONNECT
- `SEC-H-006` → Genie = Guardian Richard, verificarea lui e obligatorie
- `COM-H-001` → Session Start Checklist mandatory
- `RICHARD-RESTORE.md` → apelat dacă Pas 2 eșuează
- `OPENCLAW-STARTUP.md` → apelat dacă Pas 1 eșuează
- `CORTEX-HEALTH-CHECK-OPENCLAW.md` → apelat dacă Pas 3 eșuează
- `SOUL-MD-IMMUTABILITY.md` → apelat dacă Pas 5 detectează modificare
- `NIGHTLY-AUDIT-OPENCLAW.md` → produce raportul verificat la Pas 4
- `procedure-health.json` → adaugă entry `"GENIE-SUPERVISOR-SESSION-CHECKLIST": "active"`

### VERIFY
La finalul checklist-ului, confirmă:
- [ ] GW1 status verificat?
- [ ] Richard a răspuns sau RICHARD-RESTORE executat?
- [ ] Cortex tunnel confirmat sau fallback mode anunțat?
- [ ] Nightly audit timestamp verificat?
- [ ] SOUL.md hash Richard verificat (match sau INCIDENT declanșat)?
- [ ] Pafi briefat cu starea sistemului?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → checklist NU e complet

**VK-uri obligatorii**:
1. `✅ [PROC] GENIE-SUPERVISOR-SESSION-CHECKLIST | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Genie Supervisor Session Checklist" | FORGE ✓ | rule: SEC-H-006 | v1.2`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție checklist | Genie (Sonnet) | Orchestrare pași simpli, verificări rapide |
| Investigare issue detectat | Opus subagent | Diagnosticare problemă complexă |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| GW1 control port | Health check gateway principal | `ws://127.0.0.1:18791` |
| GW2 watchdog port | Health check watchdog | port 19001 / `ws://127.0.0.1:19003` |
| `openclaw` CLI | Health check alternativ GW1 | în PATH pe MacIntel |
| Cortex API | Knowledge base health check | `http://localhost:6400/api/health` |
| `nightly-audit-last.log` | Confirmare audit noapte | `~/.openclaw/nightly-audit-last.log` |
| `soul-hashes.json` | Baseline SOUL.md hashes | `~/.nexus/soul-hashes.json` |
| `SOUL.md` Richard | Fișier supus integritate check | `~/.openclaw/workspaces/rich-new/SOUL.md` |
| `checklist-stats.json` | Stocare metrics completion rate | `~/.openclaw/checklist-stats.json` |
| RICHARD-RESTORE.md | Recovery Richard (Pas 2 fail) | `~/.nexus/procedures/openclaw/RICHARD-RESTORE.md` |
| OPENCLAW-STARTUP.md | Startup complet (Pas 1 GW1 fail) | `~/.nexus/procedures/openclaw/OPENCLAW-STARTUP.md` |
| CORTEX-HEALTH-CHECK-OPENCLAW.md | Recovery Cortex (Pas 3 fail) | `~/.nexus/procedures/openclaw/CORTEX-HEALTH-CHECK-OPENCLAW.md` |
| SOUL-MD-IMMUTABILITY.md | Incident response SOUL (Pas 5 fail) | `~/.nexus/procedures/openclaw/SOUL-MD-IMMUTABILITY.md` |
| NIGHTLY-AUDIT-OPENCLAW.md | Produce raportul verificat la Pas 4 | `~/.nexus/procedures/openclaw/NIGHTLY-AUDIT-OPENCLAW.md` |
| `session-warnings.log` | Log warnings agenti lipsa / stare degradata | `~/.openclaw/session-warnings.log` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Unde se stochează |
|---------|-----------|--------|-------------------|
| Checklist duration | Timp total execuție Pas 1-6 | < 60 secunde | câmp `duration_sec` în Cortex log (collection: sessions) |
| Issues detectate / sesiune | Probleme găsite înainte de a prelua taskuri | ≤ 1 issue/sesiune; 0 blockere în 7 zile consecutive | câmp `issues_count` în Cortex log (collection: sessions) |
| False negatives | Probleme existente nedetectate de checklist | 0 (absolut) | raportate manual în nightly audit ca `missed_by_checklist` |
| Checklist completion rate | % sesiuni în care checklist-ul e executat complet | 100% | `~/.openclaw/checklist-stats.json` (entry per dată) |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-03 | Creat inițial |
| 1.1 | 2026-03-04 | FORGE-AUDIT fix: Pas 1 adaugă GW2 check; Pas 2 adaugă verificare prezență toți 6 agenți; continue-on-error logic explicit |
| 1.2 | 2026-03-04 | FORGE-AUDIT v2 — D2: comenzi exacte Pas 3 (curl+răspuns așteptat), Pas 4 (stat/head), Pas 5 (sha256sum+json parse), Pas 6 format issues structurat [ISSUE]/[ACȚIUNE]/[IMPACT]; Pas 2 log warning cu format explicit; D3: WHEN adaugă restart sistem, HOW adaugă meta-enforcement via nightly audit + checklist-stats.json, VERIFY adaugă SOUL.md hash check; D4: target numeric issues/sesiune (≤1), completion rate 100%, storage location pentru toate metricile; D5: tabel complet cu GW2, toate procedurile referențiate cu path absolut, SOUL.md path, checklist-stats.json, session-warnings.log |

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-006, COM-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent (include toate cele 5 pași + VK)
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Salvat în Cortex cu metadata FORGE
- [x] Dependențe complete cu path-uri exacte pentru toate componentele referențiate
- [x] Metrics cu target-uri numerice și locație de stocare specificată
- [x] Comenzi executabile exacte pentru pașii cu ambiguitate (Pas 4, 5)
