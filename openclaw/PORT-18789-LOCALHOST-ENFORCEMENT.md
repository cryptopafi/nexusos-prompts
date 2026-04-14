---
type: procedure
created: 2026-03-17
status: active
slug: port-18789-localhost-enforcement
tags: [procedure, nexus]
---

# Port 18789 Localhost Enforcement — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regulă asociată**: SEC-H-002 (Never Expose Port 18789 to Public Internet)
**Scope**: Verificarea periodică și automată că portul 18789 (și 19001) rămân bound exclusiv pe loopback (127.0.0.1), niciodată pe 0.0.0.0 sau interfața publică.

---

## 1. Problema

Regula SEC-H-002 există ca HARD rule, dar fără audit periodic rămâne "security by promise". Riscuri concrete:
- Un update OpenClaw poate schimba binding-ul default la 0.0.0.0
- O modificare greșită a `openclaw.json` poate expune portul
- Un tool extern (Docker, VPN, port forward) poate rerouta 18789 spre exterior
- Pe macOS, `pf` (packet filter) sau Network Preferences pot schimba routing-ul

Portul 18789 expus public înseamnă acces neautorizat la toți agenții OpenClaw fără autentificare suplimentară.

Situații acoperite:
- Verificare la fiecare startup OpenClaw
- Audit periodic (zilnic sau săptămânal)
- Verificare după update OpenClaw sau macOS
- Verificare după modificare `openclaw.json`

---

## 2. Procedura

### Pas 1: Verificare Binding Port 18789
Confirmă că portul ascultă DOAR pe 127.0.0.1:
- Rulează `lsof -nP -iTCP:18789` sau `netstat -an | grep 18789`
- Output acceptabil: `127.0.0.1:18789` sau `localhost:18789`
- Output INACCEPTABIL: `0.0.0.0:18789`, `*:18789`, sau orice IP public

### Pas 2: Verificare Porturi Derivate (18791, 18792)
Aceleași verificări pentru porturile de control și CDP:
- 18791 (control port) → trebuie pe 127.0.0.1
- 18792 (CDP port) → trebuie pe 127.0.0.1

### Pas 3: Verificare Port Gateway 2 (19001, 19003, 19004)
Aceleași verificări pentru GW2:
- 19001, 19003, 19004 → toate pe 127.0.0.1

### Pas 4: Verificare Config openclaw.json
Verifică că binding-ul e explicit definit în config:
- Caută `host` sau `bind` în `~/.openclaw/openclaw.json`
- Valoare corectă: `"host": "127.0.0.1"` sau `"bind": "localhost"`
- Dacă lipsește → adaugă explicit (nu te baza pe default-ul OpenClaw)

### Pas 5: Verificare Firewall macOS
Confirmă că firewall-ul macOS nu permite inbound pe 18789 din afara localhost:
- System Preferences → Security → Firewall — verifică că nu există excepție pentru OpenClaw
- Dacă Tailscale e activ: confirmă că portul nu e expus pe interfața Tailscale (`100.x.x.x`)

### Pas 6: Acțiune Imediată dacă Port Expus
Dacă oricare port e expus public:
1. **STOP imediat OpenClaw** (execută OPENCLAW-SHUTDOWN.md)
2. **Fixează config**: adaugă `"host": "127.0.0.1"` explicit în openclaw.json
3. **Restart** cu OPENCLAW-STARTUP.md
4. **Re-verifică** că portul e acum pe localhost
5. **Log incident** în Cortex + Telegram alert imediat

---

## 3. Cortex Logging

**La audit clean**:
```json
{
  "text": "Port 18789 audit: PASS. Porturile 18789/18791/18792/19001 confirmate pe 127.0.0.1 only.",
  "collection": "procedures",
  "metadata": {
    "type": "security_audit",
    "procedure": "PORT-18789-LOCALHOST-ENFORCEMENT",
    "rule_id": "SEC-H-002",
    "result": "PASS",
    "tags": ["security", "port", "audit", "openclaw", "localhost"]
  }
}
```

**La incident (port expus)**:
```json
{
  "text": "SECURITY INCIDENT: Port 18789 expus pe [IP]. Acțiune: shutdown + reconfig + restart. Cauza probabilă: [X].",
  "collection": "procedures",
  "metadata": {
    "type": "security_incident",
    "procedure": "PORT-18789-LOCALHOST-ENFORCEMENT",
    "rule_id": "SEC-H-002",
    "result": "FAIL",
    "tags": ["security", "incident", "port-exposure", "openclaw"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- OPENCLAW-STARTUP.md Pas 1 (pre-flight check) — implicit obligatoriu
- Post-update check (după fiecare update OpenClaw sau macOS network stack)
- Nightly audit (01:00 EET) — adăugat în `nightly-audit.sh`

### WHEN
- La fiecare startup al OpenClaw
- Zilnic la audit nocturn
- Imediat după orice modificare a `openclaw.json`

### HOW (violation detection)
- OpenClaw pornit fără verificarea porturilor → violation SEC-H-002
- Port 18789 pe 0.0.0.0 detectat și sistemul nu a fost oprit imediat → violation CRITICĂ
- Audit nocturn nu include verificarea portului → violation META-H-001
- Runner: script bash în `nightly-audit.sh` + verificare manuală Genie la startup

### CONNECT
- `SEC-H-002` → regula sursă — HARD rule, zero excepții
- `OPENCLAW-STARTUP.md` → Pasul 1 (pre-flight) referențiază această procedură
- `nightly-audit.sh` → adaugă verificare port binding
- `procedure-health.json` → adaugă entry `"PORT-18789-LOCALHOST-ENFORCEMENT": "active"`

### VERIFY
La finalul execuției, verifică:
- [ ] Toate porturile (18789, 18791, 18792, 19001, 19003, 19004) confirmate pe 127.0.0.1?
- [ ] Config `openclaw.json` are `host` explicit definit?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă

**VK-uri obligatorii**:
1. `✅ [PROC] PORT-18789-LOCALHOST-ENFORCEMENT | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Port 18789 Localhost Enforcement" | FORGE ✓ | rule: SEC-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Audit port binding | Script bash (determinist) | Verificare mecanică, nu LLM |
| Diagnosticare cauza expunerii | Genie (Sonnet) | Analiză config + log |
| Incident response | Genie direct + Telegram | Viteză critică |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| `openclaw.json` | Config binding | `~/.openclaw/openclaw.json` |
| `lsof` / `netstat` | Verificare runtime binding | sistem macOS |
| `nightly-audit.sh` | Audit automat | `/Users/pafi/.openclaw/nightly-audit.sh` |
| macOS Firewall | Layer suplimentar | System Preferences |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (SEC-H-002)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Descrie CE nu CUM
- [x] VK format specificat
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Adăugat în `nightly-audit.sh`
- [ ] Salvat în Cortex cu metadata FORGE
