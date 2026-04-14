---
type: procedure
created: 2026-03-17
status: active
slug: richard-hourly-report
tags: [procedure, nexus]
---

# Richard Hourly Report — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: OPS-H-001 (Technical Autonomy), MON-H-001 (Monitoring Continuity)
**Scope**: Raportul orar de stare Richard → Telegram, trimis de `rich-hourly-report.sh` via LaunchAgent `ai.openclaw.rich-hourly-report`. Documentează infrastructura existentă, setup-ul, verificarea stării și recovery-ul.

---

## 1. Problema

Richard (agentul principal OpenClaw) rulează continuu pe MacIntel. Fără un raport periodic de stare:
- **Pafi nu știe dacă Gateway e DOWN** între sesiunile active — MTTD (mean time to detect) poate fi ore
- **Incidente silențioase** — restarturi, warnings, critice din `genie-monitor.log` rămân nevăzute
- **Diagnostic întârziat** — la sesiunea Genie se pierde timp cu diagnosticarea situației

**Infrastructura existentă** (creată în Codex batch 040):
- Script: `~/.openclaw/scripts/rich-hourly-report.sh` — Gateway health + events din ultimele 60 min
- LaunchAgent: `~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist` — StartInterval=3600
- Log: `~/.openclaw/logs/rich-hourly-report.log` + `rich-hourly-report-launchd.log`
- Token: Keychain `telegram-bot-claudeautomationbot` → chat `623593648`

**Situații acoperite**:
- Raport normal (Gateway UP, fără erori) → mesaj concis ✅
- Raport cu incidente (Gateway DOWN, restarts, critice/warnings) → detalii complete
- Token Keychain lipsă → script exit cu eroare logată
- LaunchAgent oprit → recovery manual

---

## 2. Procedura

### Pas 1: Verificare stare curentă
```bash
# Verifică că LaunchAgent e activ
launchctl list ai.openclaw.rich-hourly-report
# Așteptat: linie cu PID (sau "-") și status 0

# Ultimele rapoarte trimise
tail -5 ~/.openclaw/logs/rich-hourly-report.log
# Așteptat: linii cu [SENT] gw=UP critical=0 warning=0 restart=0

# Verifică token Keychain
security find-generic-password -a "telegram-bot-claudeautomationbot" -s "telegram-token" -w 2>/dev/null && echo "TOKEN OK" || echo "TOKEN MISSING"
```

### Pas 2: Test manual
```bash
# Rulează raportul manual (fără să aștepți ora)
bash ~/.openclaw/scripts/rich-hourly-report.sh

# Verifică log
tail -1 ~/.openclaw/logs/rich-hourly-report.log
# Așteptat: 2026-03-04 14:00:01 [SENT] gw=UP critical=0 warning=0 restart=0
```

### Pas 3: Setup complet (reinstalare sau prima dată)

**Verifică că scriptul există**:
```bash
ls -la ~/.openclaw/scripts/rich-hourly-report.sh
chmod +x ~/.openclaw/scripts/rich-hourly-report.sh
```

**Creează plist dacă lipsește**:
```bash
cat > ~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>ai.openclaw.rich-hourly-report</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>/Users/pafi/.openclaw/scripts/rich-hourly-report.sh</string>
    </array>
    <key>StartInterval</key>
    <integer>3600</integer>
    <key>RunAtLoad</key>
    <true/>
    <key>StandardOutPath</key>
    <string>/Users/pafi/.openclaw/logs/rich-hourly-report-launchd.log</string>
    <key>StandardErrorPath</key>
    <string>/Users/pafi/.openclaw/logs/rich-hourly-report-launchd.log</string>
</dict>
</plist>
EOF

# Validează
plutil ~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist
# Așteptat: "ai.openclaw.rich-hourly-report.plist: OK"
```

**Încarcă LaunchAgent**:
```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist

# Verifică
launchctl list ai.openclaw.rich-hourly-report
```

### Pas 4: Adăugare/actualizare token Keychain (dacă lipsește)
```bash
# Adaugă token în Keychain (înlocuiește TOKEN cu valoarea reală)
security add-generic-password \
  -a "telegram-bot-claudeautomationbot" \
  -s "telegram-token" \
  -w "TOKEN" \
  -U  # -U = update dacă există deja

# Verifică
security find-generic-password -a "telegram-bot-claudeautomationbot" -s "telegram-token" -w 2>/dev/null && echo "OK"
```

### Pas 5: Error handling — dacă bootstrap eșuează
```bash
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist 2>/dev/null || true
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist
launchctl list ai.openclaw.rich-hourly-report
```

### Pas 6: Dezactivare (la OPENCLAW-HIBERNATE)
```bash
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist
# Verifică că s-a oprit
launchctl list ai.openclaw.rich-hourly-report 2>&1 | grep -i "not found" && echo "STOPPED" || echo "STILL RUNNING"
```

---

## 3. Cortex Logging

```json
{
  "text": "Richard Hourly Report: LaunchAgent ai.openclaw.rich-hourly-report [ACTIV/OPRIT]. Ultimul raport: [timestamp] gw=[UP/DOWN] critical=[N] warning=[N] restart=[N]. Token Keychain: [OK/MISSING].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "infrastructure_status",
    "procedure": "RICHARD-HOURLY-REPORT",
    "rule_id": "OPS-H-001",
    "tags": ["richard", "hourly-report", "telegram", "monitoring", "openclaw", "launchagent"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- LaunchAgent `ai.openclaw.rich-hourly-report` pe MacIntel — StartInterval=3600, RunAtLoad=true
- Verificat de GENIE-SUPERVISOR-SESSION-CHECKLIST Pas 1 (GW1 check implică că raportul funcționează)
- Verificat de OPENCLAW-STARTUP înainte de activare

### WHEN
- **Permanent** cât timp OpenClaw e ACTIVE (nu HIBERNATING)
- **Dezactivat** la OPENCLAW-HIBERNATE (Pas 3 §2), reactivat la Wake (Pas 4 §3)
- **Nu are limită** — rulează la fiecare 3600 secunde indiferent de activitate

### HOW (violation detection + acțiune post-violation)
- `launchctl list ai.openclaw.rich-hourly-report` → error sau absent → LaunchAgent oprit → violation MON-H-001
  - **Acțiune**: Genie rulează Pas 3 §2 (reinstalare). Dacă eșuează → Pas 5 (error handling). Dacă tot eșuează → notificare Pafi pe Telegram
- `tail -1 ~/.openclaw/logs/rich-hourly-report.log` → timestamp > 2h față de acum → raportul nu s-a trimis → violation
  - **Acțiune**: Genie testează manual Pas 2, verifică token Keychain (Pas 4), verifică LaunchAgent status
- `rich-hourly-report.log` conține `[ERROR] No Telegram token` → token Keychain lipsă → violation
  - **Acțiune**: Genie verifică token cu Pafi și rulează Pas 4 pentru re-adăugare

### CONNECT
- `OPS-H-001` → autonomie: raportul rulează fără intervenție Pafi
- `MON-H-001` → monitoring continuity: Pafi primește status la fiecare oră
- `OPENCLAW-HIBERNATE.md` → dezactivat la Hibernate, reactivat la Wake
- `OPENCLAW-STARTUP.md` → verificat la startup că LaunchAgent e activ
- `GENIE-SUPERVISOR-SESSION-CHECKLIST.md` → verificare la sesiune Genie
- `GATEWAY2-WATCHDOG-SETUP.md` → watchdog-ul și hourly report sunt complementare (60s vs 3600s)
- `procedure-health.json` → entry `"RICHARD-HOURLY-REPORT": "active"`

### VERIFY
- [ ] `launchctl list ai.openclaw.rich-hourly-report` returnează status 0? (output: `[PID/"-"]\t0\tai.openclaw.rich-hourly-report`)
- [ ] Token Keychain `telegram-bot-claudeautomationbot` prezent? (`security find-generic-password ... -w` → token string)
- [ ] `~/.openclaw/scripts/rich-hourly-report.sh` executabil? (`ls -la` → `-rwxr-xr-x`)
- [ ] `tail -1 ~/.openclaw/logs/rich-hourly-report.log` → timestamp < 2h? (`[SENT]` linie recentă)
- [ ] Plist există la `/Users/pafi/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist`?
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] RICHARD-HOURLY-REPORT | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Richard Hourly Report" | FORGE ✓ | rule: OPS-H-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Setup/verificare | Genie (Sonnet) | Comenzi bash simple |
| Diagnosticare token/LaunchAgent | Genie (Sonnet) | Troubleshooting standard |

---

## 5. Dependențe

| Componentă | Rol | Path absolut |
|-----------|-----|--------------|
| `~/.openclaw/scripts/rich-hourly-report.sh` | Script raport orar | `/Users/pafi/.openclaw/scripts/rich-hourly-report.sh` |
| `~/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist` | Scheduler orar | `/Users/pafi/Library/LaunchAgents/ai.openclaw.rich-hourly-report.plist` |
| Keychain `telegram-bot-claudeautomationbot` | Token Telegram bot | macOS Keychain (security tool) |
| `~/.openclaw/logs/rich-hourly-report.log` | Log rapoarte trimise | `/Users/pafi/.openclaw/logs/rich-hourly-report.log` |
| `~/.openclaw/logs/genie-monitor.log` | Sursă date (critice/warnings/restarts) | `/Users/pafi/.openclaw/logs/genie-monitor.log` |
| `OPENCLAW-HIBERNATE.md` | Dezactivat la hibernate | `/Users/pafi/.nexus/procedures/openclaw/OPENCLAW-HIBERNATE.md` |
| `GATEWAY2-WATCHDOG-SETUP.md` | Complementar (60s vs 3600s) | `/Users/pafi/.nexus/procedures/openclaw/GATEWAY2-WATCHDOG-SETUP.md` |
| `procedure-health.json` | Registry proceduri active | `/Users/pafi/.nexus/procedures/openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Metoda de măsurare | Frecvență |
|---------|-----------|--------|--------------------|-----------|
| Report delivery rate | % ore cu raport trimis cu succes | > 95% | `grep -c "\[SENT\]" ~/.openclaw/logs/rich-hourly-report.log` vs ore active | Zilnic |
| Token availability | Token Keychain prezent și valid | 100% | `security find-generic-password ... -w` → non-empty | La fiecare sesiune Genie |
| LaunchAgent uptime | % timp LaunchAgent activ | 100% | `launchctl list ai.openclaw.rich-hourly-report` → status 0 | La fiecare sesiune Genie |
| Alert false positive rate | % rapoarte cu warning fals | < 5% | Manual review log săptămânal | Săptămânal |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|------------|
| 1.0 | 2026-03-04 | Creat — formalizează `rich-hourly-report.sh` + LaunchAgent existent (Codex batch 040). Adaugă recovery, Keychain setup, hibernate integration |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, MON-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] Acțiune post-violation definită în HOW (3 scenarii)
- [x] VERIFY checkpoint prezent cu output așteptat explicit
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Infrastructura existentă documentată (script + plist din Codex batch 040)
- [x] Plist complet inclus pentru reinstalare
- [x] Path-uri absolute în tabelul Dependențe
- [x] Metrics cu metodă de măsurare și frecvență
- [x] Dezactivare la Hibernate documentată (Pas 6)
