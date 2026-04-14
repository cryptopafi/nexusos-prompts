---
type: procedure
created: 2026-03-17
status: active
slug: nightly-audit-openclaw
tags: [procedure, nexus]
---

# Nightly Audit OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.2
**Regulă asociată**: OPS-H-001 (Technical Autonomy), MEM-H-001 (Everything to Cortex)
**Scope**: Auditul complet automat al sistemului OpenClaw la 01:00 EET — detectarea problemelor acumulate în 24h și pregătirea morning briefing-ului de la 09:00.

---

## 1. Problema

Monitorizarea continuă (AGENT-HEALTH-MONITORING) detectează probleme în timp real dar nu face o sinteză a sănătății sistemului pe 24h. Fără un audit nocturn:
- Drift-uri subtile ale agenților (scor ușor scăzut, nu critic) rămân neobservate săptămâni
- SOUL.md-urile agenților nu sunt verificate complet (doar Richard la 15 min)
- Memoria agenților nu e auditată (MEMORY.md prea mare, lecții contradictorii)
- Pafi nu are un raport de stare la dimineață — pornește sesiunea fără context sistem

Situații acoperite:
- Audit nocturn zilnic (01:00 EET) via `nightly-audit.sh`
- Morning briefing generat pentru Pafi (09:00 EET sau la prima sesiune)
- Audit manual declanșat de Pafi sau Genie după incident

**Exemplu concret**: Agent Aria a acumulat drift comportamental timp de 3 săptămâni (scor SOUL.md: 0.82 → 0.71) fără ca AGENT-HEALTH-MONITORING să alerteze (threshold > 0.6). Fără audit nocturn sintetic, Pafi n-a descoperit problema decât după ce Aria a delegat incorect un task critic spre Muse în loc de Richard. Un audit nocturn care compară scorurile zilnice ar fi detectat trendul descendent după ziua 3.

---

## 2. Procedura

### Pas 1: System Integrity Audit
Verifică integritatea completă a sistemului:
- **SOUL.md hashes** — toate 6 agenți, comparat cu baseline din `~/.nexus/soul-hashes.json`
  - Dacă hash diferit → loghează ca INCIDENT cu diff și timestamp modificare
  - Actualizează baseline NUMAI dacă modificarea e autorizată (există entry în `~/.nexus/soul-change-log.md`)
- **Port audit** — 18789, 18791, 18792, 19001, 19003, 19004 → toate bound pe localhost?
- **Config integritate** — `~/.openclaw/openclaw.json` valid JSON5, fără modificări neautorizate

### Pas 2: Memory Health Audit
Verifică starea memoriei per agent (**rulat de script bash la 01:00**):
- **MEMORY.md size** per agent — `wc -l ~/.openclaw/workspaces/<agent>/MEMORY.md` → dacă > 200 linii → marcat pentru compaction
- **Daily logs** din ultima 24h — `ls -la ~/.openclaw/memory/YYYY-MM-DD.md` → există pentru fiecare agent activ?
- **SQLite WAL files** — `ls -la ~/.openclaw/memory/*.sqlite-wal` → trebuie să fie goale sau absente
- **Lecții contradictorii**: această verificare necesită LLM — se mută la Pas 3 (Genie la 09:00, nu bash la 01:00)

### Pas 3: Agent Performance Review + Quality Spot Check
Analiză performanță și calitate — **rulat de Genie (Sonnet) la 09:00** (nu de script bash):
- **Token usage**: RPC `sessions.history` per agent → numără `total_tokens`, identifică outlieri (>2x media)
- **Task completion**: sesiuni cu status `abandoned` sau `error` în ultimele 24h → count per agent
- **Quality spot check**: citește 1-2 sesiuni random per agent (via `sessions.history`) și evaluează dacă output-ul respectă SOUL.md. Criterii: (1) rolul e respectat? (2) delegarea e corectă? (3) Cortex a fost consultat?
- **Lecții contradictorii**: caută în MEMORY.md per agent patterns semantice contradictorii (ex: "always use X" + "never use X") — necesita Genie judgment, nu regex
- Identificare pattern: același tip de task eșuează repetat → candidat pentru procedură nouă

**Separare clară**:
- **01:00 bash**: Pas 1 (integrity) + Pas 2 (memory size/WAL) + Pas 4 (Cortex queries) + Pas 5 (incident log) + Pas 6 (raport)
- **09:00 Genie**: Pas 3 (performance + quality) + Pas 7 (briefing Telegram)

### Pas 4: Cortex Health Check
Verifică sănătatea knowledge base-ului:
- Execută CORTEX-HEALTH-CHECK-OPENCLAW.md Pas 1-3 (`~/.nexus/procedures/openclaw/CORTEX-HEALTH-CHECK-OPENCLAW.md`)
- Verifică că niciun agent n-a scris în `rules` collection — query: `SELECT source_agent, count(*) FROM cortex WHERE collection='rules' AND created_at > NOW()-INTERVAL 24h GROUP BY source_agent` → dacă apare agent non-Richard → INCIDENT
- Verifică că `procedures` collection nu are entries noi neautorizate: `SELECT * FROM cortex WHERE collection='procedures' AND pending_review=false AND source_agent != 'richard' AND created_at > NOW()-INTERVAL 24h` → dacă rezultat → INCIDENT
- Count total entries per collection — trend anormal (creștere bruscă > 20% sau scădere bruscă > 10% față de media 7 zile): `SELECT collection, count(*) FROM cortex GROUP BY collection`

### Pas 5: Incident Log Review
Revizuiește incidentele din ultima 24h:
- Citește `~/.openclaw/richard-incidents.log` — incidente noi? (`tail -100 ~/.openclaw/richard-incidents.log | grep $(date +%Y-%m-%d)`)
- Citește alertele Telegram din ultimele 24h: `~/.openclaw/logs/telegram-log.md` (`tail -200 ~/.openclaw/logs/telegram-log.md`)
- Identifică pattern-uri: același tip de incident repetat → procedură de prevenție lipsă
- Dacă > 3 incidente în 24h → flag pentru Pafi (sistem instabil) → include în raportul Pas 6 cu tag `UNSTABLE`

### Pas 6: Generare Raport Nightly
Salvează raportul complet (**rulat de bash la finalul auditului 01:00**):
- Fișier: `~/.openclaw/reports/nightly-YYYY-MM-DD.md`
- Structura obligatorie a raportului:
  ```
  # Nightly Audit Report — YYYY-MM-DD
  **Verdict**: OK | WARNINGS | INCIDENTS | PARTIAL
  **partial**: true|false
  **last_step_completed**: <N>

  ## Pas 1 — System Integrity: OK|INCIDENT
  [detalii findings sau "All hashes match. Ports OK. Config valid."]

  ## Pas 2 — Memory Health: OK|WARNING
  [agents cu MEMORY.md > 200 linii sau "All within limits."]

  ## Pas 4 — Cortex Health: OK|INCIDENT
  [anomalii detectate sau "Collections stable. No unauthorized writes."]

  ## Pas 5 — Incidents 24h: N incidente
  [lista incidente sau "None."]

  ## Recomandări
  [lista acțiuni sau "No action required."]
  ```
- Scurt summary în `~/.openclaw/nightly-audit-last.log`: `echo "$(date -u +%Y-%m-%dT%H:%M:%SZ) | verdict: OK|WARNINGS|INCIDENTS | partial: false" >> ~/.openclaw/nightly-audit-last.log`
- Salvează în Cortex: `collection="sessions", type="nightly_audit_report"`

**Fallback pentru esec parțial**: dacă bash script-ul cade la jumătate (ex: Pas 3 blocat), salvează raportul PARȚIAL cu flag `partial=true` și timestamp-ul ultimului pas completat. Raportul parțial e mai bun decât niciun raport — Genie poate continua din el la 09:00. Nu lăsa `nightly-audit-last.log` neactualizat — scrie un entry chiar și în caz de eroare: `PARTIAL audit | last_step: <N> | error: <mesaj>`.

### Pas 7: Morning Briefing (09:00 EET)
Generează briefingul de dimineață pentru Pafi:
- Trimite prin @Claudeautomationbot: summary raport nightly (3-5 bullets max)
- Format:
  ```
  📊 OpenClaw Morning Brief — [DATA]
  ✅ / ⚠️ / 🔴 Sistem: [status general]
  • [finding 1]
  • [finding 2]
  • Recomandare: [acțiune sugerată dacă e cazul]
  ```
- Dacă nicio problemă: "✅ Sistem OK. Nimic de acționat."

---

## 3. Cortex Logging

```json
{
  "text": "Nightly audit [DATA]. SOUL.md hashes: [OK/INCIDENT]. Memory: [OK/needs compaction: agentX]. Performance: [OK/degradat: agentY]. Cortex: [OK/anomalie]. Incidente 24h: [N]. Verdict: [OK/WARNINGS/INCIDENTS].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "nightly_audit_report",
    "procedure": "NIGHTLY-AUDIT-OPENCLAW",
    "rule_id": "OPS-H-001",
    "tags": ["audit", "nightly", "openclaw", "genie", "supervisor", "report"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- `~/.openclaw/nightly-audit.sh` — script executat de LaunchAgent zilnic la 01:00 EET
- Manual: Genie poate rula oricare pas individual la cerere
- GENIE-SUPERVISOR-SESSION-CHECKLIST Pas 4 — verifică că auditul a rulat

### WHEN
- Automat: zilnic la 01:00 EET (când MacIntel e pornit)
- Manual: după orice incident major, la cererea explicită a lui Pafi

### HOW (violation detection)
- `nightly-audit-last.log` mai vechi de 24h → audit ratat → Genie notifică Pafi la sesiune prin @Claudeautomationbot: `"[ALERT] Nightly audit RATAT: ultima rulare <timestamp>. Verifică LaunchAgent pe MacIntel."`
- Raport generat fără Pas 1 (SOUL.md hashes) → audit incomplet → violation → Genie adaugă `partial=true` și reia Pas 1 manual la 09:00
- Incident detectat la Pas 1-4 și nelogat în Cortex → violation MEM-H-001 → Genie loghează retrospectiv la sesiunea 09:00 și notifică Pafi
- `nightly-audit-last.log` conține `PARTIAL` → Genie continuă auditul de la `last_step_completed` la sesiunea 09:00
- Runner: LaunchAgent zilnic + verificare manuală Genie la session start (`~/.openclaw/nightly-audit-last.log` — primul check în GENIE-SUPERVISOR-SESSION-CHECKLIST Pas 4)

### CONNECT
- `OPS-H-001` → autonomie tehnică: auditul rulează fără intervenție Pafi
- `MEM-H-001` → raportul salvat în Cortex obligatoriu
- `GENIE-SUPERVISOR-SESSION-CHECKLIST` → verifică la Pas 4 că auditul a rulat
- `AGENT-HEALTH-MONITORING` → complementar (continuu vs nocturn complet)
- `SOUL-MD-IMMUTABILITY.md` → apelat dacă Pas 1 detectează modificare hash
- `CORTEX-HEALTH-CHECK-OPENCLAW.md` → apelat la Pas 4
- `CORTEX-WRITE-PERMISSIONS.md` → queries de audit folosite la Pas 4
- `procedure-health.json` → adaugă entry `"NIGHTLY-AUDIT-OPENCLAW": "active"`

### VERIFY
La verificarea configurației (audit manual):
- [ ] LaunchAgent `nightly-audit.sh` activ și schedulat la 01:00? (`launchctl list | grep nightly-audit`)
- [ ] `nightly-audit-last.log` există și e < 24h? (`stat -f "%Sm" ~/.openclaw/nightly-audit-last.log`)
- [ ] Rapoartele din `~/.openclaw/reports/` sunt salvate și în Cortex?
- [ ] Morning briefing livrat la 09:00 (sau la prima sesiune)? (verifică `~/.openclaw/logs/telegram-log.md` pentru entry morning brief)
- [ ] Pas 3 (Genie 09:00 review) executat și loggat în Cortex pentru data curentă?
- [ ] Metrics colectate și salvate în `~/.openclaw/reports/metrics-YYYY-MM-DD.json`?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e complet implementată

**VK-uri obligatorii**:
1. `✅ [PROC] NIGHTLY-AUDIT-OPENCLAW | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Nightly Audit OpenClaw" | FORGE ✓ | rule: OPS-H-001 | v1.2`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Pas 1-4 (checks automate) | Script bash + Genie Sonnet | Verificări structurate, cost redus |
| Pas 3 quality review (spot check sesiuni) | Opus subagent | Evaluare calitate output necesită judgment |
| Generare raport + briefing | Genie (Sonnet) | Sinteză și comunicare |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `nightly-audit.sh` | Script de execuție | `~/.openclaw/nightly-audit.sh` |
| LaunchAgent | Scheduling 01:00 | `~/Library/LaunchAgents/com.openclaw.nightly-audit.plist` |
| `soul-hashes.json` | Baseline SOUL.md hashes | `~/.nexus/soul-hashes.json` |
| `soul-change-log.md` | Log modificări SOUL.md autorizate | `~/.nexus/soul-change-log.md` |
| `nightly-audit-last.log` | Timestamp + verdict ultimul audit | `~/.openclaw/nightly-audit-last.log` |
| Reports dir | Rapoarte arhivate nightly | `~/.openclaw/reports/nightly-YYYY-MM-DD.md` |
| `telegram-log.md` | Log alerte Telegram trimise/primite | `~/.openclaw/logs/telegram-log.md` |
| `CORTEX-HEALTH-CHECK-OPENCLAW.md` | Procedura health check Cortex (Pas 4) | `~/.nexus/procedures/openclaw/CORTEX-HEALTH-CHECK-OPENCLAW.md` |
| `CORTEX-WRITE-PERMISSIONS.md` | Queries audit permisiuni scriere Cortex | `~/.nexus/procedures/openclaw/CORTEX-WRITE-PERMISSIONS.md` |
| `procedure-health.json` | Registry proceduri active sistem | `~/.nexus/procedure-health.json` |
| `openclaw.json` | Configurație principala OpenClaw | `~/.openclaw/openclaw.json` |
| `@Claudeautomationbot` | Canal notificări morning briefing | MacM4 relay (`com.claude.telegram-relay`) |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Baseline | Colectare |
|---------|-----------|--------|----------|-----------|
| Audit success rate | % nopți cu audit completat (partial=false) în ultimele 30 zile | > 95% (max 1 ratare/30 zile) | stabilit la 30 zile de la activare | Genie numără entries din `nightly-audit-last.log` săptămânal |
| Mean findings per audit | Nr. total findings (WARNING+INCIDENT) per noapte, medie 7 zile | < 3 findings/noapte; trend descrescător pe 30 zile | prima săptămână de operare | Genie extrage din rapoartele `~/.openclaw/reports/` și salvează în `~/.openclaw/reports/metrics-YYYY-MM-DD.json` |
| MTTD overnight issues | De la timestamp prima apariție în log la timestamp detectare în raport nightly | < 8 ore | N/A (absolut) | Calculat automat de `nightly-audit.sh` la Pas 6 din diff timestamps |
| Briefing delivery rate | % dimineți cu briefing Telegram livrat la 09:00 (±30 min) în ultimele 30 zile | > 90% (max 3 ratări/30 zile) | stabilit la 30 zile de la activare | Genie verifică `~/.openclaw/logs/telegram-log.md` pentru entries tip morning_brief |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-03 | Creat inițial |
| 1.1 | 2026-03-04 | FORGE-AUDIT fix: Pas 2/3 separat explicit bash(01:00) vs Genie+Opus(09:00); lecții contradictorii mutat la Pas 3 (LLM necesar); Pas 6 adaugă fallback raport parțial cu flag partial=true |
| 1.2 | 2026-03-04 | FORGE-AUDIT D1-D6 completat la PASS: D1 exemplu concret incident Aria drift; D2 comenzi exacte Pas 4 (SQL queries), Pas 5 (path telegram-log.md), Pas 6 (structura raport + comanda log); D3 HOW extins cu actiuni post-detectie si canal notificare, VERIFY +2 itemi (Genie 09:00 + metrics); D4 metrics cu baseline si mecanism colectare per metrica; D5 +5 dependente cu path-uri exacte; D6 checklist +2 itemi (dry-run, metrics baseline) |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, MEM-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu comenzi concrete de verificare
- [x] Procedura descrie CE se face (nu CUM e implementat intern)
- [x] VK format specificat (2 VK-uri distincte)
- [x] Entry adăugat în `procedure-health.json` (`"NIGHTLY-AUDIT-OPENCLAW": "active"`)
- [x] `nightly-audit.sh` scris și LaunchAgent configurat (`com.openclaw.nightly-audit.plist` la 01:00)
- [x] `soul-hashes.json` creat cu baseline-urile curente (`~/.nexus/soul-hashes.json`)
- [x] Salvat în Cortex cu metadata FORGE
- [x] Toate dependențele listate în D5 cu path-uri exacte (inclusiv `telegram-log.md`, `procedure-health.json`)
- [ ] Dry-run manual executat și raport `nightly-YYYY-MM-DD.md` generat corect (verificat de Pafi sau Genie)
- [ ] Metrics baseline stabilit (după primele 30 zile de operare)
