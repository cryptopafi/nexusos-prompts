---
procedure_id: NEXUS-SOC-001
version: "1.0"
created: "2026-03-27"
author: "Genie (FORGE'd)"
domain: nexus-ops
type: procedure
status: active
owner: genie
tags: [ops, sentinel, health, incident-response, runbook]
---

# NEXUS-SOC — NexusOS System Operations Center

**Status**: ACTIVE
**Creat**: 2026-03-27
**Versiune**: 1.0
**Regulă asociată**: OPS-H-001
**Scope**: Runbook complet pentru operarea, monitorizarea și remedierea NexusOS V2. Acoperă daily health check, incident response, triage, intervenție manuală, quota management și recovery procedures.

---

## 1. Problema

Fără un runbook centralizat, GENIE și Pafi nu au un protocol unificat pentru:
- A ști exact ce să verifice dimineața pentru a confirma că sistemul e sănătos
- A răspunde rapid și corect la alertele SENTINEL fără a pierde timp diagnosticând
- A executa comenzile concrete de restart/fix fără a căuta prin scripturi
- A gestiona cotele de token/credits înainte ca acestea să blocheze operațiunile

Situații acoperite:
- Dimineața: verificare status înainte de a porni taskuri
- SENTINEL trimite alert Telegram — care e pasul următor?
- Agent nu mai răspunde — cum repornim fără să stricăm state-ul
- Cortex down — cum confirmi și cum repornești
- Tunnel VPS mort — cum refaci conexiunea
- Quota aproape epuizată — ce oprești, ce continui
- Soul hash mismatch — protocol de investigare
- Lock stale blocat — cum cureți fără risc

---

## 2. Procedura

### 2.1 Daily Health Check (dimineața, înainte de orice task)

**Durata estimată**: 2-3 minute. Execuție: Genie în background la primele mesaje ale zilei.

**Pas 1: Citește SENTINEL-HEALTH.md**

```bash
cat ~/.nexus/workspace/intel/SENTINEL-HEALTH.md
```

Verifici `overall_status`. Dacă e `GREEN` — sistemul e curat, continui. Altfel treci la pasul 2.

**Pas 2: Citește GENIE-STATUS.md**

```bash
cat ~/.nexus/workspace/intel/GENIE-STATUS.md
```

Verifici `health`, `blocked`, `active_tasks`. Dacă `blocked > 0` → §2.4 Manual Intervention.

**Pas 3: Verifică vârsta log-urilor heartbeat**

```bash
# SENTINEL heartbeat — trebuie să ruleze la fiecare 5min
tail -5 ~/.nexus/logs/sentinel-heartbeat.log

# GENIE V2 heartbeat — trebuie să ruleze la fiecare 30min
tail -5 ~/.nexus/logs/genie-v2-heartbeat.log
```

Dacă ultimul entry SENTINEL > 10 min → heartbeat oprit → §2.5 Recovery / restart-sentinel.

**Pas 4: Verifică Cortex**

```bash
curl -sf http://localhost:6400/api/health && echo "CORTEX OK" || echo "CORTEX DOWN"
# Fallback alias:
curl -sf http://localhost:6400/health && echo "CORTEX OK" || echo "CORTEX DOWN"
```

Dacă ambele fail → §2.5 Recovery / restart-cortex.

**Pas 5: Verifică Telegram relay**

```bash
pgrep -fl "telegram-relay" || echo "RELAY DOWN"
```

Dacă DOWN → §2.5 Recovery / restart-relay.

**Pas 6: Citește escalation-uri nerezolvate**

```bash
ls ~/.nexus/workspace/intel/GENIE-ESCALATION.md 2>/dev/null && \
  cat ~/.nexus/workspace/intel/GENIE-ESCALATION.md || echo "No pending escalation"
```

Dacă există escalation nerezolvată → §2.3 Incident Response.

**Checkpoint daily**: Toate cele 6 verificări OK = sistem sănătos, ziua poate porni.

---

### 2.2 Alert Triage Matrix

SENTINEL generează alerte cu statusuri conform contracts.md. Matricea de acțiuni:

| Status | Severity | Canal SENTINEL | Acțiune GENIE | Acțiune Pafi |
|:------:|:--------:|:--------------|:-------------|:-------------|
| GREEN | — | log only | nimic | nimic |
| YELLOW | LOW/MEDIUM | GENIE-STATUS.md | notează, monitorizează | nimic |
| ORANGE | HIGH | SENTINEL-NOTIFY.md | analizează cauza, încearcă auto-fix | notificat dacă auto-fix eșuează |
| RED | HIGH | Telegram + GENIE-STATUS | auto-fix obligatoriu, raportează rezultat | notificat via Telegram |
| CRITICAL | CRITICAL | Telegram + halt | scrie SENTINEL-NOTIFY, suspendă dispatches | acțiune imediată cerută |

**Reguli de prioritate** (din SENTINEL IL-3):
- `GREEN` → log
- `YELLOW` → GENIE-STATUS
- `ORANGE` → SENTINEL-NOTIFY
- `RED` → Telegram
- `CRITICAL` → Telegram + halt

**Bypass dedup** (SENTINEL IL-2): Aceeași alertă (SHA256) în 30 min = suppressă, EXCEPȚIE: CRITICAL bypasează dedup mereu.

**Rate limit Telegram** (SENTINEL IL-7): max 5 alerte/oră, CRITICAL bypasează.

---

### 2.3 Incident Response Flow

Folosit când SENTINEL trimite alert Telegram sau când daily check găsește RED/CRITICAL.

```
[SENTINEL alert / daily RED/CRITICAL]
        │
        ▼
Step 1: Citește SENTINEL-HEALTH.md complet
        → Identifică sistemele afectate (câmpul `systems`)
        → Listează alertele concrete (câmpul `alerts`)
        │
        ▼
Step 2: Clasifică severitatea
        → CRITICAL (soul hash mismatch, port 18789 exposed) → §2.3.1
        → RED (service down: cortex, relay, heartbeat stale) → §2.3.2
        → ORANGE (cost near cap, unexpected ports) → §2.3.3
        → YELLOW (keychain missing, sync slow) → §2.3.4
        │
        ▼
Step 3: Execută recovery procedure specifică (§2.5)
        │
        ▼
Step 4: Verifică recuperarea
        curl -sf http://localhost:6400/api/health
        cat ~/.nexus/workspace/intel/SENTINEL-HEALTH.md
        │
        ▼
Step 5: Scrie ACK în SENTINEL-ACK.md
        → Template: §2.6
        │
        ▼
Step 6: Dacă recovery eșuat → escalează la Pafi cu detalii complete
```

#### 2.3.1 Protocol CRITICAL

CRITICAL = STOP TOT. Niciun task nou nu se dispatchează.

```bash
# 1. Citește exact ce s-a întâmplat
cat ~/.nexus/workspace/intel/SENTINEL-HEALTH.md | grep -A5 "CRITICAL"

# 2. Identifică agentul afectat (soul hash mismatch sau port exposure)
# Soul hash mismatch → §2.5 soul-hash-investigation
# Port 18789 exposed → §2.5 fix-port-exposure

# 3. Scrie ACK imediat ca să oprești re-escalarea SENTINEL (TTL 30min)
cat > ~/.nexus/workspace/intel/SENTINEL-ACK.md << 'EOF'
---
msg_type: ack
from: genie
to: sentinel
timestamp: TIMESTAMP_PLACEHOLDER
correlation_id: CORR_ID_FROM_HEALTH
---
status: investigating
action: CRITICAL protocol initiated
EOF
```

#### 2.3.2 Protocol RED (service down)

```bash
# 1. Identifică serviciul din alerts[]
# cortex → §2.5 restart-cortex
# telegram_relay → §2.5 restart-relay
# agent_heartbeats → §2.5 restart-agent-heartbeat
# memory_sync → §2.5 fix-memory-sync

# 2. Execută recovery
# 3. Verifică la 2 min că SENTINEL-HEALTH.md se actualizează cu GREEN pe acel sistem
```

#### 2.3.3 Protocol ORANGE (cost/port)

```bash
# Cost ORANGE (session >= $5)
# → Oprește dispatch taskuri noi (GENIE IL-3)
# → Raportează Pafi: "Session cost $X — dispatch suspendat până la aprobare"
# → Nu oprești serviciile existente, doar nu adaugi taskuri noi

# Port ORANGE (unexpected listener)
# → Verifică manual: lsof -i :<port>
# → Dacă process cunoscut (ssh tunnel, expressvpn) → whitelist în SENTINEL config
# → Dacă process necunoscut → escalează la Pafi imediat
```

#### 2.3.4 Protocol YELLOW (degradat, non-critic)

```bash
# Keychain missing → §2.5 fix-keychain
# Sync slow → §2.5 fix-memory-sync
# Heartbeat slow (YELLOW, nu RED) → monitorizează încă 15 min înainte de intervenție
```

---

### 2.4 Manual Intervention Steps

#### restart-cortex

```bash
# Verifică că e oprit
curl -sf http://localhost:6400/api/health && echo "STILL UP — nu e nevoie de restart" && exit 0

# Repornire via launchctl (MacM4)
launchctl kickstart -k gui/$(id -u)/com.nexus.cortex 2>/dev/null || \
  launchctl stop com.nexus.cortex && sleep 2 && launchctl start com.nexus.cortex

# Fallback manual (dacă LaunchAgent nu e configurat)
cd ~/.nexus/cortex && node server.js &

# Verificare post-restart (așteaptă 5s)
sleep 5
curl -sf http://localhost:6400/api/health && echo "CORTEX RECOVERED" || echo "CORTEX STILL DOWN"
```

#### restart-sentinel

```bash
# Verifică lock stale
ls -la /tmp/nexus-sentinel-heartbeat.lock 2>/dev/null
lock_pid=$(cat /tmp/nexus-sentinel-heartbeat.lock 2>/dev/null || echo "0")
kill -0 "$lock_pid" 2>/dev/null && echo "PID $lock_pid alive — heartbeat running" || \
  (echo "Stale lock, clearing" && rm -f /tmp/nexus-sentinel-heartbeat.lock)

# Repornire via launchctl
launchctl kickstart -k gui/$(id -u)/com.nexus.sentinel-heartbeat-v2

# Verificare: log trebuie să aibă entry nou în ~30s
sleep 35
tail -3 ~/.nexus/logs/sentinel-heartbeat.log
```

#### restart-genie-heartbeat

```bash
# Verifică lock
lock_pid=$(cat /tmp/nexus-genie-v2-heartbeat.lock 2>/dev/null || echo "0")
kill -0 "$lock_pid" 2>/dev/null && echo "GENIE heartbeat running (PID $lock_pid)" || \
  (echo "Stale lock, clearing" && rm -f /tmp/nexus-genie-v2-heartbeat.lock)

# Repornire
launchctl kickstart -k gui/$(id -u)/com.nexus.genie-v2-heartbeat

# Verificare
sleep 35
tail -3 ~/.nexus/logs/genie-v2-heartbeat.log
```

#### restart-agent-heartbeat

```bash
# Usage: substitute AGENT_NAME with: genie, sentinel, iris, mercury, tech
launchctl kickstart -k gui/$(id -u)/com.nexus.${AGENT_NAME}-heartbeat 2>/dev/null \
  || launchctl kickstart -k gui/$(id -u)/com.nexus.${AGENT_NAME}-v2 2>/dev/null
# Verify: check for fresh heartbeat file within 60s
sleep 5 && ls -la ~/.nexus/v2/agents/${AGENT_NAME}/heartbeat
```

#### restart-relay

```bash
# Identifică procesul
pgrep -fl "telegram-relay"

# Repornire via launchctl
launchctl kickstart -k gui/$(id -u)/com.claude.telegram-relay

# Verificare
sleep 5
pgrep -fl "telegram-relay" && echo "RELAY UP" || echo "RELAY STILL DOWN"
tail -5 ~/.nexus/logs/telegram-relay.log 2>/dev/null || \
  tail -5 ~/repos/godagoo/claude-telegram-relay/logs/relay.log 2>/dev/null
```

#### clear-stale-lock

Folosit când un heartbeat/script nu mai pornește din cauza unui lock stale.

```bash
# SENTINEL lock
lock_pid=$(cat /tmp/nexus-sentinel-heartbeat.lock 2>/dev/null || echo "0")
if kill -0 "$lock_pid" 2>/dev/null; then
  echo "PID $lock_pid ALIVE — nu e stale, nu șterge!"
else
  echo "Stale — removing lock"
  rm -f /tmp/nexus-sentinel-heartbeat.lock
fi

# GENIE lock
lock_pid=$(cat /tmp/nexus-genie-v2-heartbeat.lock 2>/dev/null || echo "0")
if kill -0 "$lock_pid" 2>/dev/null; then
  echo "PID $lock_pid ALIVE — nu e stale"
else
  echo "Stale — removing lock"
  rm -f /tmp/nexus-genie-v2-heartbeat.lock
fi
```

#### fix-memory-sync

```bash
# Verifică ce e stale
ls -la ~/.nexus/.git/FETCH_HEAD

# Forțează sync
cd ~/.nexus && git fetch origin --quiet && git pull --rebase --quiet
echo "Memory sync at: $(date)"

# Verificare: FETCH_HEAD trebuie să aibă timestamp recent
stat -f "%Sm" ~/.nexus/.git/FETCH_HEAD  # macOS only — Linux: stat --format="%y"
```

#### fix-vps-tunnel

Tunelul SSH spre VPS (89.116.229.189) mort — Cortex remote sau dashboard inaccesibil.

```bash
# Verifică tunelul activ
pgrep -fl "ssh.*89.116.229.189" || echo "Tunnel mort"

# Repornire tunel (ajustează porturile la configurația ta)
ssh -N -L 6400:localhost:6400 pafi@89.116.229.189 -o ServerAliveInterval=30 &
echo "Tunnel PID: $!"

# Verificare
sleep 3
curl -sf http://localhost:6400/api/health && echo "VPS CORTEX REACHABLE" || echo "STILL DOWN"
```

#### fix-keychain

```bash
# Verifică ce lipsește (din SENTINEL-HEALTH.md alerts[])
# Exemplu: telegram-bot-token-claudemacm4
security find-generic-password -s "telegram-bot-token-claudemacm4" -w 2>/dev/null && \
  echo "KEY EXISTS" || echo "KEY MISSING"

# Adaugă cheia lipsă (înlocuiește VALUE cu valoarea reală)
security add-generic-password -s "telegram-bot-token-claudemacm4" -a "pafi" -w "VALUE"

# Verificare
security find-generic-password -s "telegram-bot-token-claudemacm4" -w 2>/dev/null && echo "ADDED OK"
```

#### soul-hash-investigation

```bash
[ -f ~/.nexus/v2/config/soul-hashes.yaml ] || { echo "soul-hashes.yaml missing"; exit 1; }

# Citește hash-urile așteptate
cat ~/.nexus/v2/config/soul-hashes.yaml

# Calculează hash-ul curent al agentului suspect
# Exemplu pentru iris:
shasum -a 256 ~/.nexus/v2/agents/iris/SOUL.md

# Compară cu valoarea din soul-hashes.yaml
# Dacă diferă: fișierul a fost modificat
# Verifică git log pentru modificări recente
cd ~/.nexus && git log --oneline -20 -- v2/agents/iris/SOUL.md

# Dacă modificarea e legitimă (tu ai editat): actualizează hash-ul în soul-hashes.yaml
# Dacă modificarea e suspectă: escalează Pafi IMEDIAT, nu actualiza hash-ul
```

#### fix-port-exposure

```bash
# Port 18789 expus pe 0.0.0.0 = CRITICAL (SENTINEL IL-6)
# Identifică procesul
lsof -i :18789

# Verifică că Cortex ascultă doar pe localhost
# Oprește Cortex
launchctl stop com.nexus.cortex

# Editează config să forțeze bind pe 127.0.0.1
# Fișier config: ~/.nexus/cortex/config.json sau server.ts (HOST=127.0.0.1)

# Repornire
launchctl start com.nexus.cortex
sleep 3

# Verificare: trebuie să nu apară 0.0.0.0:18789
lsof -i :18789 | grep -v "127.0.0.1" && echo "STILL EXPOSED — escalate!" || echo "PORT SECURED"
```

#### reset-circuit-breaker

Folosit când GENIE a tripper circuit breaker pe un agent (3 failures consecutive — IL-6).

```bash
# Verifică state.json pentru circuit_breaker
cat ~/.nexus/v2/agents/genie/resources/state.json | grep -A5 "circuit_breaker"

# Pre-flight validity check
jq . ~/.nexus/v2/agents/genie/resources/state.json > /dev/null 2>&1 || { echo "ERROR: state.json is invalid JSON"; exit 1; }
# Reset circuit breaker
jq '.circuit_breaker.tripped = false' ~/.nexus/v2/agents/genie/resources/state.json > /tmp/state.tmp && mv /tmp/state.tmp ~/.nexus/v2/agents/genie/resources/state.json
```

---

### 2.5 Communication Layer Verification

Verifică că fișierele de comunicare inter-agent sunt funcționale și nu sunt blocate.

```bash
# Directorul intel
ls -la ~/.nexus/workspace/intel/

# Fișiere critice și vârsta lor (trebuie să fie recente)
for f in SENTINEL-HEALTH.md SENTINEL-ACK.md GENIE-STATUS.md GENIE-ESCALATION.md; do
  path="$HOME/.nexus/workspace/intel/$f"
  if [ -f "$path" ]; then
    age=$(( ($(date +%s) - $(stat -f %m "$path")) / 60 ))
    echo "$f: ${age}min vechi"
  else
    echo "$f: LIPSESTE"
  fi
done
```

**SENTINEL-HEALTH.md** — scris de SENTINEL la fiecare ciclu (5 min). Dacă > 10 min = SENTINEL heartbeat mort.

**SENTINEL-ACK.md** — scris de GENIE când confirmă o escalare. Format:

```bash
cat > ~/.nexus/workspace/intel/SENTINEL-ACK.md << EOF
---
msg_type: ack
from: genie
to: sentinel
timestamp: $(date -u +%FT%TZ)
correlation_id: "$(grep 'correlation_id:' ~/.nexus/workspace/intel/SENTINEL-HEALTH.md | head -1 | sed 's/.*: *//' | tr -d '"')"
---
status: acknowledged
action: investigating — see GENIE-STATUS.md for details
EOF
```

**SENTINEL-NOTIFY.md** — scris de GENIE când vrea să alerteze SENTINEL despre probleme interne. SENTINEL îl citește la fiecare ciclu.

**GENIE-STATUS.md** — scris de GENIE la fiecare heartbeat. Dacă > 60 min = GENIE heartbeat mort.

**GENIE-ESCALATION.md** — scris de SENTINEL când trimite escalare spre GENIE. TTL: 30 min. Dacă nu e consumat în TTL, SENTINEL re-escalează sau trimite direct Telegram.

---

### 2.6 Quota Management

#### Claude Token Quota

**Caps** (din MEMORY.md):
- Opus: 100M tokens / sesiune
- Sonnet: 250M tokens / sesiune
- Weekly reset: Vineri 7-8 AM

```bash
# Verifică usage curent
~/.claude/check-usage-v2.sh

# Dashboard local
open http://100.81.233.9:3200/usage
```

**Thresholds și acțiuni**:

| Threshold | Acțiune |
|:---------:|:--------|
| < 70% | Normal operations |
| 70% warn | Notify Pafi, switch non-critical tasks to Sonnet/Haiku |
| 85% throttle | Stop Opus dispatch, Sonnet only pentru noi taskuri |
| 95% full | STOP tot. Niciun task nou. Informează Pafi că ziua e terminată pentru LLM tasks |
| > 95% | Emergency: scrie summary în MEMORY.md, salvează tot în Cortex, oprește sesiunea |

```bash
# Forțare switch la Sonnet (editează routing în GENIE dispatch)
# Model routing: ~/.nexus/rules/model-routing-table.md
# Nu modifica iron-laws.md sau SOUL.md (IL-1)
```

#### ScrapeCreators Credits

```bash
# Verifică usage în dashboard ScrapeCreators (web)
# URL: https://www.scrapecreators.com/dashboard
# Notă: dacă la_checker e configurat, creditele apar în ECHELON reports

# Limită: plan curent — verifică manual
# La < 100 credite rămase → oprești taskuri de scraping X/LinkedIn
# La 0 → wait for monthly reset sau upgrade plan
```

#### Apify Compute Units (CUs)

```bash
# Verifică usage Apify
# URL: https://console.apify.com/billing
# Plan recomandat: Starter ($29/mo) — din session 2026-03-26

# La 80% CU consumate → oprești scraping non-esențial (ECHELON batch)
# La 95% → oprești tot Apify, folosești Exa fallback pentru search
# Emergency fallback: agent-browser direct (mai lent, zero CU cost)
```

---

### 2.7 Recovery Procedures per RED Condition

Mapare completă: sistem în RED → recovery procedure.

| Sistem RED | Recovery Procedure | Timp estimat |
|:-----------|:------------------|:------------:|
| `cortex` | §2.4 restart-cortex | 1-2 min |
| `memory_sync` | §2.4 fix-memory-sync | 30s |
| `agent_heartbeats` | §2.4 restart-agent-heartbeat | 1-2 min |
| `telegram_relay` | §2.4 restart-relay | 1 min |
| `soul_hashes` (CRITICAL) | §2.4 soul-hash-investigation | 5-15 min |
| `port_scan` (18789 exposed) | §2.4 fix-port-exposure | 5 min |
| `cost_tracking` (halted) | §2.3.3 ORANGE protocol | imediat |
| VPS tunnel | §2.4 fix-vps-tunnel | 2 min |
| Stale lock (orice heartbeat) | §2.4 clear-stale-lock | 30s |
| Circuit breaker tripped | §2.4 reset-circuit-breaker | 2 min |

**Escalare automată la Pafi** (când recovery eșuează după 2 încercări):

```bash
# GENIE scrie în SENTINEL-NOTIFY.md pentru Telegram
cat > ~/.nexus/workspace/intel/SENTINEL-NOTIFY.md << EOF
---
msg_type: escalation
from: genie
to: sentinel
priority: high
timestamp: $(date -u +%FT%TZ)
---
## GENIE Escalation — Recovery Failed

- System: <SYSTEM_NAME>
- Recovery attempted: 2x
- Status after retry: STILL RED
- Action required: Manual intervention from Pafi

Please review SENTINEL-HEALTH.md for full details.
EOF
```

---

### 2.8 Test Cases

1. **Normal flow (dimineata verde)**: SENTINEL-HEALTH.md `overall_status: GREEN` → citești GENIE-STATUS.md → `health: GREEN` → zero acțiuni → ziua pornește.

2. **YELLOW keychain missing**: SENTINEL-HEALTH.md arată `keychain: telegram-bot-token-claudemacm4 missing` → §2.3.4 → §2.4 fix-keychain → restart SENTINEL → verificare la 10 min.

3. **RED cortex down**: Alertă Telegram "Cortex MCP not responding" → §2.3.2 → §2.4 restart-cortex → verificare curl → ACK scris → SENTINEL-HEALTH.md se actualizează GREEN.

4. **CRITICAL soul hash mismatch**: Alertă Telegram cu [CRITICAL] → §2.3.1 → scriere ACK imediată → §2.4 soul-hash-investigation → dacă legitim: update hash; dacă suspect: STOP TOT + escalare Pafi cu git log.

5. **Quota 85%**: SENTINEL cost check returnează `warning` → GENIE oprește dispatch Opus → notifică Pafi → switch la Sonnet pentru taskuri rămase.

---

## 3. Cortex Logging

La fiecare incident rezolvat:

```json
{
  "text": "NEXUS-SOC incident resolved: {system} {severity} → {action} → {outcome}",
  "collection": "procedures",
  "metadata": {
    "type": "ops_incident",
    "procedure": "NEXUS-SOC-001",
    "rule_id": "OPS-H-001",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["sentinel", "ops", "incident", "nexusos-v2"]
  }
}
```

La daily health check (dacă GREEN, entry concis):

```json
{
  "text": "Daily health check GREEN — {date}. All 6 systems nominal.",
  "collection": "procedures",
  "metadata": {
    "type": "ops_daily_check",
    "procedure": "NEXUS-SOC-001",
    "tags": ["daily-health", "sentinel", "green"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step 1 (dimineața, prima acțiune a lui Genie în sesiune)
- SENTINEL heartbeat (la fiecare 5 min — verificare automată)
- GENIE heartbeat Step 8 (write status — citit de SENTINEL la ciclul următor)
- Post-incident: după orice remediere RED/CRITICAL

### WHEN
- Zilnic la prima sesiune activă
- La orice alertă SENTINEL indiferent de severitate
- La orice comandă Pafi despre "sistem", "agent down", "restart", "cât costă"

### HOW (violation detection)
- SENTINEL-HEALTH.md mai vechi de 10 min fără explicație → heartbeat oprit, violație
- GENIE-STATUS.md mai vechi de 60 min → GENIE heartbeat oprit
- Incident RED neacknowledged în SENTINEL-ACK.md după 10 min → GENIE nu a răspuns
- Daily health check nesolicitat la prima sesiune a zilei → violație procedurală
- Runner: SENTINEL heartbeat (verificare automată vârstă fișiere), Genie self-check la sesiune start

### CONNECT
- `SENTINEL IL-3` → escalation chain definită în §2.2
- `GENIE IL-3` → cost cap acțiuni definite în §2.6
- `contracts.md` (`~/.nexus/v2/agents/sentinel/resources/contracts.md`) → structura JSON a fiecărui check
- `sentinel-heartbeat-v2.sh` → produce SENTINEL-HEALTH.md la fiecare ciclu
- `heartbeat-v2.sh` (GENIE) → consumă SENTINEL-HEALTH.md, produce GENIE-STATUS.md
- `procedure-health.json` → adaugă entry NEXUS-SOC-001

### VERIFY

La finalul oricărei execuții a acestei proceduri, agentul verifică:
- [ ] Daily health check a acoperit toate cele 6 verificări din §2.1?
- [ ] Incidentele RED/CRITICAL au primit ACK în SENTINEL-ACK.md?
- [ ] Recovery procedures s-au finalizat cu verificare explicită (curl/pgrep/log)?
- [ ] Quota checks efectuate dacă sesiunea e lungă (> 2h)?
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per execuție FORGE** (per VK-H-001):
1. **Execution VK**: `[PROC] NEXUS-SOC-001 | §1 §2 §3 §4 VER | complete`
2. **Save VK**: `[CORTEX] "NEXUS-SOC incident {type}" | FORGE | rule: OPS-H-001 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|:-----------|:------|:--------|
| Daily health check read + triage | Sonnet 4.6 (Genie) | Operație rutinară, deterministă |
| Incident response + recovery commands | Sonnet 4.6 (Genie) | Comenzile sunt fixe din runbook |
| Soul hash investigation (CRITICAL) | Opus 4.6 | Necesită reasoning profund, context git + config |
| Procedure audit periodic | Opus 4.6 subagent | Review structural — Sonnet ratează gap-uri (DEV-H-010) |
| Recovery bash commands | Deterministic (bash direct) | Nu e LLM — e execuție script |

---

## 5. Quick Reference Card

Copiat de Genie în memorie de lucru pentru acces rapid fără a reciti documentul complet.

```
NEXUS-SOC QUICK REF
===================
Intel dir:    ~/.nexus/workspace/intel/
Logs dir:     ~/.nexus/logs/
Cortex:       http://localhost:6400/api/health
SENTINEL hb:  every 5min → sentinel-heartbeat.log
GENIE hb:     every 30min → genie-v2-heartbeat.log

STATUS FILES:
  SENTINEL-HEALTH.md  → written by SENTINEL (5min)
  GENIE-STATUS.md     → written by GENIE (30min)
  SENTINEL-ACK.md     → written by GENIE to confirm alert receipt
  SENTINEL-NOTIFY.md  → written by GENIE to notify SENTINEL
  GENIE-ESCALATION.md → written by SENTINEL to escalate to GENIE

TRIAGE:
  GREEN  → log
  YELLOW → monitor
  ORANGE → auto-fix + notify if fail
  RED    → fix + Telegram
  CRITICAL → stop dispatches + Telegram + investigate

RECOVERY ORDER:
  1. clear-stale-lock (if heartbeat won't start)
  2. service-specific restart
  3. verify (curl/pgrep/log tail)
  4. write SENTINEL-ACK.md

QUOTA ACTIONS:
  70% Opus → switch to Sonnet
  85% Opus → stop Opus dispatch
  95% any  → STOP, save state, alert Pafi
```
