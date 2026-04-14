---
type: procedure
created: 2026-03-17
status: active
slug: gateway2-watchdog-setup
tags: [procedure, nexus]
---

# Gateway 2 Watchdog Setup — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.1
**Regulă asociată**: OPS-H-001 (Technical Autonomy), SEC-H-006 (Richard Autonomy — Genie as Guardian)
**Scope**: Configurarea și operarea watchdog-ului Gateway 2 — monitorizarea continuă a GW1 (port 18789/health) cu alertare Telegram automată la downtime.

---

## 1. Problema

Fără un watchdog dedicat, downtime-ul GW1 nu este detectat automat:
- Dacă GW1 cade noaptea, Pafi află dimineața (gap potențial de ore)
- Monitoring din AGENT-HEALTH-MONITORING rulează la 5 min dar e script bash fără persistență de stare
- Nu există un mecanism de alertare Telegram dedicat pentru downtime GW1

Gateway 2 Watchdog = daemon persistent care verifică GW1 la fiecare 60 secunde și alertează imediat.

Situații acoperite:
- GW1 cade (crash, kill, restart)
- GW1 nu răspunde pe `/health` (timeout > 10s)
- Recovery GW1 — alertă de recuperare trimisă automat

---

## 2. Componente

### Fișiere de configurare

| Fișier | Path | Rol |
|--------|------|-----|
| `watchdog.json` | `~/.openclaw/watchdog.json` | Config watchdog (port, target, threshold, alerting) |
| `watchdog-gateway2.sh` | `~/.openclaw/scripts/watchdog-gateway2.sh` | Script de monitorizare |
| `watchdog-state.json` | `~/.openclaw/watchdog-state.json` | Stare persistentă (fail_count, alerted, last_ok/fail) |
| LaunchAgent | `~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist` | Scheduling 60s, KeepAlive=true |

### Parametri operaționali

| Parametru | Valoare | Semnificație |
|-----------|---------|-------------|
| Target | `http://localhost:18789/health` | Endpoint monitorizat (GW1 data port) |
| Interval | 60 secunde | Frecvența check-urilor (via LaunchAgent StartInterval) |
| Failure threshold | 2 eșecuri consecutive | Înainte de a trimite alertă |
| Timeout per check | 10 secunde | `curl --max-time 10` |
| Alert channel | Telegram `@Claudeautomationbot` | Chat ID: `623593648` |
| Token | `TELEGRAM_BOT_TOKEN` (keychain) | `security find-generic-password -s TELEGRAM_BOT_TOKEN` |

### Logică script (watchdog-gateway2.sh)

```
1. Citește stare din watchdog-state.json (fail_count, alerted)
2. curl -fsS http://localhost:18789/health
   - OK → dacă fusese alertă → trimite "✅ GW1 recovered" pe Telegram
            → resetează state: fail_count=0, alerted=false
   - FAIL → incrementează fail_count
            → dacă fail_count >= 2 și nu a alertat → trimite "🚨 GW1 DOWN"
            → salvează stare cu alerted=true
3. Loghează în ~/.openclaw/logs/watchdog.log
```

### LaunchAgent config

```xml
<key>Label</key><string>com.genie.openclaw-watchdog</string>
<key>StartInterval</key><integer>60</integer>   <!-- 1 minut -->
<key>KeepAlive</key><true/>                      <!-- restart automat dacă cade -->
<key>RunAtLoad</key><true/>
```

---

## 3. Procedura de Setup

### 3A. Reinstalare (fișierele există deja)

#### Pas 1: Verificare fișiere existente
```bash
ls -la ~/.openclaw/watchdog.json
ls -la ~/.openclaw/scripts/watchdog-gateway2.sh
launchctl list | grep openclaw-watchdog
```

#### Pas 2: Dacă LaunchAgent nu e activ
```bash
# macOS modern (Ventura+):
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist

# macOS legacy fallback:
launchctl load ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist
```

#### Pas 3: Verificare funcționare
```bash
# Verifică că rulează
launchctl list com.genie.openclaw-watchdog

# Verifică logs recente
tail -20 ~/.openclaw/logs/watchdog.log

# Verifică stare curentă
cat ~/.openclaw/watchdog-state.json
```

#### Pas 4: Test manual
```bash
# Rulează o singură iterație
bash ~/.openclaw/scripts/watchdog-gateway2.sh
cat ~/.openclaw/watchdog-state.json  # trebuie să aibă last_ok sau last_fail
```

### 3B. Setup from scratch (dacă fișierele nu există)

#### Pas 1: Creează structura de directoare
```bash
mkdir -p ~/.openclaw/scripts ~/.openclaw/logs
```

#### Pas 2: Creează watchdog.json
```bash
cat > ~/.openclaw/watchdog.json << 'EOF'
{
  "target": "http://localhost:18789/health",
  "interval": 60,
  "failure_threshold": 2,
  "timeout": 10,
  "telegram_chat_id": "623593648",
  "token_source": "keychain:TELEGRAM_BOT_TOKEN"
}
EOF
```

#### Pas 3: Creează watchdog-state.json (stare inițială)
```bash
echo '{"fail_count":0,"alerted":false,"last_ok":null,"last_fail":null}' > ~/.openclaw/watchdog-state.json
```

#### Pas 4: Creează scriptul watchdog-gateway2.sh
Conținutul trebuie să implementeze logica din §2 (Logică script). Referință: Codex batch 040 output.

#### Pas 5: Creează LaunchAgent plist
Copiază din template-ul din §2 (LaunchAgent config) în:
`~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist`

#### Pas 6: Încarcă și verifică
```bash
chmod +x ~/.openclaw/scripts/watchdog-gateway2.sh
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist
launchctl list com.genie.openclaw-watchdog  # trebuie să returneze PID
```

#### Pas 7: Adaugă token Telegram în keychain (dacă lipsește)
```bash
security add-generic-password -a "$USER" -s TELEGRAM_BOT_TOKEN -w "TOKEN_VALUE_HERE"
```

### 3C. Troubleshooting

| Simptom | Diagnostic | Rezolvare |
|---------|-----------|-----------|
| `launchctl load` dă "service already loaded" | Deja activ | `launchctl list com.genie.openclaw-watchdog` pt confirmare |
| `launchctl bootstrap` dă "Could not find specified service" | Plist invalid/lipsă | Verifică XML valid: `plutil ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist` |
| Log-ul nu se actualizează | Script crash sau LaunchAgent oprit | Verifică: `launchctl list com.genie.openclaw-watchdog`. Dacă exit code != 0, rulează manual: `bash -x ~/.openclaw/scripts/watchdog-gateway2.sh` |
| Alertă Telegram nu ajunge | Token invalid sau bot blocat | Testează: `security find-generic-password -s TELEGRAM_BOT_TOKEN -w` apoi curl manual către API Telegram |
| False positives frecvente | GW1 răspunde lent | Crește timeout în watchdog.json (10 → 15s) sau threshold (2 → 3) |

### 3D. Rollback / Dezactivare completă

```bash
# Oprește daemon-ul
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist

# Legacy fallback:
launchctl unload ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist

# Verificare oprire
launchctl list | grep openclaw-watchdog  # nu trebuie să returneze nimic

# Opțional: șterge fișierele (doar dacă se renunță definitiv)
# rm ~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist
# rm ~/.openclaw/scripts/watchdog-gateway2.sh
```

---

## 4. Cortex Logging

```json
{
  "text": "Gateway 2 Watchdog: GW1 health check [OK/DOWN]. fail_count=[N]. alerted=[true/false].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "watchdog_status",
    "procedure": "GATEWAY2-WATCHDOG-SETUP",
    "rule_id": "OPS-H-001",
    "tags": ["watchdog", "gateway", "monitoring", "openclaw", "genie"]
  }
}
```

---

## 5. Enforcement Loop (META-H-002)

### WHERE
- `com.genie.openclaw-watchdog` LaunchAgent — daemon persistent pe MacIntel
- Manual: Genie verifică la session start că LaunchAgent e activ (GENIE-SUPERVISOR-SESSION-CHECKLIST Pas 1)

### WHEN
- Continuu: la fiecare 60 secunde când MacIntel e pornit
- La restart sistem: RunAtLoad=true → pornire automată

### HOW (violation detection)
- `launchctl list com.genie.openclaw-watchdog` returnează error → watchdog oprit → violation OPS-H-001
- `watchdog.log` mai vechi de 2 minute → watchdog nu rulează → alertă
- GW1 down > 2 minute fără alertă Telegram → watchdog nefuncțional → violation SEC-H-006
- Runner: GENIE-SUPERVISOR-SESSION-CHECKLIST verifică GW1 la Pas 1 (independent de watchdog)

### CONNECT
- `OPS-H-001` → autonomie tehnică: watchdog rulează fără intervenție Pafi
- `SEC-H-006` → Genie guardian: detectarea downtime GW1 e obligatorie
- `OPENCLAW-STARTUP.md` → apelat de Genie dacă GW1 confirmă down
- `AGENT-HEALTH-MONITORING.md` → complementar (5 min check vs 60s watchdog)
- `GENIE-SUPERVISOR-SESSION-CHECKLIST.md` → Pas 1 verifică GW1 independent
- `procedure-health.json` → entry `"GATEWAY2-WATCHDOG-SETUP": "active"`

### VERIFY
La verificarea configurației:
- [ ] `launchctl list com.genie.openclaw-watchdog` returnează PID activ?
- [ ] `watchdog.log` există și are entries recente (< 2 min)?
- [ ] `watchdog-state.json` există și e valid JSON?
- [ ] Token Telegram accesibil din keychain?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → watchdog NU e activ

**VK-uri obligatorii**:
1. `✅ [PROC] GATEWAY2-WATCHDOG-SETUP | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Gateway 2 Watchdog Setup" | FORGE ✓ | rule: OPS-H-001 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Watchdog script (bash) | Script bash (fără LLM) | Check simplu, 60s, cost zero |
| Diagnosticare downtime | Genie (Sonnet) | Decizie escalare/recovery |
| Root cause analiză | Opus subagent | Dacă downtime pattern persistent |

---

## 6. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `watchdog-gateway2.sh` | Script de monitoring | `~/.openclaw/scripts/watchdog-gateway2.sh` |
| `watchdog.json` | Configurare | `~/.openclaw/watchdog.json` |
| `watchdog-state.json` | Persistență stare | `~/.openclaw/watchdog-state.json` |
| LaunchAgent | Scheduling 60s | `~/Library/LaunchAgents/com.genie.openclaw-watchdog.plist` |
| GW1 health endpoint | Target monitorizat | `http://localhost:18789/health` |
| `@Claudeautomationbot` | Alertare Telegram | MacM4 relay, chat ID: 623593648 |
| `TELEGRAM_BOT_TOKEN` | Auth alertare | macOS Keychain |

---

## 7. Metrics

| Metrică | Ce măsoară | Target | Cum se măsoară | Perioadă |
|---------|-----------|--------|---------------|----------|
| MTTD GW1 downtime | De la cădere la alertă Telegram | < 2 minute | `last_fail` timestamp din `watchdog-state.json` vs timestamp alertă Telegram (API `getUpdates`) | Per incident |
| False positive rate | Alerte fără downtime real | < 1% | Nr alerte cu GW1 efectiv OK / nr total alerte. Verificare: log entries cu FAIL urmat de OK în < 60s | Lunar |
| Watchdog uptime | % timp cu daemon activ | > 99.9% | Gap-uri > 2 min între entries consecutive în `watchdog.log`. Calcul: `(timp total - timp gaps) / timp total * 100` | Săptămânal |
| Recovery detection | Alertă de recovery trimisă după rezolvare | 100% | Fiecare incident DOWN trebuie urmat de mesaj RECOVERED pe Telegram. Audit lunar din log | Lunar |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — documentează infrastructura creată de Codex batch 040 (2026-02-26) |
| 1.1 | 2026-03-04 | FORGE-AUDIT: +setup from scratch (3B), +troubleshooting (3C), +rollback (3D), launchctl bootstrap modern, metrics cu metodă de măsurare și perioadă |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, SEC-H-006)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Infrastructura există și funcționează (Codex batch 040 + launchctl verified)
- [x] Salvat în Cortex cu metadata FORGE
