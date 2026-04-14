---
type: procedure
created: 2026-03-17
status: active
slug: business-intelligence
tags: [procedure, nexus]
---

# BUSINESS-INTELLIGENCE — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-02-28
**Versiune**: 1.0
**Regulă asociată**: OPS-S-003
**Scope**: Operare business intelligence pentru 4 business-uri prin colectare automată, alertare și raportare.

## 1. Problema

Trebuie monitorizate continuu semnale de piață, competitori și riscuri pentru ALBASTRU, CLICKWIN, SOLNEST și AI-B2B, cu livrare rapidă a alertelor critice și rezumate periodice în Telegram.

Fără această procedură, informația rămâne fragmentată, alertele pot fi ratate și execuția comercială devine reactivă.

## 2. Procedura

### Pas 1: Onboard business
Rulează `~/.openclaw/scripts/onboard-business.sh <business>` și generează/actualizează config-ul din `~/.openclaw/intelligence/`.

### Pas 2: Configurează reguli de alertare
Validează regulile active în `~/.nexus/business/alert-checker.py` (R01-R13) și severitatea lor (`CRITICAL`, `WARNING`, `INFO`).
**Skill**: `business-alerts` (`~/.claude/skills/business-alerts.md`) — invocabil direct pentru alert check manual pe orice business. Wrap pentru `alert-checker.py` cu output structurat.

### Pas 3: Activează radar pollers
Asigură rularea `radar_daemon.py` și a pollerelor (`Telegram`, `Gmail`, `Discord`, `WhatsApp`) din `~/.nexus/pafi/radar/`.

### Pas 4: Activează raportarea
Menține active scripturile de raport:
- `~/.nexus/business/weekly-intelligence-report.sh`
- `~/.nexus/business/monthly-intelligence-report.sh`
**Skill**: `weekly-report` (`~/.claude/skills/weekly-report.md`) — invocabil direct pentru generare raport ad-hoc sau verificare output. Wrap pentru ambele scripturi de raportare.

### Pas 5: Monitorizează livrarea în Telegram
Verifică alertele critice (imediate) și digest-urile WARNING/INFO, plus recepția rapoartelor săptămânale/lunare.

## 3. Cortex Logging

Stocare în `procedures` cu metadata de procedură business.

```json
{
  "text": "BUSINESS-INTELLIGENCE procedure applied: onboarding/alerts/radar/reports/telegram verified.",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "domain": "business",
    "procedure": "BUSINESS-INTELLIGENCE",
    "rule_id": "OPS-S-003",
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- Business intelligence operations (`~/.nexus/business/`)
- Daily/weekly health checks în monitoring stack

### WHEN
- La onboarding business nou
- La schimbări de reguli de alertare
- La audituri periodice

### HOW (violation detection)
- Lipsă config business în `~/.openclaw/intelligence/*-config.json`
- LaunchAgents radar/ingestion inactive
- Lipsă livrare raport săptămânal în Telegram
- Runner: `test-all-procedures.js` + verificare launchctl + health checks

### CONNECT
- `onboard-business.sh` (input/config flow)
- `alert-checker.py` (rules + routing)
- `weekly-intelligence-report.sh` / `monthly-intelligence-report.sh` (reporting)
- Cortex `procedures` (persist procedural state)

### VERIFY
- [ ] Business config există și este valid
- [ ] Alert checker rulează fără erori
- [ ] Radar pollers și report jobs sunt active
- [ ] Raport săptămânal a fost primit în Telegram
- [ ] VK emis în sesiune

**VK-uri obligatorii**:
1. `✅ [PROC] BUSINESS-INTELLIGENCE | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Business Intelligence" | FORGE ✓ | rule: OPS-S-003 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Operare business intelligence (monitorizare, alertare) | Genie direct (Sonnet) | Orchestrare simplă |
| Audit calitate rapoarte | Opus subagent | Evaluare complexă necesită reasoning |

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Cortex KB | Search/store intelligence | `http://localhost:6400` |
| Telegram bot | Alerting/report delivery | `https://api.telegram.org` |
| Perplexity API | Source discovery în onboarding | via `discover-sources.sh` |
| Ollama | Clasificare locală | `http://localhost:11434` |

