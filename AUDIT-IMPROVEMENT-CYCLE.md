---
type: procedure
created: 2026-03-22
status: active
slug: audit-improvement-cycle
tags: [procedure, nexus]
---

> **⚠️ DEPRECATED (2026-03-22)** — This procedure has been absorbed into **AUDIT-PRO** (`~/.nexus/audit/AUDIT-PRO.md`). Do not use. Use `/audit` command instead.

# AUDIT → IMPROVEMENT CYCLE — Meta-Procedură pentru Toate Auditurile

**Status**: DEPRECATED | Absorbed into AUDIT-PRO v1.1 Step 12 Ralph Loop + SOL integration (`~/.nexus/audit/AUDIT-PRO.md`) on 2026-03-21. Use `/audit --auto-fix` instead.
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: META-H-003 (nou)
**Scope**: Guvernează TOATE auditurile din sistem — nu doar raportare, ci consecințe și self-improvement

---

## 1. Principiu

**Audit fără consecință = checkbox mort.**

Fiecare audit produce 3 lucruri obligatorii:
1. **REZULTAT** — scor, findings, status
2. **CONSECINȚĂ** — acțiune concretă declanșată de rezultat
3. **IMPROVEMENT** — schimbare permanentă care previne repetarea

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   AUDIT → REZULTAT → CONSECINȚĂ → IMPROVEMENT → AUDIT  │
│     ↑                                              │    │
│     └──────────────────────────────────────────────┘    │
│                    LOOP PERPETUU                        │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

## 2. Audituri Guvernate (toate sub acest ciclu)

| ID | Audit | Frecvență | Procedură |
|----|-------|-----------|-----------|
| AUD-01 | PromptForge compliance | End-of-session + daily | `PROMPTFORGE-DAILY-AUDIT.md` |
| AUD-02 | Codex Model Routing | Nightly 02:00 + monthly benchmark | `CODEX-MODEL-ROUTING-PROCEDURE.md` |
| AUD-03 | Enforcement Registry completeness | Weekly Sunday | `META-H-001` health check |
| AUD-04 | Error Response tracking | Continuous + weekly aggregate | `ERROR-RESPONSE-PROTOCOL.md` |
| AUD-05 | Skool Reader output | Daily post-run | `SKOOL-PROCEDURE.md` |
| AUD-06 | ECHELON channels health | Daily post-orchestrate | `echelon-orchestrate-m4.sh` |
| AUD-07 | Codex delivery quality | La fiecare livrare | `DEV-H-010` audit gate |
| AUD-08 | Procedure Meta-Test | Daily → weekly | `m4-147` (Codex) |
| AUD-09 | Nightly system audit | Daily 01:00 | `nightly-audit.sh` |

## 3. Ciclul Complet — 6 Faze

### FAZA 1: EXECUTE AUDIT
Rulează auditul conform procedurii specifice. Output standard:

```json
{
  "audit_id": "AUD-01",
  "audit_name": "PromptForge compliance",
  "date": "2026-02-27",
  "score": 6,
  "max_score": 8,
  "rating": "GOOD",
  "checks_passed": ["A.1", "A.2", "B.1", "B.2", "C.1", "C.2"],
  "checks_failed": ["A.3", "D.1"],
  "findings": [
    {
      "check": "A.3",
      "description": "2 întrebări deschise fără opțiuni concrete",
      "severity": "MEDIUM",
      "evidence": "Session line 45: 'cum vrei să fac asta?'"
    }
  ]
}
```

### FAZA 2: CLASIFICĂ REZULTATUL

| Scor % | Rating | Nivel |
|--------|--------|-------|
| 90-100% | EXCELLENT | GREEN |
| 70-89% | GOOD | YELLOW |
| 50-69% | NEEDS IMPROVEMENT | ORANGE |
| 0-49% | POOR | RED |

**Trend tracking** (obligatoriu):
```
Săptămâna curentă vs anterioară:
  ↑ IMPROVING (scor mediu crescut)
  → STABLE (±5%)
  ↓ DEGRADING (scor mediu scăzut)
```

### FAZA 3: CONSECINȚĂ (automată, per nivel)

| Nivel | Consecință Imediată | Consecință Sistemică |
|-------|--------------------|--------------------|
| **GREEN** | Log OK. No action. | Dacă GREEN 4 săptămâni consecutive → reduce frecvența audit (daily→weekly) |
| **YELLOW** | Self-correction note + 1 finding documentat | Review procedura: unde a fost gap-ul? Update dacă neclar. |
| **ORANGE** | Opus audit pe procedura asociată + Codex test pe findings | Creează/update training scenario. Flag în morning briefing 3 zile. |
| **RED** | STOP current work + full review + Pafi notification | Root cause analysis. Procedura rescrisă. Codex regression test. Daily audit forțat 2 săptămâni. |

**Trend consequences**:
| Trend | Consecință |
|-------|-----------|
| ↑ IMPROVING 3 săptămâni | Reduce frecvența dacă e daily. Log success pattern. |
| → STABLE 4 săptămâni pe YELLOW | Escalează: ce blochează trecerea la GREEN? Opus review. |
| ↓ DEGRADING 2 săptămâni | Forțează daily audit + Opus root cause + Codex regression |

### FAZA 4: IMPROVEMENT ACTION

Fiecare finding ORANGE+ generează un **Improvement Ticket**:

```markdown
## IMP-{date}-{seq}: {titlu scurt}

**Sursă**: AUD-{NN} audit din {date}
**Finding**: {descriere}
**Root Cause**: {de ce s-a întâmplat}
**Fix Type**: [PROCEDURE_UPDATE | NEW_GATE | TRAINING_SCENARIO | RULE_CHANGE]

### Acțiune
{Ce trebuie schimbat, concret}

### Verificare
{Cum știm că fix-ul funcționează — ce audit check va trece}

### Status: PENDING | IN_PROGRESS | DONE | VERIFIED
```

**Improvement Types**:

| Fix Type | Ce se schimbă | Cine |
|----------|--------------|------|
| PROCEDURE_UPDATE | Update procedura existentă cu edge case/clarificare | Genie direct |
| NEW_GATE | Adaugă un check nou într-un proces existent | Genie + Opus audit |
| TRAINING_SCENARIO | Creează test case specific pentru Codex regression | Codex brief |
| RULE_CHANGE | Modifică/adaugă regulă HARD/STANDARD | Genie + Opus audit + Pafi approve (dacă HARD) |

### FAZA 5: VERIFY IMPROVEMENT

La următorul ciclu de audit:
```
1. Citește Improvement Tickets DONE din ciclul anterior
2. Verifică: check-ul care a eșuat acum trece?
   → DA: Mark ticket VERIFIED. Score ar trebui să crească.
   → NU: Escalează ticket. Fix-ul nu a funcționat.
         Root cause revizuit. Opus re-audit.
         Dacă eșuează a 2-a oară → restructurare procedură completă.
```

### FAZA 6: LEARN & ADAPT

**Monthly (prima zi a lunii) — Meta-Review**:

```
1. AGGREGATE: Toate auditurile din luna anterioară
   - Score trends per audit type
   - Top 5 findings recurente
   - Improvement tickets: created vs verified vs stuck

2. PATTERNS: Ce zone eșuează sistematic?
   - Același check eșuează >3x/lună → procedura e fundamental greșită
   - Aceeași zonă (prompting, routing, security) → training gap
   - Improvement tickets stuck >2 săptămâni → blocker investigat

3. ADAPT:
   - Proceduri rescrise dacă pattern recurent
   - Audituri noi create dacă zonă neacoperită descoperită
   - Audituri vechi desființate dacă GREEN 3 luni consecutive
   - Frecvențe ajustate per trend

4. SELF-GRADE: Cât de bine funcționează sistemul de audit însuși?
   - Auditurile rulează la timp? (skip rate)
   - Consecințele se aplică? (consecință rate)
   - Improvements se verifică? (verify rate)
   - Score-urile cresc în timp? (improvement rate)
```

Output: `~/.nexus/procedures/audit-reports/monthly-meta-{YYYY-MM}.md`

## 4. Self-Improvement Scoring (Meta-Scor)

Peste toate auditurile, un scor agregat:

```
GENIE IMPROVEMENT INDEX (GII)

Formula:
  GII = (avg_audit_score × 0.4) + (improvement_verify_rate × 0.3) + (trend_score × 0.3)

Unde:
  avg_audit_score = media scorurilor % din toate auditurile active
  improvement_verify_rate = tickets VERIFIED / tickets DONE (%)
  trend_score = 100 dacă ↑, 70 dacă →, 30 dacă ↓

Rating:
  GII ≥ 85 = EXCELLENT — sistem robust, self-improving
  GII 70-84 = GOOD — funcțional, cu margini de îmbunătățire
  GII 50-69 = NEEDS WORK — improvement loop spart undeva
  GII < 50 = ALERT — system degradation, full review
```

**GII se raportează în morning briefing** (dacă se calculează — necesită date din cel puțin 2 săptămâni).

## 5. Enforcement Loop

```
┌───────────────────────────────────────────────────────────┐
│            AUDIT-IMPROVEMENT ENFORCEMENT                   │
│                                                           │
│  PER-AUDIT (la fiecare run):                             │
│    □ Faza 1: Execute audit → output JSON standard         │
│    □ Faza 2: Clasifică (GREEN/YELLOW/ORANGE/RED)         │
│    □ Faza 3: Aplică consecință per nivel (automată!)      │
│    □ Faza 4: Creează Improvement Ticket dacă ORANGE+      │
│    □ Cortex log obligatoriu                              │
│                                                           │
│  NEXT-CYCLE (la următorul audit din aceeași serie):       │
│    □ Faza 5: Verify tickets DONE din ciclul anterior     │
│    □ Check eșuat din nou? → escalează                    │
│                                                           │
│  MONTHLY (prima zi):                                     │
│    □ Faza 6: Meta-review aggregate                       │
│    □ Calculate GII (Genie Improvement Index)             │
│    □ Adapt: frecvențe, proceduri, audituri               │
│    □ Report → morning briefing + Cortex                  │
│                                                           │
│  DETECTION:                                               │
│    - Audit skip (nu a rulat la timp) → flag în nightly   │
│    - Consecință skip (ORANGE+ fără ticket) → violation   │
│    - Improvement ticket stuck >14 zile → auto-escalate   │
│    - GII ↓ 2 luni consecutive → full system review       │
│                                                           │
│  CONNECTS TO:                                             │
│    - META-H-001 (procedure health — weekly check)        │
│    - META-H-002 (enforcement loop — ogni regulă)         │
│    - ERR-H-001 (errors feed into audits)                 │
│    - QUAL-H-002 (auto-learning — findings → skills)      │
│    - QUAL-H-003 (escalation path for lessons)            │
│    - Toate procedurile AUD-01..09                        │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

## 6. Cortex Storage

### Per audit run:
```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"AUDIT {audit_id} {date}: {score}/{max} ({rating}). PASS: {list}. FAIL: {list}. Findings: {summary}. Consequence: {action}. Tickets: {N created}\",
  \"collection\":\"procedures\",
  \"metadata\":{\"type\":\"audit-result\",\"audit_id\":\"{ID}\",
  \"score\":{N},\"max_score\":{M},\"rating\":\"{RATING}\",\"trend\":\"{UP/STABLE/DOWN}\",
  \"tags\":[\"audit\",\"improvement-cycle\",\"meta-h-003\"],
  \"source_agent\":\"genie-opus\",\"genie_visible\":true}}'"
```

### Per improvement ticket:
```bash
# La creare
"text": "IMP-{date}-{seq}: {titlu}. Source: {audit_id}. Root cause: {cause}. Fix: {action}. Status: PENDING"
"metadata": {"type": "improvement-ticket", "status": "PENDING", ...}

# La verificare
# Update Cortex entry sau creează nou cu status VERIFIED/FAILED
```

## 7. Dependențe

- Toate procedurile de audit (AUD-01..09) — produc input
- Cortex VPS — storage pentru scoruri, tickets, trends
- Opus subagent — audit proceduri la ORANGE+
- Codex daemon — regression tests
- Morning briefing — GII report
- Nightly audit — fallback detection
- Improvement tickets — tracked în `~/.nexus/procedures/improvement-tickets/`
