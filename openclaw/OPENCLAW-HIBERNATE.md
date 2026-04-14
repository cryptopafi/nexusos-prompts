---
type: procedure
created: 2026-03-17
status: active
slug: openclaw-hibernate
tags: [procedure, nexus]
---

# OpenClaw Hibernate — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.1
**Regulă asociată**: OPS-H-001 (Technical Autonomy)
**Scope**: Hibernarea controlată a sistemului OpenClaw (suspendare agenți, oprire LaunchAgent-uri, preservare stare) și trezirea (reactivare completă). Complementul OPENCLAW-SHUTDOWN (oprire totală) pentru pauze temporare.

---

## 1. Problema

OpenClaw are două stări diferite de "oprit":
- **SHUTDOWN** (OPENCLAW-SHUTDOWN) — oprire completă, inclusiv Gateway, toate serviciile
- **HIBERNATE** (această procedură) — suspendare agenților dar păstrarea infrastructurii gata de restart rapid

Fără un flux formal de hibernare:
- Agenții rămân activi consumând resurse fără a face nimic util
- LaunchAgent-urile de monitoring continuă să ruleze inutil (token cost, CPU)
- Starea sistemului nu e documentată — la revenire nu se știe ce era activ
- Sleep prevention (caffeinate) continuă inutil

**Situații acoperite**:
- Pauze planificate de 1-7 zile (vacanță, perioadă fără proiecte active)
- Perioadă de mentenanță MacIntel
- Economisire resurse când OpenClaw nu e necesar activ

**Diferență față de OPENCLAW-SHUTDOWN**:
- HIBERNATE: Gateway rămâne oprit dar configurația e intactă, LaunchAgents dezactivați temporar
- SHUTDOWN: oprire completă pentru motive tehnice sau de securitate

---

## 2. Procedura de Hibernare

### Pas 1: Notificare și stare curentă
```bash
echo "=== OpenClaw HIBERNATE START: $(date) ===" | tee ~/.openclaw/hibernate.log

# Verifică ce e activ
launchctl list | grep -E "openclaw|genie" | tee -a ~/.openclaw/hibernate.log
```

### Pas 2: Oprire Gateway (dacă activ)
```bash
# Verifică stare Gateway
launchctl list ai.openclaw.gateway 2>/dev/null && GATEWAY_WAS_ACTIVE=true || GATEWAY_WAS_ACTIVE=false

if [ "$GATEWAY_WAS_ACTIVE" = "true" ]; then
  openclaw stop 2>/dev/null || launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist 2>/dev/null || true
  echo "[$(date)] Gateway oprit" >> ~/.openclaw/hibernate.log
fi
```

### Pas 3: Dezactivare LaunchAgent-uri monitoring
```bash
AGENTS_TO_STOP=(
  "com.genie.openclaw-watchdog"
  "com.genie.openclaw-backup"
  "com.genie.openclaw-monitor"
  "com.genie.sleep-prevention"
  "com.genie.nightly-audit"
)

for agent in "${AGENTS_TO_STOP[@]}"; do
  if launchctl list "$agent" &>/dev/null; then
    launchctl bootout gui/$(id -u) ~/Library/LaunchAgents/${agent}.plist 2>/dev/null || \
    launchctl unload ~/Library/LaunchAgents/${agent}.plist 2>/dev/null || true
    echo "[$(date)] Dezactivat: $agent" >> ~/.openclaw/hibernate.log
  fi
done
```

### Pas 4: Backup final pre-hibernate
```bash
HIBERNATE_BACKUP="$HOME/.openclaw/backups/pre-hibernate-$(date +%Y%m%d-%H%M)"
cp -R ~/.openclaw/workspaces/ "$HIBERNATE_BACKUP/"
echo "[$(date)] Backup final: $HIBERNATE_BACKUP" >> ~/.openclaw/hibernate.log
```

### Pas 5: Salvează starea de hibernare
```bash
HIBERNATE_TS=$(date +%Y%m%d-%H%M)
python3 - "$GATEWAY_WAS_ACTIVE" "$HIBERNATE_TS" << 'PYEOF'
import json, sys, os
from datetime import datetime

gateway_was_active = sys.argv[1] == "true"
hibernate_ts = sys.argv[2]
home = os.path.expanduser("~")

state = {
    "status": "HIBERNATING",
    "hibernate_started": datetime.now().isoformat(),
    "gateway_was_active": gateway_was_active,
    "agents_suspended": [
        "com.genie.openclaw-watchdog",
        "com.genie.openclaw-backup",
        "com.genie.openclaw-monitor",
        "com.genie.sleep-prevention",
        "com.genie.nightly-audit"
    ],
    "pre_hibernate_backup": f"{home}/.openclaw/backups/pre-hibernate-{hibernate_ts}"
}
with open(f"{home}/.openclaw/hibernate-state.json", 'w') as f:
    json.dump(state, f, indent=2)
print("Stare hibernare salvată: ~/.openclaw/hibernate-state.json")
PYEOF
```

### Pas 6: Actualizare MEMORY.md
Actualizează `openclaw-architecture.md` sau MEMORY.md cu status HIBERNATING + data.

### Pas 7: Confirmare
```bash
echo "=== OpenClaw HIBERNATING din $(date) ===" >> ~/.openclaw/hibernate.log
echo "Sistem hibernat. Reactivare: OPENCLAW-HIBERNATE.md §3 (Wake)."
```

---

## 3. Procedura de Trezire (Wake)

### Pas 1: Citire stare pre-hibernate
```bash
cat ~/.openclaw/hibernate-state.json
echo "---"
cat ~/.openclaw/hibernate.log | tail -20
```

### Pas 2: Verificare integritate pre-wake
```bash
# Verifică că SOUL.md-urile sunt intacte
for agent in rich-new finance marketing legal ops insights; do
  ls -la ~/.openclaw/workspaces/$agent/SOUL.md && \
  echo "OK: $agent SOUL.md ($(stat -f %p ~/.openclaw/workspaces/$agent/SOUL.md))"
done
```

### Pas 3: Reactivare sleep prevention
```bash
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.genie.sleep-prevention.plist
sleep 2
pmset -g | grep sleep  # trebuie: "sleep prevented by caffeinate"
```

### Pas 4: Reactivare LaunchAgent-uri monitoring
```bash
AGENTS_TO_START=(
  "com.genie.openclaw-backup"
  "com.genie.openclaw-watchdog"
  "com.genie.openclaw-monitor"
  "com.genie.nightly-audit"
)

for agent in "${AGENTS_TO_START[@]}"; do
  launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/${agent}.plist 2>/dev/null || \
  launchctl load ~/Library/LaunchAgents/${agent}.plist 2>/dev/null || true
  echo "Reactivat: $agent"
done
```

### Pas 5: Pornire Gateway
```bash
openclaw start 2>/dev/null || \
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/ai.openclaw.gateway.plist
sleep 5

# Verifică că Gateway e activ
openclaw health 2>/dev/null || \
curl -s --max-time 5 http://localhost:18789/health && echo "Gateway OK"
```

### Pas 6: Execută GENIE-SUPERVISOR-SESSION-CHECKLIST
Verifică starea completă a sistemului conform checklist-ului de sesiune (toți 6 pași).

### Pas 7: Actualizare stare
```bash
python3 -c "
import json, os
from datetime import datetime
f = os.path.expanduser('~/.openclaw/hibernate-state.json')
with open(f, 'r') as file:
    data = json.load(file)
data['status'] = 'ACTIVE'
data['wake_at'] = datetime.now().isoformat()
with open(f, 'w') as file:
    json.dump(data, f, indent=2)
print('Status: ACTIVE')
"
```

---

## 4. Cortex Logging

### La Hibernate:
```json
{
  "text": "OpenClaw HIBERNATE inceput la [DATA]. LaunchAgents dezactivati: watchdog, backup, monitor, sleep-prevention, nightly-audit. Backup pre-hibernate creat. Motiv: [pauza planificata / mentenanta / etc].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "hibernate_start",
    "procedure": "OPENCLAW-HIBERNATE",
    "rule_id": "OPS-H-001",
    "tags": ["hibernate", "openclaw", "shutdown", "maintenance"]
  }
}
```

### La Wake:
```json
{
  "text": "OpenClaw WAKE din hibernare la [DATA]. Hibernat de la [DATA_HIBERNATE]. LaunchAgents reactivati. Gateway pornit. GENIE-SUPERVISOR-SESSION-CHECKLIST executat: [verdict].",
  "collection": "sessions",
  "metadata": {
    "source_agent": "genie",
    "type": "hibernate_wake",
    "procedure": "OPENCLAW-HIBERNATE",
    "rule_id": "OPS-H-001",
    "tags": ["wake", "openclaw", "startup", "recovery"]
  }
}
```

---

## 5. Enforcement Loop (META-H-002)

### WHERE
- Executat manual de Genie la cererea Pafi
- Nu e automat — hibernarea e o decizie deliberată

### WHEN
- **Hibernate**: când Pafi decide pauză > 1 zi sau MacIntel e pentru mentenanță
- **Wake**: la revenire, înainte de orice task OpenClaw
- Nu se face SHUTDOWN în loc de HIBERNATE pentru pauze scurte

### HOW (violation detection)
- OpenClaw în stare necunoscută (nici ACTIVE nici HIBERNATING) → violation
- LaunchAgent-uri de monitoring active fără Gateway activ → ineficiență → warning
- `hibernate-state.json` absent → stare nedocumentată → violation
- caffeinate activ fără Gateway → inutil, dezactivează

### CONNECT
- `OPS-H-001` → autonomie: starea sistemului e mereu documentată
- `OPENCLAW-STARTUP.md` → alternativă la Wake pentru restart complet
- `OPENCLAW-SHUTDOWN.md` → oprire totală vs hibernare temporară
- `MACINTEL-SLEEP-PREVENTION.md` → dezactivat la Hibernate, reactivat la Wake
- `GENIE-SUPERVISOR-SESSION-CHECKLIST.md` → executat după Wake (Pas 6)
- `procedure-health.json` → entry `"OPENCLAW-HIBERNATE": "active"`

### VERIFY
**Post-Hibernate**:
- [ ] `hibernate-state.json` există cu status HIBERNATING?
- [ ] LaunchAgent-uri monitoring oprite (`launchctl list | grep genie`)?
- [ ] Backup pre-hibernate creat?
- [ ] Cortex logging completat?

**Post-Wake**:
- [ ] Gateway activ (`openclaw health` / `curl localhost:18789/health`)?
- [ ] Sleep prevention activ (`pmset -g | grep sleep`)?
- [ ] GENIE-SUPERVISOR-SESSION-CHECKLIST executat?
- [ ] `hibernate-state.json` actualizat cu status ACTIVE?
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] OPENCLAW-HIBERNATE | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "OpenClaw Hibernate" | FORGE ✓ | rule: OPS-H-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Hibernate / Wake | Genie (Sonnet) | Orchestrare comenzi bash, verificări |
| Diagnosticare post-wake | Genie (Sonnet) | Session checklist + briefing |

---

## 6. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `~/.openclaw/hibernate-state.json` | Stare hibernare | Creat de această procedură |
| `~/.openclaw/hibernate.log` | Log operațional | Creat de această procedură |
| `MACINTEL-SLEEP-PREVENTION.md` | Dezactivat/reactivat | `~/.nexus/procedures/openclaw/` |
| `GENIE-SUPERVISOR-SESSION-CHECKLIST.md` | Post-wake verify | `~/.nexus/procedures/openclaw/` |
| `OPENCLAW-SHUTDOWN.md` | Alternativă oprire completă | `~/.nexus/procedures/openclaw/` |
| `OPENCLAW-STARTUP.md` | Alternativă restart complet | `~/.nexus/procedures/openclaw/` |
| `procedure-health.json` | Registry proceduri active | `~/.nexus/procedures/procedure-health.json` |
| LaunchAgents (5) | Dezactivate la hibernate | `~/Library/LaunchAgents/com.genie.*.plist` |

---

## 7. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Hibernate completeness | % pași completați | 100% (toți 7 pași) |
| Wake success rate | % wake-uri fără incident | > 99% |
| State documentation | `hibernate-state.json` prezent la fiecare hibernare | 100% |
| Wake recovery time | Timp de la wake la sistem complet activ | < 5 minute |

---

## Changelog

| Versiune | Data | Modificări |
|---------|------|-----------|
| 1.0 | 2026-03-04 | Creat — formalizează fluxul de hibernare (sistem e HIBERNATING din 2026-03-04) |
| 1.1 | 2026-03-04 | FORGE-AUDIT: Fix Pas 5 Python (variabile bash→argv), aliniat agenți Wake cu Hibernate (+openclaw-monitor), completat tabelul dependențe (+SHUTDOWN, +STARTUP, +procedure-health.json) |

## Checklist Pre-Publicare

- [x] Regulă asociată există (OPS-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent (separat pentru Hibernate și Wake)
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [x] Entry adăugat în `procedure-health.json`
- [x] Diferență față de OPENCLAW-SHUTDOWN documentată explicit
- [x] Salvat în Cortex cu metadata FORGE
