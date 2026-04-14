---
type: procedure
created: 2026-03-17
status: active
slug: codex-model-routing-procedure
tags: [procedure, nexus]
---

# CODEX MODEL ROUTING — Procedură Standard de Operare

**Status**: UPDATED — audit-only mode (2026-03-17)
**Creat**: 2026-02-27
**Versiune**: 1.2 (audit-only routing update 2026-03-17)
**Regulă asociată**: COST-H-001 + DEV-H-004
**Tabel routing**: `memory/rules/model-routing-table.md`
**Enforcement**: Pre-queue gate (la fiecare brief) + Nightly audit + Monthly benchmark

---

## 1. Problema

Asigură că fiecare task trimis la Codex folosește modelul corect conform tabelului de routing. Previne:
- Risipă tokens (model prea puternic pentru task simplu)
- Calitate slabă (model prea slab pentru task complex)
- Drift în timp (fără audit, se folosește același model pentru tot)

## 2. Modele Disponibile (actualizat 2026-03-17, audit-only routing)

| Model | Speed | Reasoning | Context | Când |
|-------|-------|-----------|---------|------|
| **gpt-5.4** | faster than 5.3 | low→xhigh | 1M (exp.) | Default pentru audit tehnic extern: NPLF code review, security, bash compat, cross-file audit |
| **gpt-5.3-codex-spark** | 1000+ tok/s | fixed (no levels) | 128K | Lint/syntax/smoke-review rapid, single-file, low-risk audit helper |
| **gpt-5.3-codex** | 65-70 tok/s | low→xhigh | 400K+ | Coding-only legacy lane — DEPRECATED pentru audit mode |
| **gpt-5.2-codex** | ~80 tok/s | low→xhigh | 256K | Coding-only legacy lane — DEPRECATED pentru audit mode |
| **gpt-5.1-codex-max** | ~60 tok/s | low→xhigh | 400K+ | Coding-only legacy lane — DEPRECATED pentru audit mode |
| **gpt-5.2** | ~80 tok/s | low→xhigh | 256K | Non-coding legacy lane — DEPRECATED pentru audit mode |
| **gpt-5.1-codex-mini** | ~100 tok/s | medium/high | 64K | Legacy economic lane — DEPRECATED pentru audit mode |

**Nu mai sunt recomandate** (hide în API): gpt-5.1-codex, gpt-5-codex, gpt-5, gpt-5.1, gpt-5-codex-mini (nume greșit — e gpt-5.1-codex-mini)

## 3. Routing Decision Tree

> **NOTĂ (2026-03-17)**: Codex funcționează **exclusiv** în modul `audit-only`.
> ❌ NU mai primește coding tasks — toate merg la TECH (Claude Sonnet 4.6) direct.
> ✅ Verifică `~/.nexus/state/codex-status.json` → `mode: "audit-only"` înainte de orice dispatch.
> 
> **Q0 (nou)**: Înainte de orice routing — "Este task de tip AUDIT (read-only)?"
> - NU (coding/build/fix/deploy) → STOP. Trimite la TECH, nu Codex.
> - DA → continuă cu Q1 de mai jos.

### Decision Tree (audit tasks only)


```
Task primit de Genie → clasificare:

Q0: Este task de tip audit?
  └─ DA → Q1
  └─ NU → REJECT pentru Codex audit lane
       [DEPRECATED - coding tasks merg la TECH]

Q1: Auditul este low-risk lint/syntax only?
  └─ DA (single-file, syntax, lint, smoke review) → gpt-5.3-codex-spark
  └─ NU → Q2

Q2: Auditul implică oricare dintre următoarele?
  └─ >200 linii total
  └─ >5 fișiere
  └─ bash script
  └─ security review
  └─ daemon/pipeline/config cross-file
  └─ DA la oricare → gpt-5.4
  └─ NU → gpt-5.4 (default audit technical lane)

Legacy branches:
  └─ Q-code / Q-non-code / Q-security-code
       [DEPRECATED - coding tasks merg la TECH]
```

**Ramuri deprecated pentru referință istorică**:
- `Q1: E cod sau non-cod?` → `[DEPRECATED - coding tasks merg la TECH]`
- `Q2: Câte fișiere afectează?` → `[DEPRECATED - coding tasks merg la TECH]`
- `Q3: E verificabil în <30 secunde?` → înlocuit de lint/syntax gate pentru Spark
- `Q4: Combină cod cu decizii non-cod?` → `[DEPRECATED - coding tasks merg la TECH]`
- `Override Q: E security-critical?` → audit security merge direct la `gpt-5.4`

## 4. Pre-Queue Validation Gate (la FIECARE brief)

**Când**: Înainte de a scrie brief-ul în `~/.codex/genie-to-codex.md`
**Cine**: Genie (automat, parte din workflow H → Codex)

### Checklist (5 puncte, <10 secunde):

```
□ 1. MODE CHECK: `~/.nexus/state/codex-status.json` are `mode: "audit-only"`?
     → Dacă nu: NU scrie brief la Codex. Redirect la TECH sau Opus, după caz.
□ 2. TYPE CHECK: Brief-ul este `Type: audit-tehnic (read-only)`?
     → Dacă nu: REJECT. Coding tasks nu mai merg la Codex.
□ 3. MODEL MATCH: Task-ul este lint/syntax low-risk?
     → DA: `gpt-5.3-codex-spark`
     → NU: `gpt-5.4`
□ 4. SECURITY/BASH CHECK: Dacă task-ul atinge bash/security/daemon/pipeline
     → Folosește `gpt-5.4` obligatoriu. Spark INTERZIS.
□ 5. OUTPUT CHECK: Brief-ul cere JSON audit output + NPLF scoring?
     → Dacă nu: corectează înainte de queue.
```

### Format brief header (obligatoriu):

```markdown
## [DATE] [TASK ID] [PRIORITY] TASK: [Titlu]

### Model Setting
Model: [model] | Reasoning: [level]
Routing: [care Q din Decision Tree a determinat alegerea]
```

**Exemplu**:
```markdown
### Model Setting
Model: gpt-5.3-codex-spark | Reasoning: —
Routing: Q2→1 fișier <100 linii → Q3→verificabil rapid (template)
```

## 5. Nightly Audit (automatizat)

**Când**: Zilnic la 02:00 EET (după nightly audit general)
**Script**: `~/.nexus/procedures/codex-routing-audit.sh`
**LaunchAgent**: `com.genie.codex-routing-audit`

### Ce verifică:

```bash
# 1. Parsează genie-to-codex.md
#    Extrage: task_id, model, reasoning, task_description
# 2. Per fiecare task din ultimele 24h:
#    - Aplică Decision Tree pe task_description
#    - Compară model ales vs model recomandat
#    - Flaggează discrepanțe
# 3. Output: JSON report
```

### Output format:

```json
{
  "timestamp": "2026-02-27T02:00:00+02:00",
  "tasks_audited": 5,
  "correct_routing": 4,
  "misrouted": [
    {
      "task_id": "m4-148",
      "model_used": "gpt-5.3-codex",
      "model_recommended": "gpt-5.3-codex-spark",
      "reason": "Single-file boilerplate, verificabil rapid",
      "waste_estimate": "~3x slower than needed"
    }
  ],
  "spark_candidates_missed": 1,
  "security_violations": 0
}
```

### Acțiuni pe baza audit-ului:

| Rezultat | Acțiune |
|----------|---------|
| 100% correct | Log OK, no action |
| 1-2 misroutes (minor) | Log warning, include în morning briefing |
| Misroute pe security task | **ALERT TELEGRAM** + flag în session |
| >30% misroute rate | **ALERT** + recalibrare Decision Tree |

## 6. Monthly Benchmark (manual trigger sau cron)

**Când**: Prima zi a lunii (sau la cerere)
**Frecvență**: Lunar (ajustabil după primele 3 luni)
**Script**: `~/.nexus/procedures/codex-model-benchmark.sh`

### Test Suite (5 tasks standard):

| # | Task | Expected Model | Măsurăm |
|---|------|---------------|---------|
| 1 | "Audit this 300-line bash script for bash 3.2 compatibility" | gpt-5.4 | Findings quality, false positives |
| 2 | "Security review: find injection vectors in this API handler" | gpt-5.4 | Findings depth, accuracy |
| 3 | "Lint/syntax review on this 80-line helper script" | gpt-5.3-codex-spark | Speed, syntax accuracy |
| 4 | "Audit a 6-file daemon pipeline for dependency and state issues" | gpt-5.4 | Cross-file consistency, depth |
| 5 | "Smoke-review a small JSON/plist config change" | gpt-5.3-codex-spark | Speed, low-noise findings |
| 6 | "Re-audit after targeted fix on previous HIGH findings" | gpt-5.4 | Regression detection, closure accuracy |

### Metrici per model:

```json
{
  "model": "gpt-5.3-codex-spark",
  "benchmark_date": "2026-03-01",
  "tasks_run": 2,
  "avg_time_seconds": 8.3,
  "correctness_rate": 0.90,
  "tokens_used": 1847,
  "cost_equivalent": "$0.00"
}
```

### Acțiuni post-benchmark:

| Situație | Acțiune |
|----------|---------|
| Model nou apare (ca Spark) | Adaugă la routing table + benchmark |
| Model degradat (correctness < 80%) | Downgrade în routing, switch la alternativă |
| Spark depășește 5.2 pe task standard | Expand Spark scope în Decision Tree |
| Benchmark stabil 3 luni | Reduce la quarterly |

## 7. Model Discovery (săptămânal)

**Când**: Duminică noapte (03:00 EET)
**Cum**: `codex --help 2>&1` + check changelog OpenAI
**Script**: `~/.nexus/procedures/codex-model-discovery.sh`

### Ce verifică:

```bash
# 1. Listează modelele disponibile în Codex CLI
# 2. Compară cu lista din §2 (acest document)
# 3. Dacă model nou:
#    - Web search pentru capabilități
#    - Propune slot în routing table
#    - Alert Telegram: "Nou model Codex: [name]. Benchmark?"
# 4. Dacă model deprecated:
#    - Verifică dacă e folosit în routing
#    - Propune replacement
#    - Alert Telegram
```

## 8. Cortex Logging (la fiecare routing decision)

**Când**: La fiecare brief trimis la Codex
**Collection**: `procedures`
**Tag**: `model-routing`

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"ROUTING: [task_id] [task_type] → [model] [reasoning] | Q: [decision_path]\",
  \"collection\":\"procedures\",
  \"metadata\":{\"type\":\"model-routing\",\"task_id\":\"[ID]\",\"model\":\"[MODEL]\",
  \"reasoning\":\"[LEVEL]\",\"task_type\":\"[TYPE]\",\"decision_path\":\"[Q1→Q2→...]\",
  \"source_agent\":\"genie-opus\",\"genie_visible\":true}}'"
```

**Beneficiu**: După 30+ entries, putem analiza pattern-uri:
- Care tip de task merge cel mai des la care model?
- Unde avem cele mai multe misroutes?
- Unde Spark a fost sufficient vs unde a trebuit escalat?

## 9. Enforcement Loop Summary

```
┌─────────────────────────────────────────────────┐
│              ENFORCEMENT LOOP                    │
│                                                  │
│  PRE-QUEUE (fiecare brief):                     │
│    □ 5-point checklist (§4)                     │
│    □ Header format cu routing justification     │
│    □ Cortex log routing decision (§8)           │
│                                                  │
│  NIGHTLY (02:00 EET):                           │
│    □ Audit toate task-urile din ziua respectivă  │
│    □ Compară model ales vs Decision Tree         │
│    □ Alert pe misroutes (§5)                    │
│                                                  │
│  WEEKLY (duminică 03:00):                       │
│    □ Model discovery — modele noi/deprecated     │
│    □ Update routing table dacă e cazul          │
│                                                  │
│  MONTHLY (prima zi a lunii):                    │
│    □ Benchmark 5 tasks standard (§6)            │
│    □ Compare performance vs luna anterioară      │
│    □ Ajustează Decision Tree dacă e nevoie      │
│    □ Report → Cortex + morning briefing          │
│                                                  │
│  CORTEX (permanent):                            │
│    □ Fiecare routing decision → logged           │
│    □ Pattern analysis la 30+ entries            │
│                                                  │
└─────────────────────────────────────────────────┘
```

### WHERE
- WISH Step H (Flux B și C) — la fiecare brief Codex, gate pre-queue (§4)
- Nightly audit (02:00 EET) — scan automat task-uri din ziua respectivă
- Monthly benchmark — prima zi a lunii

### WHEN
- La FIECARE brief scris în `~/.codex/genie-to-codex.md` → 5-point checklist (§4)
- Daily 02:00 → `codex-routing-audit.sh` compară model ales vs Decision Tree
- Monthly → benchmark 5 tasks standard, compare vs luna anterioară

### HOW (violation detection)
- Brief fără model justification în header → violation la nightly audit
- Model ales != model recomandat de Decision Tree (§3) → misroute flagged
- Token usage trending peste budget → failsafe alert
- Runner: `codex-routing-audit.sh` (nightly), Genie manual la WISH Step H, monthly benchmark

### CONNECT
- COST-H-001 (hard rule) → routing decision obligatorie
- DEV-H-004 (hard rule) → 20K threshold evaluation
- `memory/rules/model-routing-table.md` → sursa de adevăr pentru routing
- `memory/codex-brief-template.md` → model field obligatoriu în header
- `procedure-health.json` → entry `codex-model-routing`

### VERIFY
- [ ] Model ales se potrivește cu Decision Tree (§3)?
- [ ] Reasoning level justificat?
- [ ] Security check executat?
- [ ] Cortex log salvat cu routing decision?
- [ ] VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] CODEX-MODEL-ROUTING | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Codex Model Routing" | FORGE ✓ | rule: COST-H-001 | v1.0`

## 10. Dependențe

- `~/.codex/genie-to-codex.md` — sursa de task-uri de auditat
- `memory/rules/model-routing-table.md` — sursa de adevăr pentru routing
- Cortex VPS (100.81.233.9:6400) — logging + pattern analysis
- Codex CLI — model discovery
- Telegram relay — alerting

## 11. Enforcement Registry Entry

**Adaugă în `rules-hard.md` META-H-002 Enforcement Registry:**

```
| COST-H-001-ROUTING | Codex model routing | Pre-queue 5-point gate + nightly audit + monthly benchmark | codex-routing-audit.sh | 2026-02-27 |
```
