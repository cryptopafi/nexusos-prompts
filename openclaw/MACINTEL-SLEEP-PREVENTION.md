---
type: procedure
created: 2026-03-17
status: active
slug: macintel-sleep-prevention
tags: [procedure, nexus]
---

# MacIntel Sleep Prevention — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.2
**Regulă asociată**: OPS-H-001 (Technical Autonomy)
**Scope**: Prevenirea sleep-ului MacIntel în timp ce OpenClaw e activ — toate LaunchAgent-urile se opresc la sleep, deci monitorizarea și backup-urile sunt suspendate.

---

## 1. Problema

macOS pune MacIntel în sleep după inactivitate (configurat: `sleep 1`, `displaysleep 5`). Efecte:
- **Toate LaunchAgent-urile se suspendă** — watchdog, backup, health-monitor, nightly-audit nu rulează
- **Gap de monitoring** — MTTD crește de la minute la ore
- **Backup-urile se întrerup** — posibil gap de > 15 min în retenție
- **Nightly audit ratat** — dacă MacIntel doarme la 01:00 EET
- **caffeinate -i -s** rulează manual din Duminică (PID 1217) dar nu e persistent la restart

**Situații acoperite**:
- OpenClaw activ pe MacIntel (indiferent dacă agenții sunt în sesiune sau nu)
- Sesiuni lungi fără activitate user (noapte, weekend)
- Restart MacIntel — caffeinate trebuie să repornească automat

**Incident documentat**: 2026-03-02 (duminică) — caffeinate manual PID 1217 nu era persistent. La un potențial restart, nightly-audit de la 01:00 EET nu ar fi rulat, creând un gap de monitoring de ~7 ore (01:00–08:00). Backup-urile ar fi acumulat un gap de >15 min în retenție. Remedierea a fost formalizarea ca LaunchAgent persistent.

---

## 2. Componente

### Soluție implementată: caffeinate via LaunchAgent

```xml
<!-- ~/Library/LaunchAgents/com.genie.sleep-prevention.plist -->
Label: com.genie.sleep-prevention
ProgramArguments: ["/usr/bin/caffeinate", "-i", "-s"]
RunAtLoad: true
KeepAlive: true
```

**Flaguri caffeinate**:
- `-i` — previne idle sleep (system idle sleep assertion)
- `-s` — previne system sleep (AC power sleep assertion)

### Verificare stare curentă
```bash
# Verifică dacă caffeinate rulează
ps aux | grep "caffeinate -i -s" | grep -v grep

# Verifică LaunchAgent activ
launchctl list com.genie.sleep-prevention

# Verifică motivul sleep-prevention
pmset -g | grep sleep
# Așteptat: "sleep prevented by caffeinate, caffeinate"
```

---

## 3. Procedura de Setup

### Pas 1: Creează LaunchAgent plist
```bash
cat > ~/Library/LaunchAgents/com.genie.sleep-prevention.plist << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.genie.sleep-prevention</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/caffeinate</string>
        <string>-i</string>
        <string>-s</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>KeepAlive</key>
    <true/>
</dict>
</plist>
EOF
```

### Pas 1b: Validează plist-ul
```bash
plutil ~/Library/LaunchAgents/com.genie.sleep-prevention.plist
# Așteptat: "com.genie.sleep-prevention.plist: OK"
# Dacă eroare → corectează XML-ul înainte de Pas 2
```

### Pas 2: Încarcă LaunchAgent
```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.sleep-prevention.plist

# Verifică
launchctl list com.genie.sleep-prevention
pmset -g | grep sleep
```

### Pas 3: Dacă caffeinate manual deja rulează (PID vechi)
```bash
# Extrage și oprește PID-ul vechi (LaunchAgent va porni unul nou imediat)
OLD_PID=$(ps aux | grep "caffeinate -i -s" | grep -v grep | awk '{print $2}')
if [ -n "$OLD_PID" ]; then
  kill "$OLD_PID"
  echo "Oprit caffeinate PID $OLD_PID"
else
  echo "Niciun caffeinate manual activ — skip"
fi
```

### Pas 4: Verificare finală post-setup (OBLIGATORIU)
```bash
# Confirmă că sleep este efectiv prevenit — nu doar că procesul rulează
pmset -g assertions | grep -E "PreventUserIdleSystemSleep|PreventSystemSleep"
# Așteptat: linii cu "caffeinate" și assertion activ (Level=1 sau YES)

# Verifică PID-ul nou al LaunchAgent
launchctl list com.genie.sleep-prevention
# Așteptat: linie cu PID numeric (ex: 1342), status 0

# Confirmare finală
pmset -g | grep sleep
# Așteptat: "sleep prevented by caffeinate, caffeinate"
```

### Pas 5: Error handling — dacă `launchctl bootstrap` eșuează
```bash
# Dacă primești "Bootstrap failed: 5: Input/output error" sau "already loaded"
# Step 1: Descarcă mai întâi
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.genie.sleep-prevention.plist 2>/dev/null || true

# Step 2: Reîncearcă bootstrap
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.sleep-prevention.plist

# Step 3: Verifică
launchctl list com.genie.sleep-prevention
```

---

## 4. Dezactivare (când OpenClaw e hibernat)

Când OpenClaw intră în HIBERNATING și nu mai necesită monitoring continuu:

```bash
# Oprește sleep prevention
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.genie.sleep-prevention.plist

# Verifică că s-a oprit
ps aux | grep caffeinate | grep -v grep
pmset -g | grep sleep  # nu mai trebuie "sleep prevented by caffeinate"
```

⚠️ **Repornire la reactivare OpenClaw**: la OPENCLAW-STARTUP, verifică că sleep prevention e activ (Pas 1 din această procedură).

---

## 5. Cortex Logging

```json
{
  "text": "MacIntel Sleep Prevention: LaunchAgent com.genie.sleep-prevention [ACTIV/OPRIT]. caffeinate -i -s [rulează/nu rulează]. pmset sleep prevented by caffeinate: [DA/NU].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "infrastructure_status",
    "procedure": "MACINTEL-SLEEP-PREVENTION",
    "rule_id": "OPS-H-001",
    "tags": ["sleep", "caffeinate", "macintel", "launchagent", "openclaw", "infrastructure"]
  }
}
```

---

## 6. Enforcement Loop (META-H-002)

### WHERE
- `com.genie.sleep-prevention` LaunchAgent pe MacIntel — pornit la login, persistent
- Verificat de GENIE-SUPERVISOR-SESSION-CHECKLIST Pas 1 (GW1 check implică că sistemul nu doarme)
- Verificat de OPENCLAW-STARTUP înainte de activare

### WHEN
- Permanent cât timp OpenClaw e **ACTIVE** (nu HIBERNATING)
- La fiecare restart MacIntel — RunAtLoad=true asigură repornire automată
- Dezactivat explicit la OPENCLAW-HIBERNATE

### HOW (violation detection + acțiune post-violation)
- `pmset -g | grep sleep` nu conține "sleep prevented by caffeinate" → sleep prevention inactiv → violation OPS-H-001
  - **Acțiune**: Genie rulează imediat Pas 2 din §3 (bootstrap LaunchAgent). Dacă eșuează → Pas 5 (error handling). Dacă tot eșuează → Genie notifică Pafi pe Telegram cu mesaj: `[ALERT] MACINTEL-SLEEP-PREVENTION violation: caffeinate inactiv. Intervenție manuală necesară.`
- `launchctl list com.genie.sleep-prevention` → error sau PID absent → LaunchAgent oprit → violation OPS-H-001
  - **Acțiune**: Genie rulează `launchctl bootstrap gui/$(id -u) ~/.nexus/... ` (Pas 2). Logează în Cortex collection `sessions` cu type `violation_recovery`.
- NIGHTLY-AUDIT ratat (log mai vechi de 24h) → posibil sleep gap → investigează
  - **Acțiune**: Genie deschide subagent Opus pentru diagnosticare gap monitoring. Escaladare la Pafi dacă gap > 2h.

### CONNECT
- `OPS-H-001` → autonomie tehnică: LaunchAgent-urile funcționează fără intervenție Pafi
- `OPENCLAW-STARTUP.md` → adaugă verificare sleep prevention ca Pas 0
- `OPENCLAW-HIBERNATE.md` → dezactivează sleep prevention la hibernare
- `GENIE-SUPERVISOR-SESSION-CHECKLIST.md` → Pas 1 poate include pmset check
- `NIGHTLY-AUDIT-OPENCLAW.md` → audit ratat = posibil sleep gap
- `procedure-health.json` → entry `"MACINTEL-SLEEP-PREVENTION": "active"`

### VERIFY
- [ ] `launchctl list com.genie.sleep-prevention` returnează PID activ? (output așteptat: `1234\t0\tcom.genie.sleep-prevention`)
- [ ] `pmset -g | grep sleep` conține "sleep prevented by caffeinate"? (output așteptat: `sleep prevented by caffeinate, caffeinate`)
- [ ] `pmset -g assertions | grep PreventUserIdleSystemSleep` returnează assertion activ? (output așteptat: linie cu `caffeinate` și Level=1)
- [ ] Plist există la `/Users/pafi/Library/LaunchAgents/com.genie.sleep-prevention.plist`?
- [ ] La restart MacIntel: caffeinate repornit automat fără intervenție? (verificare: `RunAtLoad: true` în plist)
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] MACINTEL-SLEEP-PREVENTION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "MacIntel Sleep Prevention" | FORGE ✓ | rule: OPS-H-001 | v1.2`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Setup/verificare | Genie (Sonnet) | Comenzi bash simple |
| Diagnosticare gap monitoring | Opus subagent | Dacă audit gaps detectate |

---

## 7. Dependențe

| Componentă | Rol | Path absolut |
|-----------|-----|--------------|
| `/usr/bin/caffeinate` | Prevent sleep | `/usr/bin/caffeinate` (macOS built-in, verificat: `which caffeinate`) |
| LaunchAgent plist | Persistență la restart | `/Users/pafi/Library/LaunchAgents/com.genie.sleep-prevention.plist` |
| `/usr/bin/pmset` | Verificare stare power | `/usr/bin/pmset` (macOS built-in) |
| `/usr/bin/launchctl` | Gestiune LaunchAgent | `/usr/bin/launchctl` (macOS built-in) |
| `OPENCLAW-HIBERNATE.md` | Dezactivare controlată | `/Users/pafi/.nexus/procedures/openclaw/OPENCLAW-HIBERNATE.md` |
| `OPENCLAW-STARTUP.md` | Activare la startup | `/Users/pafi/.nexus/procedures/openclaw/OPENCLAW-STARTUP.md` |
| `GENIE-SUPERVISOR-SESSION-CHECKLIST.md` | Verificare sleep prevention la GW1 | `/Users/pafi/.nexus/procedures/openclaw/GENIE-SUPERVISOR-SESSION-CHECKLIST.md` |
| `NIGHTLY-AUDIT-OPENCLAW.md` | Proxy detecție sleep gaps | `/Users/pafi/.nexus/procedures/openclaw/NIGHTLY-AUDIT-OPENCLAW.md` |
| `procedure-health.json` | Registry stare proceduri | `/Users/pafi/.nexus/monitoring/procedure-health.json` |

---

## 8. Metrics

| Metrică | Ce măsoară | Target | Metoda de măsurare | Frecvență |
|---------|-----------|--------|--------------------|-----------|
| Sleep prevention uptime | % timp cu caffeinate activ (cât timp OpenClaw e ACTIVE) | 100% | `launchctl list com.genie.sleep-prevention` → PID activ = uptime. Deviere = violation. | La fiecare sesiune GENIE (GW1 check) |
| Nightly audit success rate | % nopți cu audit completat (proxy pentru sleep gaps) | > 95% | Verifică timestamp ultimul log NIGHTLY-AUDIT: `ls -la /Users/pafi/.nexus/logs/nightly-audit-*.log | tail -1` — timestamp < 24h = success | Zilnic, la GENIE startup |
| Restart recovery time | Timp de la restart MacIntel la caffeinate activ | < 30 secunde (RunAtLoad=true) | Verificat post-restart: `uptime` + `ps aux | grep caffeinate` — dacă PID activ în primele 30s = OK | La fiecare restart MacIntel |
| Violation frequency | Număr violări OPS-H-001 detectate | 0/săptămână | Count intrări Cortex cu `type: violation_recovery` și `procedure: MACINTEL-SLEEP-PREVENTION` în ultimele 7 zile | Săptămânal (luni dimineața) |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — formalizează caffeinate manual (PID 1217, din Duminică) ca LaunchAgent persistent |
| 1.1 | 2026-03-04 | FORGE-AUDIT fix: +Pas 1b validare plutil, typo fix §1, +2 dependențe lipsă (GENIE-SUPERVISOR, NIGHTLY-AUDIT) |
| 1.2 | 2026-03-04 | FORGE-AUDIT complet D2-D6: +Pas 3 PID extraction explicit, +Pas 4 verificare finală post-setup, +Pas 5 error handling bootstrap, +acțiune post-violation în HOW (D3), +output așteptat în VERIFY (D3), +path-uri absolute în Dependențe (D5), +procedure-health.json în Dependențe (D5), +metoda de măsurare și frecvență în Metrics (D4), +incident documentat în Problemă (D1), +VK versiune corectată v1.2, +checklist Pre-Publicare extins (D6) |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] Acțiune post-violation definită în HOW (nu doar detecție)
- [x] VERIFY checkpoint prezent cu output așteptat explicit
- [x] Descrie CE nu CUM
- [x] VK format specificat cu versiune corectă (v1.2)
- [x] Entry adăugat în `procedure-health.json`
- [x] Infrastructura: caffeinate deja rulează (PID 1217), LaunchAgent de creat
- [x] Salvat în Cortex cu metadata FORGE
- [x] Path-uri absolute (nu `~/`) în tabelul Dependențe
- [x] Metrics au metodă de măsurare și frecvență definite
- [x] Error handling definit pentru pașii critici (Pas 5)
- [x] Incident concret documentat în §1 Problema
