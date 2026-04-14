---
type: procedure
created: 2026-03-17
status: active
slug: backup-automation-openclaw
tags: [procedure, nexus]
---

# Backup Automation OpenClaw — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.1
**Regulă asociată**: OPS-H-001 (Technical Autonomy), OPS-H-003 (Data Integrity)
**Scope**: Backup automat la fiecare 15 minute al workspace-urilor agenților OpenClaw — prevenirea pierderii de date (SOUL.md, MEMORY.md, sesiuni) în caz de corupție sau incident.

---

## 1. Problema

Workspace-urile agenților conțin date critice care se pot pierde sau corupe:
- **SOUL.md** — identitatea agentului (modificare neautorizată → agent drift)
- **MEMORY.md** — memoria agentului (corupție → pierdere context)
- **Sesiuni** — istoricul conversațiilor (valoros pentru debug și audit)

Fără backup automat:
- Un `chmod 444` accidental sau un bug în script poate face SOUL.md inaccesibil
- Modificarea neautorizată a SOUL.md e detectată de SOUL-MD-IMMUTABILITY dar nu poate fi revertită fără backup
- OpenClaw îngheață sau corupe fișierele la crash

Situații acoperite:
- Corupție workspace agent individual
- Modificare neautorizată SOUL.md (revert rapid la ultima versiune curată)
- Crash OpenClaw cu fișiere incomplete
- Recovery post-incident (SOUL-MD-IMMUTABILITY → are nevoie de backup curat)

---

## 2. Componente

### Fișiere de configurare

| Fișier | Path | Rol |
|--------|------|-----|
| `backup-openclaw.sh` | `~/.openclaw/scripts/backup-openclaw.sh` | Script de backup |
| LaunchAgent | `~/Library/LaunchAgents/com.genie.openclaw-backup.plist` | Scheduling 15 min |
| Backup dir | `~/.openclaw/backups/` | Director cu backup-uri |

### Parametri operaționali

| Parametru | Valoare | Semnificație |
|-----------|---------|-------------|
| Sursă | `~/.openclaw/workspaces/` | Tot conținutul workspace-urilor (6 agenți) |
| Destinație | `~/.openclaw/backups/YYYYMMDD-HHMM/` | Directoare cu timestamp |
| Interval | 900 secunde (15 min) | `StartInterval=900` în LaunchAgent |
| Retenție | 48 backup-uri | ~24 ore acoperire cu granularitate 15 min |
| Metodă | `cp -R` (copy recursiv) | Nu arhivare — acces direct la fișiere |
| Log | `~/.openclaw/logs/backup.log` | Timestamp per backup |

### Logică script (backup-openclaw.sh)

```
1. Creează ~/.openclaw/backups/ și ~/.openclaw/logs/ dacă nu există
2. cp -R ~/.openclaw/workspaces/ ~/.openclaw/backups/YYYYMMDD-HHMM/
3. Șterge backup-urile mai vechi de 48 (ls -1dt | tail -n +49 | rm -rf)
4. Loghează în backup.log: timestamp + path destinație
```

### Script complet (backup-openclaw.sh)

```bash
#!/bin/bash
# backup-openclaw.sh — Backup automat workspaces OpenClaw
# Rulat de LaunchAgent la fiecare 15 minute

BACKUP_DIR="$HOME/.openclaw/backups"
LOG_DIR="$HOME/.openclaw/logs"
SOURCE="$HOME/.openclaw/workspaces"
TIMESTAMP=$(date +%Y%m%d-%H%M)
DEST="$BACKUP_DIR/$TIMESTAMP"
MAX_BACKUPS=48

mkdir -p "$BACKUP_DIR" "$LOG_DIR"

if cp -R "$SOURCE/" "$DEST/"; then
    echo "[$(date)] BACKUP OK: $DEST" >> "$LOG_DIR/backup.log"
else
    echo "[$(date)] BACKUP FAILED: $DEST" >> "$LOG_DIR/backup.log"
    exit 1
fi

# Retenție: păstrează doar ultimele $MAX_BACKUPS
cd "$BACKUP_DIR" && ls -1dt */ 2>/dev/null | tail -n +$((MAX_BACKUPS + 1)) | xargs rm -rf 2>/dev/null

exit 0
```

### LaunchAgent complet (com.genie.openclaw-backup.plist)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.genie.openclaw-backup</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/bash</string>
        <string>~/.openclaw/scripts/backup-openclaw.sh</string>
    </array>
    <key>StartInterval</key>
    <integer>900</integer>
    <key>KeepAlive</key>
    <false/>
    <key>RunAtLoad</key>
    <true/>
    <key>StandardOutPath</key>
    <string>~/.openclaw/logs/backup-stdout.log</string>
    <key>StandardErrorPath</key>
    <string>~/.openclaw/logs/backup-stderr.log</string>
</dict>
</plist>
```

### Setup (instalare de la zero)

```bash
# 1. Creează directoare
mkdir -p ~/.openclaw/scripts ~/.openclaw/backups ~/.openclaw/logs

# 2. Copiază scriptul (sau creează din conținutul de mai sus)
chmod +x ~/.openclaw/scripts/backup-openclaw.sh

# 3. Copiază plist-ul în LaunchAgents
cp com.genie.openclaw-backup.plist ~/Library/LaunchAgents/

# 4. Încarcă LaunchAgent
launchctl load ~/Library/LaunchAgents/com.genie.openclaw-backup.plist

# 5. Verifică
launchctl list com.genie.openclaw-backup
```

### LaunchAgent config

```xml
<key>Label</key><string>com.genie.openclaw-backup</string>
<key>StartInterval</key><integer>900</integer>   <!-- 15 minute -->
<key>KeepAlive</key><false/>                     <!-- task periodic, nu daemon -->
<key>RunAtLoad</key><true/>
```

---

## 3. Procedura de Recovery (Restaurare Backup)

### Pas 1: Identifică backup-ul dorit
```bash
ls -lt ~/.openclaw/backups/ | head -20
# Format: YYYYMMDD-HHMM (ex: 20260304-1400)
```

### Pas 2: Verifică conținutul backup-ului
```bash
BACKUP="20260304-1400"  # înlocuiește cu timestamp dorit
ls -la ~/.openclaw/backups/$BACKUP/
# Verifică că SOUL.md există pentru agentul de interes
cat ~/.openclaw/backups/$BACKUP/rich-new/SOUL.md | head -5
```

### Pas 3: Restaurare agent individual
```bash
AGENT="rich-new"  # sau: finance, marketing, legal, ops, insights
BACKUP="20260304-1400"

# Dezactivează read-only temporar (dacă SOUL.md e chmod 444)
chmod 644 ~/.openclaw/workspaces/$AGENT/SOUL.md

# Restaurează SOUL.md
cp ~/.openclaw/backups/$BACKUP/$AGENT/SOUL.md \
   ~/.openclaw/workspaces/$AGENT/SOUL.md

# Reactivează read-only
chmod 444 ~/.openclaw/workspaces/$AGENT/SOUL.md

# Actualizează soul-hashes.json cu noul hash
NEW_HASH=$(shasum -a 256 ~/.openclaw/workspaces/$AGENT/SOUL.md | awk '{print $1}')
# Scrie hash-ul nou în soul-hashes.json (necesită jq)
jq --arg agent "$AGENT" --arg hash "$NEW_HASH" \
  '.[$agent] = $hash' ~/.nexus/soul-hashes.json > /tmp/soul-hashes-tmp.json \
  && mv /tmp/soul-hashes-tmp.json ~/.nexus/soul-hashes.json
echo "Hash actualizat pentru $AGENT: $NEW_HASH"
```

### Pas 4: Loghează restaurarea
```bash
echo "[$(date)] RESTORE: $AGENT din backup $BACKUP" >> ~/.openclaw/logs/backup.log
```

Loghează în `~/.nexus/soul-change-log.md` dacă SOUL.md a fost restaurat (conform SOUL-MD-IMMUTABILITY Pas 4).

---

## 4. Verificare stare backup

```bash
# Câte backup-uri există
ls ~/.openclaw/backups/ | wc -l

# Cel mai recent backup
ls -1dt ~/.openclaw/backups/* | head -1

# Log recent
tail -10 ~/.openclaw/logs/backup.log

# LaunchAgent activ?
launchctl list com.genie.openclaw-backup
```

---

## 5. Cortex Logging

```json
{
  "text": "OpenClaw backup: [OK/FAILED]. Destination: ~/.openclaw/backups/YYYYMMDD-HHMM. Total backups retained: [N]/48.",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "backup_status",
    "procedure": "BACKUP-AUTOMATION-OPENCLAW",
    "rule_id": "OPS-H-001",
    "tags": ["backup", "automation", "openclaw", "workspaces", "recovery"]
  }
}
```

---

## 6. Enforcement Loop (META-H-002)

### WHERE
- `com.genie.openclaw-backup` LaunchAgent pe MacIntel — rulează la fiecare 15 min
- Manual: NIGHTLY-AUDIT-OPENCLAW Pas 2 verifică că backup-urile sunt recente

### WHEN
- Automat: la fiecare 15 minute când MacIntel e pornit
- Manual: înainte de orice modificare majoră la workspaces (preventiv)
- La incident: imediat după restaurare (verificare că backup-ul curent e consistent)

### HOW (violation detection)
- `launchctl list com.genie.openclaw-backup` → error → backup oprit → violation OPS-H-001
- Cel mai recent backup mai vechi de 30 min → LaunchAgent căzut → alertă
- `~/.openclaw/backups/` gol sau sub 5 intrări → backup nu a funcționat → violation
- Runner: NIGHTLY-AUDIT-OPENCLAW Pas 2 verifică backup-urile

### CONNECT
- `OPS-H-001` → autonomie tehnică: backup fără intervenție Pafi
- `SOUL-MD-IMMUTABILITY.md` → restaurare SOUL.md din backup la incident
- `AGENT-RESTART-STATE-PRESERVATION.md` → complementar (state preservation la restart)
- `NIGHTLY-AUDIT-OPENCLAW.md` → Pas 2 verifică freshness backup-uri
- `procedure-health.json` → entry `"BACKUP-AUTOMATION-OPENCLAW": "active"`

### VERIFY
La verificarea configurației:
- [ ] `launchctl list com.genie.openclaw-backup` returnează PID sau exit 0?
- [ ] `~/.openclaw/backups/` are >= 2 intrări cu timestamp recent (< 30 min)?
- [ ] `backup.log` are entries recente?
- [ ] Backup-urile conțin SOUL.md pentru toți 6 agenți?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → backup NU funcționează

**VK-uri obligatorii**:
1. `✅ [PROC] BACKUP-AUTOMATION-OPENCLAW | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Backup Automation OpenClaw" | FORGE ✓ | rule: OPS-H-001 | v1.1`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Backup script (bash) | Script bash (fără LLM) | `cp -R`, zero cost |
| Decizie restaurare | Genie (Sonnet) | Selectare backup + validare conținut |
| Recovery complex | Opus subagent | Dacă corupție multiplă sau conflict de versiuni |

---

## 7. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `backup-openclaw.sh` | Script backup | `~/.openclaw/scripts/backup-openclaw.sh` |
| LaunchAgent | Scheduling 15 min | `~/Library/LaunchAgents/com.genie.openclaw-backup.plist` |
| `~/.openclaw/workspaces/` | Sursă (6 agenți) | `rich-new, finance, marketing, legal, ops, insights` |
| `~/.openclaw/backups/` | Destinație backup-uri | Max 48 directoare (24h retenție) |
| `~/.openclaw/logs/backup.log` | Log operațional | Timestamp per backup |
| `soul-hashes.json` | Baseline integritate | `~/.nexus/soul-hashes.json` (actualizat post-restore) |

---

## 8. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Backup success rate | % rulări reușite | > 99% |
| Backup freshness | Vârsta celui mai recent backup | < 20 min |
| Recovery time | De la incident la workspace restaurat | < 5 minute |
| Retenție acoperire | Interval acoperit de backup-uri | > 12 ore |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — documentează infrastructura creată de Codex batch 040 (2026-02-26) |
| 1.1 | 2026-03-04 | FORGE-AUDIT: script complet, plist complet, setup instructions, comanda exacta soul-hashes.json update |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Infrastructura există și funcționează (Codex batch 040 + 17 rapoarte generate)
- [x] Salvat în Cortex cu metadata FORGE
