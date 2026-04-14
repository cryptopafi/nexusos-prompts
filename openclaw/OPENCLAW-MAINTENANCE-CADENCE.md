---
type: procedure
created: 2026-03-17
status: active
slug: openclaw-maintenance-cadence
tags: [procedure, nexus]
---

# OPENCLAW-MAINTENANCE-CADENCE — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-11
**Versiune**: 1.0
**Regulă asociată**: OPS-H-001
**Scope**: Cadenta operațională daily/weekly/monthly/quarterly pentru menținerea stabilității și securității OpenClaw pe MacIntel.

## 1. Problema

Fără un cadence clar, mentenanța OpenClaw devine reactivă: logurile se acumulează, tokenurile expiră, skill-urile rămân stale, iar drift-ul de configurare crește riscul de downtime.

Situații acoperite:
- verificări operaționale zilnice
- audit tehnic și securitate săptămânală
- actualizări și igienă de platformă lunară
- rotații credențiale trimestriale

## 2. Procedura

### Pas 1: Daily Checklist (la începutul zilei)
1. Verifică starea gateway: `openclaw gateway status`.
2. Verifică sănătatea generală: `openclaw doctor`.
3. Verifică ultimele loguri critice:
   - `tail -n 80 ~/.openclaw/logs/startup-gate.log`
   - `tail -n 80 ~/.openclaw/logs/watchdog.log`
   - `tail -n 80 ~/.openclaw/logs/errors.log`
4. Dacă apare un FAIL repetitiv, creează task de remediere în aceeași zi.

### Pas 2: Weekly Checklist (1x/săptămână)
1. Rulează audit de securitate OpenClaw:
   - `bash ~/.openclaw/nightly-audit.sh`
   - `bash ~/.openclaw/scripts/security-audit.sh` (dacă scriptul există)
2. Verifică rotația logurilor și spațiul ocupat:
   - `du -sh ~/.openclaw/logs`
   - `find ~/.openclaw/logs -type f -mtime +14 | wc -l`
3. Revizuiește sesiuni/erori recurente și extrage 1-3 acțiuni preventive.

### Pas 3: Monthly Checklist (1x/lună)
1. Actualizează OpenClaw stack controlat:
   - `openclaw update` (sau procedura internă de upgrade)
2. Rulează skills audit:
   - `openclaw skills list`
   - verifică skill-uri stale/deprecate și curăță ce nu mai e folosit.
3. Verifică prospețimea OAuth/API auth (keychain + sesiuni active).

### Pas 4: Quarterly Checklist (1x/trimestru)
1. Execută rotația de chei conform procedurii:
   - `~/.nexus/procedures/openclaw/API-KEYS-ROTATION-OPENCLAW.md`
2. Revocă și recreează sesiunile OAuth critice.
3. Verifică permisiuni + minim privileges pentru toate integrările active.
4. Rulează un smoke-test complet după rotații.

## 3. Cortex Logging

```json
{
  "text": "OPENCLAW-MAINTENANCE-CADENCE executed: daily/weekly/monthly/quarterly checks completed.",
  "collection": "sessions",
  "metadata": {
    "type": "maintenance",
    "procedure": "OPENCLAW-MAINTENANCE-CADENCE",
    "rule_id": "OPS-H-001",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["openclaw", "maintenance", "cadence", "operations"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Session Checklist (operational upkeep)
- Nightly/weekly OpenClaw audit reviews
- Procedure health reviews

### WHEN
- Daily: la primul check operațional
- Weekly: în ziua de audit stabilită
- Monthly: în prima săptămână din lună
- Quarterly: în prima săptămână din trimestru

### HOW (violation detection)
- Lipsă verificări din checklist pe perioada definită = violation.
- Erori critice repetate >7 zile fără task de remediere = violation.
- Runner: audit manual + `procedure-health.json` review.

### CONNECT
- `OPENCLAW-STARTUP` pentru starea gateway
- `NIGHTLY-AUDIT-OPENCLAW` pentru audit recurent
- `SECURITY-AUDIT-OPENCLAW` pentru verificări hardening
- `API-KEYS-ROTATION-OPENCLAW` pentru rotație trimestrială
- `procedure-health.json` entry activ pentru această procedură

### VERIFY
- [ ] Daily section executată
- [ ] Weekly section executată
- [ ] Monthly section executată
- [ ] Quarterly section executată
- [ ] Rezultate notate în jurnal de sesiune/Cortex

**VK-uri obligatorii**:
1. `✅ [PROC] OPENCLAW-MAINTENANCE-CADENCE | daily✓ weekly✓ monthly✓ quarterly✓ | complete`
2. `✅ [CORTEX] "OPENCLAW-MAINTENANCE-CADENCE" | FORGE ✓ | rule: OPS-H-001 | v1.0`
