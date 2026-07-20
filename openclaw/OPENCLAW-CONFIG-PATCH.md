---
type: procedure
created: 2026-03-17
status: active
slug: openclaw-config-patch
tags: [procedure, nexus]
---

# OpenClaw Config Patch — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: OPS-H-001 (Technical Autonomy), SEC-H-006 (Richard Autonomy — Genie as Guardian)
**Scope**: Modificarea controlată a `~/.openclaw/openclaw.json` — schimbare model, ajustare parametri agent, configurare gateway, actualizare providers. Orice patch la config necesită backup + validare JSON + restart Gateway dacă e necesar.

---

## 1. Problema

`~/.openclaw/openclaw.json` e fișierul central de configurare OpenClaw. Conține: `models`, `agents`, `gateway`, `auth`, `channels`, `skills`, `plugins`. Modificările directe fără procedură:
- **Risc JSON invalid** — sintaxă greșită oprește Gateway la restart
- **Fără audit trail** — nu se știe ce s-a schimbat, când și de ce
- **Gateway restart omis** — unele schimbări (ex: model swap) necesită restart; dacă se uită, agentul rulează cu config veche
- **Backup absent** — revertul e imposibil dacă nu există un snapshot pre-patch

**Incidente documentate**: Multiple backup-uri manuale existente (`.bak`, `.bak.1`, `.bak.2`, `bak.20260220-...`) — semn că patch-urile ad-hoc au cauzat probleme și au necesitat revert.

**Situații acoperite**:
- Schimbare model agent (ex: `rich-new` → Opus 4.6 în loc de Sonnet)
- Ajustare parametri (`bootstrapMaxChars`, `compaction`, `maxConcurrent`)
- Actualizare provider (nou API key, nou endpoint)
- Configurare gateway (port, bind, auth)
- Rollback la config anterioară

---

## 2. Procedura

### Pas 1: Verificare stare curentă și backup
```bash
# Verifică config curentă
python3 -c "import json; d=json.load(open('$HOME/.openclaw/openclaw.json')); print('JSON valid. Keys:', list(d.keys()))" 2>/dev/null || echo "EROARE: JSON invalid!"

# Backup pre-patch (OBLIGATORIU)
PATCH_TS=$(date +%Y%m%d-%H%M%S)
cp ~/.openclaw/openclaw.json ~/.openclaw/openclaw.json.bak.${PATCH_TS}
echo "Backup creat: ~/.openclaw/openclaw.json.bak.${PATCH_TS}"

# Verifică backup-urile existente (curăță manual dacă > 10)
ls -lt ~/.openclaw/openclaw.json.bak.* 2>/dev/null | head -5
```

### Pas 2: Aplică patch-ul

**Opțiunea A — Edit direct cu python3** (pentru schimbări structurate):
```bash
# Exemplu: schimbare model pentru agentul defaults
python3 << 'PYEOF'
import json
config_file = open(f"{__import__('os').path.expanduser('~')}/.openclaw/openclaw.json")
d = json.load(config_file)
config_file.close()

# Modifică DOAR ce e autorizat — ex: model agent
# d['agents']['defaults']['model'] = 'claude-opus-4-8'

# Salvează
with open(f"{__import__('os').path.expanduser('~')}/.openclaw/openclaw.json", 'w') as f:
    json.dump(d, f, indent=2)
print("Config salvat.")
PYEOF
```

**Opțiunea B — Edit manual** cu Genie via Edit tool, urmat de validare la Pas 3.

### Pas 3: Validare JSON post-patch (OBLIGATORIU)
```bash
python3 -c "
import json
with open('$HOME/.openclaw/openclaw.json') as f:
    d = json.load(f)
print('JSON valid.')
print('Keys:', list(d.keys()))
# Verifică că secțiunile critice sunt intacte
assert 'agents' in d, 'EROARE: secțiunea agents lipsă!'
assert 'gateway' in d, 'EROARE: secțiunea gateway lipsă!'
assert 'models' in d, 'EROARE: secțiunea models lipsă!'
print('Toate secțiunile critice: OK')
"
# Așteptat: "JSON valid." + "Toate secțiunile critice: OK"
```

### Pas 4: Diff față de backup
```bash
# Folosește $PATCH_TS capturat la Pas 1 (rulează în același shell)
diff ~/.openclaw/openclaw.json.bak.${PATCH_TS} ~/.openclaw/openclaw.json
# Verifică că modificările sunt DOAR cele intenționate
```

### Pas 5: Restart Gateway dacă e necesar
```bash
# Necesar pentru: schimbare model, schimbare auth, schimbare port
# Nu e necesar pentru: schimbare parametri workspace-only

# Verifică stare curentă
openclaw health 2>/dev/null || curl -sf http://127.0.0.1:18789/health && echo "Gateway UP" || echo "Gateway DOWN"

# Restart (dacă e necesar)
openclaw stop 2>/dev/null || launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist 2>/dev/null || true
sleep 3
openclaw start 2>/dev/null || launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist
sleep 5

# Verifică după restart
openclaw health 2>/dev/null || curl -sf http://127.0.0.1:18789/health && echo "Gateway OK post-restart"
```

### Pas 6: Rollback (dacă ceva merge rău)
```bash
# Identifică backup-ul corect (ultimul creat la Pas 1)
ls -lt ~/.openclaw/openclaw.json.bak.* | head -5

# Revert — folosește $PATCH_TS din Pas 1 sau alege manual din lista de mai sus
BACKUP="$HOME/.openclaw/openclaw.json.bak.${PATCH_TS}"
cp "$BACKUP" ~/.openclaw/openclaw.json
echo "Revert la $BACKUP"

# Validează și restartează
python3 -c "import json; json.load(open('$HOME/.openclaw/openclaw.json')); print('JSON valid post-revert')"

# Restart Gateway după revert
openclaw stop 2>/dev/null || true
sleep 2
openclaw start 2>/dev/null || launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist
sleep 5
openclaw health 2>/dev/null || curl -sf http://127.0.0.1:18789/health && echo "Gateway OK"
```

### Pas 7: Logging în soul-change-log.md (pentru patch-uri cu impact major)
```bash
# Opțional — recomandat pentru schimbări de model sau auth
# Substituie [DESCRIERE], [MOTIV], [CE S-A SCHIMBAT] cu valorile reale înainte de rulare
cat >> ~/.nexus/soul-change-log.md << EOF

## $(date +%Y-%m-%d) — openclaw.json — [DESCRIERE SCURTĂ]
- **Autorizat de**: Pafi
- **Motiv**: [de ce e necesară modificarea]
- **Modificare**: [ce s-a schimbat exact]
- **Backup**: $HOME/.openclaw/openclaw.json.bak.${PATCH_TS}
- **Aprobat la**: $(date '+%Y-%m-%d %H:%M')
EOF
```

---

## 3. Cortex Logging

```json
{
  "text": "OpenClaw Config Patch: openclaw.json modificat. Modificare: [descriere]. Backup pre-patch: openclaw.json.bak.[TIMESTAMP]. JSON valid post-patch. Gateway restart: [DA/NU]. Procedura OPENCLAW-CONFIG-PATCH aplicata.",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "config_change",
    "procedure": "OPENCLAW-CONFIG-PATCH",
    "rule_id": "OPS-H-001",
    "tags": ["config", "openclaw", "patch", "openclaw-json", "gateway", "governance"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Executat de Genie la orice modificare `openclaw.json`, indiferent de context
- Nu e automat — necesită inițiativă Genie sau cerere Pafi

### WHEN
- La orice modificare planificată `openclaw.json`: model swap, parametri, auth, gateway
- Pre-test nou model sau provider — backup înainte de experimentare
- Post-incident — rollback controlat dacă configurația cauzează probleme
- Nu se sare NICIODATĂ Pas 1 (backup) și Pas 3 (validare JSON)

### HOW (violation detection + acțiune post-violation)
- `~/.openclaw/openclaw.json` invalid JSON (Gateway nu pornește) → violation OPS-H-001
  - **Acțiune**: Genie rulează Pas 6 (rollback la ultimul backup valid). Validare Pas 3. Restart Gateway Pas 5.
- Modificare `openclaw.json` fără backup (nu există `.bak.YYYYMMDD` cu timestamp recent) → violation
  - **Acțiune**: Genie face backup retroactiv și documentează în soul-change-log.md
- Gateway DOWN după patch → config incompatibilă → violation
  - **Acțiune**: Genie rulează Pas 6 (rollback) + Pas 5 (restart) + notificare Pafi dacă rollback eșuează

### CONNECT
- `OPS-H-001` → autonomie: Genie poate patch config fără intervenție Pafi pentru modificări minore
- `SEC-H-006` → Richard Autonomy: schimbările de model sau auth necesită notificare Pafi
- `SOUL-MD-UPDATE-AUTHORIZED.md` → procedură analogă pentru SOUL.md (backup + autorizare + hash update)
- `OPENCLAW-STARTUP.md` → la startup verifică config validă înainte de pornire Gateway
- `GATEWAY2-WATCHDOG-SETUP.md` → watchdog detectează Gateway DOWN post-patch
- `procedure-health.json` → entry `"OPENCLAW-CONFIG-PATCH": "active"`

### VERIFY
- [ ] Backup `openclaw.json.bak.YYYYMMDD-HHMMSS` creat înainte de patch?
- [ ] `python3 -c "import json; json.load(open('$HOME/.openclaw/openclaw.json'))"` → JSON valid?
- [ ] `diff backup_file openclaw.json` → arată DOAR modificările intenționate?
- [ ] Gateway UP după patch (`openclaw health` sau `curl localhost:18789/health`)? (dacă restart a fost necesar)
- [ ] Cortex logging completat?
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] OPENCLAW-CONFIG-PATCH | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "OpenClaw Config Patch" | FORGE ✓ | rule: OPS-H-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Patch minor (parametri, compaction) | Genie (Sonnet) | Modificări simple, verificare JSON |
| Patch complex (model swap, auth, gateway) | Genie (Sonnet) + notificare Pafi | Impact major, necesită confirmare |
| Diagnosticare Gateway DOWN post-patch | Genie (Sonnet) | Rollback + restart rapid |

---

## 5. Dependențe

| Componentă | Rol | Path absolut |
|-----------|-----|--------------|
| `~/.openclaw/openclaw.json` | Config principală OpenClaw | `/Users/pafi/.openclaw/openclaw.json` |
| `~/.openclaw/openclaw.json.bak.*` | Backup-uri pre-patch | `/Users/pafi/.openclaw/openclaw.json.bak.YYYYMMDD-HHMMSS` |
| `~/Library/LaunchAgents/ai.openclaw.gateway.plist` | Gateway LaunchAgent (restart) | `/Users/pafi/Library/LaunchAgents/ai.openclaw.gateway.plist` |
| `~/.nexus/soul-change-log.md` | Audit trail modificări majore | `/Users/pafi/.nexus/soul-change-log.md` |
| `OPENCLAW-STARTUP.md` | Verificare config la startup | `/Users/pafi/.nexus/procedures/openclaw/OPENCLAW-STARTUP.md` |
| `GATEWAY2-WATCHDOG-SETUP.md` | Detecție Gateway DOWN post-patch | `/Users/pafi/.nexus/procedures/openclaw/GATEWAY2-WATCHDOG-SETUP.md` |
| `SOUL-MD-UPDATE-AUTHORIZED.md` | Procedură analogă (backup + audit) | `/Users/pafi/.nexus/procedures/openclaw/SOUL-MD-UPDATE-AUTHORIZED.md` |
| `procedure-health.json` | Registry proceduri active | `/Users/pafi/.nexus/procedures/openclaw/procedure-health.json` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target | Metoda de măsurare | Frecvență |
|---------|-----------|--------|--------------------|-----------|
| Backup compliance | % patch-uri cu backup pre-modificare | 100% | `ls ~/.openclaw/openclaw.json.bak.*` → timestamp recents vs modificare config | La fiecare patch |
| JSON validity post-patch | % patch-uri fără erori JSON | 100% | `python3 -c "import json; json.load(...)"` → exit 0 | La fiecare patch |
| Gateway uptime post-patch | Gateway UP în 30s după patch | 100% (când restart necesar) | `curl -sf http://127.0.0.1:18789/health` → 200 OK | La fiecare patch cu restart |
| Rollback frequency | Număr rollback-uri necesare | 0/lună | `ls ~/.openclaw/openclaw.json.bak.*` + correlation cu Cortex `type: config_change` | Lunar |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|------------|
| 1.0 | 2026-03-05 | Creat — formalizează patch-urile ad-hoc la openclaw.json (multiple backup-uri manuale pre-existente ca semn al problemei) |
| 1.1 | 2026-03-05 | FORGE-AUDIT fixes: bug python double-open Pas 1, variabilă $PATCH_TS consistentă Pas 1/4/6/7, HOW rollback eșuat → notificare Pafi, Cortex JSON nota substituție placeholders |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001, SEC-H-006)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] Acțiune post-violation definită în HOW (3 scenarii: JSON invalid, fără backup, Gateway DOWN)
- [x] VERIFY checkpoint prezent cu comenzi concrete
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Rollback complet documentat (Pas 6)
- [x] Ambele opțiuni de edit documentate (python3 + Edit tool)
- [x] Path-uri absolute în tabelul Dependențe
- [x] Metrics cu metodă de măsurare și frecvență
- [x] Incident documentat în §1 (backup-uri multiple = semn al patch-urilor ad-hoc)
