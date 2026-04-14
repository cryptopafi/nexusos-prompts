---
type: procedure
created: 2026-03-22
status: active
slug: promptforge-daily-audit
tags: [procedure, nexus]
---

> **⚠️ DEPRECATED (2026-03-22)** — This procedure has been absorbed into **AUDIT-PRO** (`~/.nexus/audit/AUDIT-PRO.md`). Do not use. Use `/audit` command instead.

# PROMPTFORGE DAILY AUDIT — Procedură Standard de Operare

**Status**: DEPRECATED | Absorbed into AUDIT-PRO v1.1 SOL integration + audit-discover.py auto-queue (`~/.nexus/audit/AUDIT-PRO.md`) on 2026-03-21. Use `/audit` + SOL auto-discover instead.
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: PROMPT-H-002 (Silent Mode Default)
**Enforcement**: Daily la sfârșitul fiecărei sesiuni active + nightly audit

---

## 1. Scope

Verifică zilnic dacă Genie a respectat regulile PromptForge în TOATE interacțiunile din ziua respectivă, inclusiv și mai ales conversațiile directe cu Pafi.

Acoperă:
- Silent Mode aplicat corect? (optimizare invizibilă pe fiecare cerere non-trivială)
- Întrebări inutile puse? (SCOPE pe input clar = violation)
- Termeni vagi lăsați neoptimizați?
- Delegări la subagenti/Codex cu prompt-uri slabe?
- Cortex patterns utilizate unde erau disponibile?

## 2. Trigger

**Când**: La finalul fiecărei sesiuni active (înainte de /sync sau exit)
**Fallback**: Dacă sesiunea se încheie fără audit → nightly audit la 01:00 EET recuperează

## 3. Audit Checklist (8 puncte)

### A. Conversații cu Pafi

```
□ A.1 SILENT MODE: Pentru fiecare cerere non-trivială de la Pafi,
      am aplicat optimizare silent? (specificity boost, constraint anchoring,
      eliminare termeni vagi)
      PASS = optimizat invizibil pe ≥90% din cereri
      FAIL = am lăsat cereri ambigue neoptimizate

□ A.2 ZERO ÎNTREBĂRI INUTILE: Am pus întrebări când puteam deduce din
      context (sesiune, memorie, Cortex, CLAUDE.md)?
      PASS = max 1-2 întrebări pe toată sesiunea, doar când info CRITICĂ lipsea
      FAIL = am pus întrebări pe cereri cu context suficient

□ A.3 FOLLOW-UP QUALITY: Când am cerut clarificări, erau specifice
      și acționabile? (nu "cum vrei?" ci "X sau Y?")
      PASS = fiecare întrebare avea opțiuni concrete
      FAIL = întrebări deschise/vagi
```

### B. Delegări la Subagenti

```
□ B.1 PROMPT STRUCTURE: Fiecare prompt delegat avea:
      ROLE + INPUT + TASK + OUTPUT FORMAT + BOUNDARIES?
      PASS = structura completă pe ≥90% din delegări
      FAIL = prompt-uri vagi ("look into X")

□ B.2 CONTEXT TRIMMING: Am trimis doar context relevant,
      nu MEMORY.md complet sau conversația întreagă?
      PASS = context sub 50% din prompt budget
      FAIL = context dump fără trimming
```

### C. Codex Briefs

```
□ C.1 BRIEF QUALITY: Fiecare brief Codex era self-contained,
      cu context, task clar, output format, testing criteria?
      PASS = brief template complet
      FAIL = brief vag sau incomplet

□ C.2 ROUTING CORRECT: Modelul ales per CODEX-MODEL-ROUTING-PROCEDURE?
      PASS = routing justificat conform Decision Tree
      FAIL = model ales fără justificare
```

### D. Meta

```
□ D.1 CORTEX PATTERNS: Am verificat Cortex pentru patterns similare
      înainte de a compune prompt-uri de la zero?
      PASS = Cortex search pe ≥70% din task-uri non-triviale
      FAIL = prompt-uri from scratch când patterns existau
```

## 4. Scoring

| Puncte PASS | Rating | Acțiune |
|-------------|--------|---------|
| 8/8 | EXCELLENT | Log OK, no action |
| 6-7/8 | GOOD | Log findings, minor self-correction |
| 4-5/8 | NEEDS IMPROVEMENT | Document gaps + create fix plan |
| 0-3/8 | POOR | Full review + Opus audit + Pafi notification |

## 5. Output Format

```
[PROMPTFORGE AUDIT — {date}]
Score: {N}/8 — {rating}
Session: {session_id or machine+time}
Duration: {hours}h, {N} interactions audited

PASS: A.1, A.2, B.1, B.2, C.1, C.2, D.1
FAIL: A.3 — "Am pus 3 întrebări deschise pe cereri cu context suficient"

Findings:
- {finding 1}
- {finding 2}

Actions:
- {action if needed}
```

## 6. Cortex Logging

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"PROMPTFORGE AUDIT {date}: {score}/8 {rating}. PASS: {list}. FAIL: {list}. Findings: {summary}\",
  \"collection\":\"procedures\",
  \"metadata\":{\"type\":\"audit-result\",\"audit_type\":\"promptforge-daily\",
  \"score\":{N},\"rating\":\"{rating}\",
  \"tags\":[\"promptforge\",\"audit\",\"prompt-h-002\"],
  \"source_agent\":\"genie-opus\",\"genie_visible\":true}}'"
```

## 7. Nightly Fallback

Dacă sesiunea se încheie fără audit manual, nightly audit la 01:00 EET:
- Citește session log (JSONL)
- Parsează interacțiuni Pafi
- Aplică checklist automat
- Scrie raport în `~/.nexus/procedures/audit-reports/promptforge-{date}.json`
- Include în morning briefing

## 8. Enforcement Loop

```
┌──────────────────────────────────────────────────┐
│        PROMPTFORGE AUDIT ENFORCEMENT              │
│                                                   │
│  END-OF-SESSION (manual):                        │
│    □ Run 8-point checklist (§3)                  │
│    □ Score + log findings                        │
│    □ Cortex save (§6)                            │
│    □ Actions if score <6                         │
│                                                   │
│  NIGHTLY (01:00 EET, fallback):                  │
│    □ Parse session log                           │
│    □ Auto-apply checklist                        │
│    □ Report in morning briefing                  │
│                                                   │
│  WEEKLY (Sunday):                                │
│    □ Aggregate week scores                       │
│    □ Trend: improving or degrading?              │
│    □ Pattern analysis: which checks fail most?   │
│    □ Update procedures if systematic fails       │
│                                                   │
│  CONNECTS TO:                                    │
│    - PROMPT-H-002 (source rule)                  │
│    - META-H-001 (procedure health)               │
│    - QUAL-H-002 (auto-learning)                  │
│    - ERR-H-001 (errors during prompting)         │
│                                                   │
└──────────────────────────────────────────────────┘
```

## 9. Dependențe

- Session logs (JSONL) — pentru nightly fallback parse
- Cortex VPS — logging + pattern history
- Nightly audit script — integrare cu `nightly-audit.sh`
- Morning briefing — include PromptForge score
