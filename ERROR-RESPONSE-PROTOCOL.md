---
type: procedure
created: 2026-03-17
status: active
slug: error-response-protocol
tags: [procedure, nexus]
---

# ERROR RESPONSE PROTOCOL — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: ERR-H-001
**Enforcement**: La fiecare eroare, zero excepții

---

## 1. Scope

Orice eroare care apare în operațiunile Genie trebuie capturată, clasificată și standardizată. Scopul: **zero erori repetate fără procedură**.

Tipuri de erori acoperite:
- Runtime (script crash, timeout, syntax error)
- Tool failure (grep flag greșit, API 404, connection refused)
- Logic (routing greșit, format invalid, selector CSS mort)
- Procedural (header lipsă, enforcement loop spart, gate sărită)
- Misroute (model greșit, task la destinație greșită)

## 2. Protocol — 3 Steps

### STEP 1: INVESTIGATE (max 2 minute)

```
1. CE s-a întâmplat?
   → Error message exact (copy-paste, nu parafrazare)
   → Context: ce task rula, ce tool, ce fișier

2. ROOT CAUSE (1-2 propoziții):
   → "X a eșuat pentru că Y"
   → Nu simptomul, ci CAUZA

3. IMPACT:
   → Ce s-a pierdut? (date, timp, task ratat)
   → Cine e afectat? (Pafi, daemon, Codex, altă procedură)
```

### STEP 2: CLASSIFY

#### PATH A — Eroare NOUĂ

Cortex search nu găsește procedură/skill relevantă (score < 0.5).

```
A.1 — Documentează fix-ul aplicat
A.2 — Creează procedură standard:
      - Nume descriptiv
      - Root cause + context
      - Fix step-by-step
      - Enforcement loop: WHEN se aplică, HOW se detectează violarea
      - Conectare la reguli existente (care HARD rule acoperă zona?)
A.3 — Adaugă enforcement entry în META-H-002 Registry (dacă e HARD-worthy)
A.4 — Salvează în Cortex:
      Collection: procedures
      Tags: error-protocol, err-h-001, [domain-tag]
      Metadata: type=error-procedure, severity=[LOW/MEDIUM/HIGH]
A.5 — Anunță: [ERR-H-001: NEW — "{descriere}" → procedure created]
```

#### PATH B — Eroare REPETATĂ

Cortex search găsește procedură/skill relevantă (score >= 0.5).

```
B.1 — Găsește procedura existentă (Cortex ID + location)
B.2 — Investighează REPETAREA:
      - Enforcement loop-ul exista? Funcționa?
      - Gate-ul era în loc corect (WISH step, daemon, script)?
      - Condiție nouă pe care procedura nu o acoperea?
      - Edge case neanticipat?
      Root cause repetare: _______________

B.3 — Opus audit pe procedura existentă:
      Subagent Opus cu prompt:
      "Auditează această procedură. Verifică:
       1. Enforcement loop complet? (WHERE/WHEN/HOW)
       2. Acoperă edge case-ul: [descriere eroare nouă]?
       3. Conectată la Enforcement Registry?
       4. Gaps sau ambiguități?
       Verdict: PASS / FAIL + recomandări concrete"

B.4 — Update procedura:
      - Adaugă noul edge case / condiție
      - Fix enforcement loop dacă era spart
      - Update Cortex entry (store nou cu metadata actualizat)

B.5 — Codex test intensiv:
      Brief în ~/.codex/genie-to-codex.md:
      - Scenariul original care a generat procedura
      - Scenariul NOU care a cauzat repetarea
      - 3-5 variante edge case
      - Regression: verifică că fix-ul nu sparge alte lucruri
      Model: gpt-5.6-sol high (sau gpt-5.3-codex-spark pentru teste simple)

B.6 — Anunță: [ERR-H-001: REPEAT — "{descriere}" → procedure updated + Codex test m4-NNN]
```

### STEP 3: VERIFY (post-fix)

```
□ Eroarea fixată? (reproduce și confirma fix)
□ Enforcement loop acoperă cazul? (WHERE/WHEN/HOW complet)
□ Cortex entry actualizat? (procedură nouă sau update)
□ Dacă PATH B: Codex test brief trimis?
□ Log generat: [ERR-H-001: ...] vizibil în session
```

## 3. Severity Classification

| Severity | Criterii | Acțiune suplimentară |
|----------|---------|---------------------|
| **LOW** | Cosmetic, no data loss, easy workaround | Document only, no Codex test |
| **MEDIUM** | Funcționalitate afectată, workaround exists | Document + fix + Cortex |
| **HIGH** | Data loss, security, sau repetare a 3-a oară | Document + fix + Opus audit + Codex test + Telegram alert |
| **CRITICAL** | Security breach, production down, ireversibil | STOP everything + fix + full audit + Pafi notification |

## 4. Escalation Rules

| Condiție | Escalare |
|----------|---------|
| Eroare LOW apare a 2-a oară | → MEDIUM |
| Eroare MEDIUM apare a 2-a oară | → HIGH (PATH B obligatoriu) |
| Eroare HIGH apare a 2-a oară | → CRITICAL + Opus deep audit pe toată zona |
| 3+ erori diferite în aceeași zonă (într-o sesiune) | → Zone audit cu Opus |
| Enforcement loop confirmat spart | → Fix imediat + Codex regression test |

## 5. Cortex Logging Format

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{\"text\":\"ERR-H-001: [NEW|REPEAT] — [descriere scurtă]\\nRoot cause: [cauza]\\nFix: [ce s-a făcut]\\nProcedure: [path sau Cortex ID]\\nSeverity: [LOW|MEDIUM|HIGH|CRITICAL]\",
  \"collection\":\"procedures\",
  \"metadata\":{\"type\":\"error-procedure\",\"error_type\":\"[NEW|REPEAT]\",
  \"severity\":\"[level]\",\"domain\":\"[zona]\",
  \"tags\":[\"error-protocol\",\"err-h-001\"],
  \"source_agent\":\"genie-opus\",\"genie_visible\":true}}'"
```

## 6. Exemple Concrete

### Exemplu A — Eroare NOUĂ (grep -P pe macOS)

```
STEP 1: INVESTIGATE
  CE: grep -P flag nu există pe macOS BSD grep
  ROOT CAUSE: Genie folosește Bash grep cu Perl regex flag (-P) care e doar GNU grep
  IMPACT: Comanda eșuează, output gol, task întârziat

STEP 2: PATH A — NOUĂ
  A.1: Fix = folosește tool-ul Grep dedicat (ripgrep) nu Bash grep
  A.2: Procedură: "NEVER use Bash for grep/find/cat — use dedicated tools"
  A.3: Entry MEMORY.md + Preferences section
  A.4: Cortex saved
  A.5: [ERR-H-001: NEW — "grep -P pe macOS = BSD, nu GNU" → preference added]
```

### Exemplu B — Eroare REPETATĂ (Codex brief fără header)

```
STEP 1: INVESTIGATE
  CE: m4-146 și m4-147 nu erau procesate de daemon
  ROOT CAUSE: Header "### Prompt pentru Codex" lipsea — aveau "### Context" direct
  IMPACT: 2 task-uri blocate, daemon ignora PENDING tasks

STEP 2: PATH B — REPETATĂ
  B.1: Procedură existentă: CODEX-H-001 (Brief Format Gate)
  B.2: DE CE s-a repetat? Genie a scris brief-ul ad-hoc fără template.
       Enforcement loop CODEX-H-001 nu a fost verificat la scriere.
  B.3: Opus audit: CODEX-H-001 are template dar gate-ul e self-check
       (nu automated). Recomandare: add validation script.
  B.4: Update procedură: emphasis pe "### Prompt pentru Codex" header
  B.5: Codex test: brief validare script (check header presence)
  B.6: [ERR-H-001: REPEAT — "Codex brief missing required header" → CODEX-H-001 updated + Codex test]
```

## 7. Enforcement Loop Summary

```
┌──────────────────────────────────────────────────┐
│           ERROR RESPONSE ENFORCEMENT              │
│                                                   │
│  TRIGGER: Orice eroare, oriunde                  │
│    → STEP 1: Investigate (max 2 min)             │
│    → STEP 2: Classify (NEW → A / REPEAT → B)    │
│    → STEP 3: Verify (fix + enforcement + Cortex) │
│                                                   │
│  DETECTION:                                       │
│    - Genie log [ERR-H-001: ...] la fiecare eroare │
│    - Session review: erori fără log = violation   │
│    - Cortex audit: error count per domain/month   │
│                                                   │
│  ESCALATION:                                      │
│    - 2x same error → severity +1                  │
│    - 3+ errors same zone → Opus zone audit        │
│    - Enforcement loop spart → fix imediat         │
│                                                   │
│  CONNECTS TO:                                     │
│    - META-H-002 (enforcement loop creation)       │
│    - MEM-H-001 (Cortex save)                      │
│    - QUAL-H-002 (auto-learning)                   │
│    - DEV-H-014 (no ad-hoc fixes)                  │
│    - CODEX-H-001 (brief format gate)              │
│                                                   │
└──────────────────────────────────────────────────┘
```

## 8. Dependențe

- Cortex VPS (100.81.233.9:6400) — logging + search
- Opus subagent — audit proceduri existente (PATH B.3)
- Codex daemon — test intensiv (PATH B.5)
- Enforcement Registry — META-H-002 în rules-hard.md
- Telegram relay — alerting pe CRITICAL
