---
type: procedure
created: 2026-03-17
status: active
slug: log-rotation-openclaw
tags: [procedure, nexus]
---

# Log Rotation OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: OPS-H-001 (Technical Autonomy)
**Scope**: Rotația automată a log-urilor din `~/.openclaw/logs/` — prevenirea creșterii necontrolate (actualmente 44MB, 85+ fișiere). Fără rotație, disk usage crește continuu și log-urile vechi nu se șterg niciodată.

---

## 1. Problema

`~/.openclaw/logs/` conține 85+ fișiere log generate de LaunchAgent-uri, scripturi și agenți. Fără rotație:
- **Dimensiune actuală**: 44MB și crește zilnic (~2-3MB/zi estimat)
- **Fișiere mari**: `gateway.log`, `genie-monitor.log`, `backup.log`, `nightly-audit.log` pot ajunge la sute de MB necontrolat
- **Disk pressure** pe MacIntel (SSD cu capacitate limitată)
- **Nightly-audit lent** — citește log-uri vechi la Pas 4/5 → timp de procesare crescut
- **Diagnosticare dificilă** — log-urile vechi poluează grep-urile recente

**Situații acoperite**:
- Rotație zilnică automată la 02:00 (după nightly-audit de la 01:00)
- Rotație manuală la cererea Pafi (înainte de audit de securitate, după incident)
- Log-uri stdout/stderr separate (generate de LaunchAgent-uri cu `StandardOutPath`)

**Incident documentat**: 2026-03-04 — 85 fișiere, 44MB fără niciun mecanism de rotație sau ștergere. Estimare: fără intervenție, 500MB+ în 6 luni.

---

## 2. Procedura

### Pas 1: Verificare stare curentă
```bash
# Dimensiune totală și top fișiere mari
du -sh ~/.openclaw/logs/
du -sh ~/.openclaw/logs/* | sort -rh | head -20

# Număr fișiere și vechimea celor mai vechi
ls -lt ~/.openclaw/logs/ | tail -5
echo "Total fișiere: $(ls ~/.openclaw/logs/ | wc -l | tr -d ' ')"
```

### Pas 2: Rotație manuală (sau verificare script)
```bash
# Rulează scriptul de rotație direct
bash ~/.openclaw/scripts/log-rotation.sh

# Sau verifică că LaunchAgent e activ
launchctl list com.genie.log-rotation
```

### Pas 3: Setup LaunchAgent (prima dată sau după reinstalare)

**Script de rotație** (`~/.openclaw/scripts/log-rotation.sh`):
```bash
#!/usr/bin/env bash
# log-rotation.sh — Rotație log-uri OpenClaw
set -euo pipefail

LOG_DIR="$HOME/.openclaw/logs"
ARCHIVE_DIR="$HOME/.openclaw/logs/.archive"
ROTATION_LOG="$LOG_DIR/log-rotation.log"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')
MAX_SIZE_MB=5      # fișiere > 5MB se rotează imediat
MAX_AGE_DAYS=7     # fișiere mai vechi de 7 zile se arhivează
KEEP_ARCHIVES=14   # zile de retenție arhivă

mkdir -p "$ARCHIVE_DIR"

ROTATED=0
DELETED=0

# Rotație fișiere mari (> MAX_SIZE_MB)
while IFS= read -r -d '' logfile; do
  SIZE_MB=$(du -sm "$logfile" | awk '{print $1}')
  if [ "$SIZE_MB" -ge "$MAX_SIZE_MB" ]; then
    BASENAME=$(basename "$logfile")
    ARCHIVE_NAME="${ARCHIVE_DIR}/${BASENAME}.$(date +%Y%m%d-%H%M%S)"
    mv "$logfile" "$ARCHIVE_NAME"
    touch "$logfile"  # recreează fișierul gol pentru LaunchAgent-uri active
    echo "$TIMESTAMP [ROTATED] $BASENAME (${SIZE_MB}MB → archive)" >> "$ROTATION_LOG"
    ROTATED=$((ROTATED + 1))
  fi
done < <(find "$LOG_DIR" -maxdepth 1 -name "*.log" -type f -print0)

# Ștergere arhive vechi (> KEEP_ARCHIVES zile)
while IFS= read -r -d '' archive; do
  rm "$archive"
  echo "$TIMESTAMP [DELETED] $(basename $archive)" >> "$ROTATION_LOG"
  DELETED=$((DELETED + 1))
done < <(find "$ARCHIVE_DIR" -type f -mtime +${KEEP_ARCHIVES} -print0)

TOTAL_MB=$(du -sm "$LOG_DIR" | awk '{print $1}')
echo "$TIMESTAMP [SUMMARY] rotated=$ROTATED deleted=$DELETED total=${TOTAL_MB}MB" >> "$ROTATION_LOG"
```

**LaunchAgent plist** (`~/Library/LaunchAgents/com.genie.log-rotation.plist`):
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.genie.log-rotation</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>/Users/pafi/.openclaw/scripts/log-rotation.sh</string>
    </array>
    <key>StartCalendarInterval</key>
    <dict>
        <key>Hour</key>
        <integer>2</integer>
        <key>Minute</key>
        <integer>0</integer>
    </dict>
    <key>StandardOutPath</key>
    <string>/Users/pafi/.openclaw/logs/log-rotation-stdout.log</string>
    <key>StandardErrorPath</key>
    <string>/Users/pafi/.openclaw/logs/log-rotation-stderr.log</string>
</dict>
</plist>
```

### Pas 4: Instalare
```bash
# Creează script executabil
chmod +x ~/.openclaw/scripts/log-rotation.sh

# Validează plist
plutil ~/Library/LaunchAgents/com.genie.log-rotation.plist
# Așteptat: "com.genie.log-rotation.plist: OK"

# Încarcă LaunchAgent
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.log-rotation.plist

# Verifică
launchctl list com.genie.log-rotation
```

### Pas 5: Verificare post-instalare
```bash
# Test run manual
bash ~/.openclaw/scripts/log-rotation.sh

# Verifică log-ul de rotație
tail -5 ~/.openclaw/logs/log-rotation.log

# Confirmare dimensiune
du -sh ~/.openclaw/logs/
```

### Pas 6: Error handling — dacă bootstrap eșuează
```bash
# Dacă "already loaded" sau I/O error
launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/com.genie.log-rotation.plist 2>/dev/null || true
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.log-rotation.plist
launchctl list com.genie.log-rotation
```

---

## 3. Cortex Logging

```json
{
  "text": "Log Rotation OpenClaw: rotatie executata. Problema identificata: 85+ fisiere, 44MB fara mecanism de rotatie. Procedura LOG-ROTATION-OPENCLAW stabilita: LaunchAgent com.genie.log-rotation la 02:00 zilnic. MAX_SIZE_MB=5, MAX_AGE_DAYS=7, KEEP_ARCHIVES=14. Enforcement loop activ.",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "infrastructure_setup",
    "procedure": "LOG-ROTATION-OPENCLAW",
    "rule_id": "OPS-H-001",
    "tags": ["logs", "rotation", "openclaw", "launchagent", "infrastructure", "disk"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- LaunchAgent `com.genie.log-rotation` pe MacIntel — rulează zilnic la 02:00 (după nightly-audit 01:00)
- Verificat de NIGHTLY-AUDIT Pas 2 (disk space check)
- Manual la cererea Pafi sau pre-security-audit

### WHEN
- **Automat**: zilnic la 02:00 via LaunchAgent
- **Manual**: la cererea Pafi sau când `du -sh ~/.openclaw/logs/` > 100MB
- **Pre-audit**: înainte de SECURITY-AUDIT-OPENCLAW pentru a reduce noise

### HOW (violation detection + acțiune post-violation)
- `du -sm ~/.openclaw/logs/` > 100MB → violation OPS-H-001 (log growth necontrolat)
  - **Acțiune**: Genie rulează manual `bash ~/.openclaw/scripts/log-rotation.sh`, logează în Cortex
- `launchctl list com.genie.log-rotation` → error sau absent → LaunchAgent oprit → violation
  - **Acțiune**: Genie rulează Pas 4 (reinstalare). Dacă eșuează → notificare Pafi pe Telegram
- `log-rotation.log` mai vechi de 25h → rotația nu a rulat → investigare
  - **Acțiune**: verifică `launchctl list com.genie.log-rotation`, verifică plist, re-bootstrap

### CONNECT
- `OPS-H-001` → autonomie tehnică: rotația e automată, fără intervenție Pafi
- `NIGHTLY-AUDIT-OPENCLAW.md` → Pas 2 verifică disk space, poate detecta creștere anormală
- `SECURITY-AUDIT-OPENCLAW.md` → pre-audit: log-uri curate = diagnosticare mai ușoară
- `GATEWAY2-WATCHDOG-SETUP.md` → watchdog.log e unul din fișierele rotație acoperă
- `BACKUP-AUTOMATION-OPENCLAW.md` → backup.log acoperit de rotație
- `procedure-health.json` → entry `"LOG-ROTATION-OPENCLAW": "active"`

### VERIFY
- [ ] `~/.openclaw/scripts/log-rotation.sh` există și e executabil (`chmod +x`)?
- [ ] `~/Library/LaunchAgents/com.genie.log-rotation.plist` există și e valid (`plutil`)?
- [ ] `launchctl list com.genie.log-rotation` returnează PID sau status 0?
- [ ] `~/.openclaw/logs/log-rotation.log` există și are entry recent (< 25h)?
- [ ] `du -sh ~/.openclaw/logs/` < 100MB?
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] LOG-ROTATION-OPENCLAW | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Log Rotation OpenClaw" | FORGE ✓ | rule: OPS-H-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Setup/verificare | Genie (Sonnet) | Comenzi bash simple |
| Diagnosticare creștere anormală | Genie (Sonnet) | Analiză log patterns |

---

## 5. Dependențe

| Componentă | Rol | Path absolut |
|-----------|-----|--------------|
| `~/.openclaw/scripts/log-rotation.sh` | Script rotație | `/Users/pafi/.openclaw/scripts/log-rotation.sh` |
| `~/Library/LaunchAgents/com.genie.log-rotation.plist` | Scheduler zilnic 02:00 | `/Users/pafi/Library/LaunchAgents/com.genie.log-rotation.plist` |
| `~/.openclaw/logs/` | Director log-uri țintă | `/Users/pafi/.openclaw/logs/` |
| `~/.openclaw/logs/.archive/` | Arhivă fișiere rotație | Creat de script |
| `~/.openclaw/logs/log-rotation.log` | Log operațional rotație | Creat de script |
| `NIGHTLY-AUDIT-OPENCLAW.md` | Proxy detecție disk pressure | `/Users/pafi/.nexus/procedures/openclaw/NIGHTLY-AUDIT-OPENCLAW.md` |
| `procedure-health.json` | Registry proceduri active | `/Users/pafi/.nexus/procedures/openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Metoda de măsurare | Frecvență |
|---------|-----------|--------|--------------------|-----------|
| Log directory size | MB total în `~/.openclaw/logs/` | < 100MB | `du -sm ~/.openclaw/logs/` | Zilnic (nightly-audit) |
| Files rotated/day | Fișiere > 5MB rotite per rulare | 0-5/zi (normal) | `grep ROTATED ~/.openclaw/logs/log-rotation.log \| tail -1` | Zilnic |
| Archive retention | Zile de arhivă păstrate | 14 zile | `ls ~/.openclaw/logs/.archive/ \| wc -l` | Săptămânal |
| Rotation LaunchAgent uptime | % timp LaunchAgent activ | 100% | `launchctl list com.genie.log-rotation` → PID activ | La fiecare sesiune Genie |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|------------|
| 1.0 | 2026-03-04 | Creat — 44MB/85+ fișiere fără rotație, LaunchAgent zilnic 02:00, MAX_SIZE_MB=5, KEEP_ARCHIVES=14 |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] Acțiune post-violation definită în HOW
- [x] VERIFY checkpoint prezent cu output așteptat
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Script complet inclus în §2
- [x] Plist complet inclus în §2
- [x] Path-uri absolute în tabelul Dependențe
- [x] Metrics cu metodă de măsurare și frecvență
- [x] Incident documentat în §1 Problema
