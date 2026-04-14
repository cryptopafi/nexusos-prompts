---
type: procedure
created: 2026-03-17
status: active
slug: soul-md-update-authorized
tags: [procedure, nexus]
---

# SOUL.md Update Authorized — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.1
**Regulă asociată**: SEC-H-006 (Richard Autonomy — Genie as Guardian), META-H-002 (FORGE)
**Scope**: Fluxul autorizat pentru modificarea SOUL.md a unui agent OpenClaw — complement la SOUL-MD-IMMUTABILITY (care acoperă incidentele). Orice modificare SOUL.md necesită autorizare Pafi + logging.

---

## 1. Problema

SOUL-MD-IMMUTABILITY acoperă răspunsul la modificări **neautorizate**. Dar există situații legitime când SOUL.md trebuie actualizat:
- Rolul agentului evoluează (ex: Finance primește acces la un tool nou)
- Bug în instrucțiunile actuale (agent produce output incorect)
- Rebalansare echipă (redistribuire responsabilități între agenți)
- Update la format/structură (toți agenții primesc secțiune nouă)

Fără o procedură autorizată:
- Modificarea directă activează SOUL-MD-IMMUTABILITY ca INCIDENT fals
- Nu există audit trail al modificărilor legitime
- `soul-hashes.json` rămâne cu hash vechi → toate verificările ulterioare eșuează

**Situații acoperite**:
- Modificare conținut SOUL.md (instrucțiuni, rol, tools, restricții)
- Update structural (adăugare secțiune nouă la toți agenții)
- Rollback autorizat (revert la versiune anterioară după incident)

---

## 2. Procedura

### Pas 1: Autorizare Pafi (OBLIGATORIU)
Orice modificare SOUL.md necesită aprobare explicită Pafi:
- Describe modificarea propusă: **ce**, **de ce**, **care agent**
- Pafi confirmă verbal sau prin Telegram
- Loghează autorizarea în `~/.nexus/soul-change-log.md`:

```bash
cat >> ~/.nexus/soul-change-log.md << EOF

## $(date +%Y-%m-%d) — [AGENT] — [DESCRIERE SCURTĂ]
- **Autorizat de**: Pafi
- **Motiv**: [de ce e necesară modificarea]
- **Agent**: [rich-new / finance / marketing / legal / ops / insights]
- **Modificare**: [ce se schimbă]
- **Aprobat la**: $(date '+%Y-%m-%d %H:%M')
EOF
```

### Pas 2: Backup pre-modificare
```bash
AGENT="rich-new"  # sau alt agent
TIMESTAMP=$(date +%Y%m%d-%H%M%S)
BACKUP_DIR="$HOME/.nexus/soul-backups"
mkdir -p "$BACKUP_DIR"

# Backup SOUL.md înainte de modificare
cp ~/.openclaw/workspaces/$AGENT/SOUL.md \
   $BACKUP_DIR/${AGENT}-SOUL-${TIMESTAMP}.md

echo "Backup creat: $BACKUP_DIR/${AGENT}-SOUL-${TIMESTAMP}.md"
```

### Pas 3: Dezactivează read-only temporar (window 10 min)
```bash
AGENT="rich-new"
chmod 644 ~/.openclaw/workspaces/$AGENT/SOUL.md
echo "SOUL.md $AGENT: read-only dezactivat. Ai 10 minute să faci modificarea."

# Safety net: re-lock automat după 10 minute (dacă uiți Pas 5)
(sleep 600 && chmod 444 ~/.openclaw/workspaces/$AGENT/SOUL.md && echo "[AUTO-LOCK] SOUL.md $AGENT revenit la 444 după 10 min timeout") &
LOCK_PID=$!
echo "Auto-lock PID: $LOCK_PID (kill $LOCK_PID dacă ai nevoie de mai mult timp)"
```

### Pas 4: Aplică modificarea
Editează SOUL.md direct sau via Genie (cu Edit tool). Modifică NUMAI ce a fost autorizat la Pas 1.

```bash
# Verifică că modificarea e corectă înainte de reactivare read-only
diff $BACKUP_DIR/${AGENT}-SOUL-${TIMESTAMP}.md \
     ~/.openclaw/workspaces/$AGENT/SOUL.md
```

### Pas 5: Reactivează read-only și actualizează baseline
```bash
AGENT="rich-new"

# Reactivează protecția
chmod 444 ~/.openclaw/workspaces/$AGENT/SOUL.md
echo "SOUL.md $AGENT: read-only reactivat"

# Calculează noul hash
NEW_HASH=$(shasum -a 256 ~/.openclaw/workspaces/$AGENT/SOUL.md | awk '{print $1}')
echo "Nou hash: $NEW_HASH"

# Actualizează soul-hashes.json
# Nota: richard = rich-new in path dar "richard" in soul-hashes.json
AGENT_KEY="${AGENT/rich-new/richard}"  # mapare path → key
python3 - << PYEOF
import json
hashes_file = "$HOME/.nexus/soul-hashes.json"
with open(hashes_file, 'r') as f:
    data = json.load(f)
data['agents']['$AGENT_KEY']['sha256'] = '$NEW_HASH'
data['agents']['$AGENT_KEY']['last_verified'] = '$(date +%Y-%m-%d)'
with open(hashes_file, 'w') as f:
    json.dump(data, f, indent=2)
print(f"soul-hashes.json actualizat pentru $AGENT_KEY: ${NEW_HASH[:16]}...")
PYEOF
```

### Pas 6: Verifică și loghează
```bash
# Verifică că monitoring nu detectează false INCIDENT
bash ~/.openclaw/scripts/genie-security-prescan.sh 2>/dev/null | grep -i soul || echo "Hash check: OK"

# Completează log-ul soul-change-log.md
cat >> ~/.nexus/soul-change-log.md << EOF
- **Executat la**: $(date '+%Y-%m-%d %H:%M')
- **Hash nou**: $(shasum -a 256 ~/.openclaw/workspaces/$AGENT/SOUL.md | awk '{print $1}')
- **Backup**: $BACKUP_DIR/${AGENT}-SOUL-${TIMESTAMP}.md
- **Status**: COMPLETAT
EOF

echo "Update SOUL.md finalizat. Loghează în Cortex (Pas 7)."
```

### Pas 7: Cortex logging
Salvează în Cortex — comanda completă:
```bash
AGENT="rich-new"  # agentul modificat
DESCRIERE="[descriere scurtă a modificării]"
HASH_PREFIX=$(shasum -a 256 ~/.openclaw/workspaces/$AGENT/SOUL.md | awk '{print substr($1,1,16)}')

ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{
    \"text\": \"SOUL.md update autorizat: agent=$AGENT, modificare=$DESCRIERE, autorizat de Pafi la $(date +%Y-%m-%d). Hash nou: ${HASH_PREFIX}.... Backup: soul-backups/${AGENT}-SOUL-${TIMESTAMP}.md.\",
    \"collection\": \"sessions\",
    \"metadata\": {
      \"source_agent\": \"genie\",
      \"type\": \"soul_update_authorized\",
      \"procedure\": \"SOUL-MD-UPDATE-AUTHORIZED\",
      \"rule_id\": \"SEC-H-006\",
      \"tags\": [\"soul-md\", \"update\", \"authorized\", \"openclaw\", \"governance\"]
    }
  }'"
```

---

## 3. Cortex Logging

```json
{
  "text": "SOUL.md update autorizat: agent=[AGENT], modificare=[DESCRIERE], autorizat de Pafi la [DATA]. Hash nou: [HASH_PREFIX].... Backup: soul-backups/[AGENT]-SOUL-[TIMESTAMP].md.",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "soul_update_authorized",
    "procedure": "SOUL-MD-UPDATE-AUTHORIZED",
    "rule_id": "SEC-H-006",
    "tags": ["soul-md", "update", "authorized", "openclaw", "governance"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Executat de Genie la cererea Pafi, oricând e necesară o modificare legitimă SOUL.md
- Nu e automat — necesită inițiativă explicită (Pafi sau Genie cu propunere)

### WHEN
- La orice modificare planificată SOUL.md, indiferent de urgență
- Rollback post-incident: dacă SOUL-MD-IMMUTABILITY detectează modificare, iar Pafi decide că e legitimă → această procedură formalizează noul baseline
- Nu se sare NICIODATĂ Pas 1 (autorizare Pafi) — nici în urgențe

### HOW (violation detection)
- Modificare SOUL.md detectată de monitoring fără entry în `soul-change-log.md` → INCIDENT (SOUL-MD-IMMUTABILITY)
- `soul-hashes.json` neactualizat după modificare → false alerte continue → violation
- Pas 1 sărit (modificare fără aprobare Pafi) → violation SEC-H-006
- Runner: audit manual Genie sau NIGHTLY-AUDIT Pas 1

### CONNECT
- `SEC-H-006` → Genie guardian: modificările SOUL.md sunt controlate
- `SOUL-MD-IMMUTABILITY.md` → incident response dacă modificare detectată fără autorizare
- `AGENT-HEALTH-MONITORING.md` → Pas 3 detectează hash diferit → trebuie să vadă entry în soul-change-log.md
- `NIGHTLY-AUDIT-OPENCLAW.md` → Pas 1 verifică SOUL.md hashes — baseline actualizat de această procedură
- `~/.nexus/soul-change-log.md` → audit trail obligatoriu
- `procedure-health.json` → entry `"SOUL-MD-UPDATE-AUTHORIZED": "active"`

### VERIFY
- [ ] Entry în `soul-change-log.md` cu autorizare Pafi?
- [ ] Backup creat în `~/.nexus/soul-backups/` înainte de modificare?
- [ ] `soul-hashes.json` actualizat cu noul hash?
- [ ] SOUL.md revenit la `chmod 444`?
- [ ] Logging în Cortex făcut?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → modificarea nu e complet auditată

**VK-uri obligatorii**:
1. `✅ [PROC] SOUL-MD-UPDATE-AUTHORIZED | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "SOUL.md Update Authorized" | FORGE ✓ | rule: SEC-H-006 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Coordonare update | Genie (Sonnet) | Orchestrare pași, verificare |
| Redactare modificare SOUL.md | Opus subagent | Calitate instrucțiuni agent — necesită judgment |
| Validare diff | Genie (Sonnet) | Verificare că modificarea e în scope autorizat |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `~/.nexus/soul-change-log.md` | Audit trail autorizări | Creat dacă nu există |
| `~/.nexus/soul-backups/` | Backup pre-modificare | Creat la primul update |
| `~/.nexus/soul-hashes.json` | Baseline hash actualizat | `~/.nexus/soul-hashes.json` |
| `genie-security-prescan.sh` | Validare post-update | `~/.openclaw/scripts/genie-security-prescan.sh` |
| `SOUL-MD-IMMUTABILITY.md` | Incident response complementar | `~/.nexus/procedures/openclaw/` |
| `~/.openclaw/workspaces/$AGENT/SOUL.md` | Fișierul țintă al modificării | Per agent: `rich-new`, `finance`, `marketing`, `legal`, `ops`, `insights` |
| `procedure-health.json` | Registry proceduri active | `~/.openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Authorization rate | % modificări SOUL.md cu entry în soul-change-log.md | 100% |
| Backup coverage | % modificări cu backup pre-update | 100% |
| Hash sync latency | Timp de la modificare la soul-hashes.json actualizat | < 5 minute |
| False INCIDENT rate | Alerte SOUL-MD-IMMUTABILITY după update autorizat | 0% |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — complement la SOUL-MD-IMMUTABILITY pentru fluxul autorizat |
| 1.1 | 2026-03-04 | FORGE-AUDIT: +safety net auto-lock 10min (F1), +Cortex curl complet Pas 7 (F3), +SOUL.md si procedure-health.json in Dependencies (F4/F5) |

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-006)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] `soul-change-log.md` și `soul-backups/` create ca dependențe
- [x] Salvat în Cortex cu metadata FORGE
