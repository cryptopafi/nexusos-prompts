---
type: procedure
created: 2026-03-17
status: active
slug: codex-brief-prewrite-checklist
tags: [procedure, nexus]
---

# CODEX BRIEF PRE-WRITE CHECKLIST — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 1.0
**Regulă asociată**: CODEX-H-001 + DEV-H-013
**Scope**: Previne over-specification și format incorect în briefurile Genie→Codex

---

## 1. Problema

Genie scrie briefuri Codex cu cod complet inline (over-specification), violând template-ul v2.1 și regula DEV-H-013 (Genie NU scrie cod). Fără checklist, briefurile sunt ad-hoc: wall of text, lipsă Goal/Constraints/Safety, cod în loc de descriere outcome.

Situații acoperite:
- Orice task scris în `~/.codex/genie-to-codex.md`
- Orice brief pentru Codex daemon sau manual execution
- Decompoziție de task-uri mari în sub-tasks

---

## 2. Procedura

### Pas 1: LOAD TEMPLATE
Citește `memory/codex-brief-template.md` (v2.1 curent). Nu presupune că știi formatul din memorie.

### Pas 2: VERIFY STRUCTURE
Verifică că brieful conține toate secțiunile obligatorii pentru audit briefs read-only:
- [ ] `### Prompt pentru Codex` (header obligatoriu — daemon parsează DOAR asta)
- [ ] **Goal** (1 propoziție, formulată ca audit finding / evaluare, nu implementare)
- [ ] **Context** (fișiere, stare curentă, decizii anterioare)
- [ ] **Type**: `audit-tehnic (read-only)`
- [ ] **Scope** (fișiere + volum estimat, dacă aplicabil)
- [ ] **NPLF focus** sau rubrică de scoring relevantă
- [ ] **Steps** (numerotate, cu checkpoint per pas; read + score + report, nu build/fix)
- [ ] **Output** (schema JSON sau format structurat exact, conform planului de audit)
- [ ] **Success Criteria** (checkbox-uri testabile pentru audit complet și raport valid)
- [ ] **Constraints** (ce NU trebuie făcut)
- [ ] **Safety** (rate limits, costuri, riscuri — dacă aplicabil)

### Pas 3: ANTI-PATTERN CHECK
- [ ] Brief < 100 linii? (dacă nu → decompose sau scurtează scope-ul)
- [ ] Zero cod inline > 10 linii? (descrie CE trebuie auditat, nu CUM să fie implementat)
- [ ] Zero cerințe de scriere/modificare cod? (Codex este read-only în acest flux)
- [ ] Goal, Steps și Success Criteria sunt formulate ca `read + score + report`, nu build/fix/deploy
- [ ] Output cere JSON schema sau structură audit explicită, nu changelog de implementare
- [ ] Model selectat conform routing table și rolului curent Codex audit-only? (COST-H-001)

### Pas 4: SELF-CHECK
Întreabă-te:
- "Ar înțelege un auditor tehnic extern ce trebuie citit, ce rubrică folosește și ce format trebuie să livreze?"
- "Este clar că task-ul este audit-tehnic (read-only), nu implementare?"

Dacă nu → rescrie mai clar, nu adăuga cod sau instrucțiuni de build.

### Pas 5: ASSIGN NUMBER
Înainte de a scrie, determină corect următorul task number:
1. `grep "## Task m4-" ~/.codex/genie-to-codex.md | tail -5` — ultimele brief-uri din queue
2. `grep "## m4-" ~/.codex/codex-to-genie.md | tail -5` — ultimele delivery-uri
3. Compară TOATE numerele m4-NNN găsite. Următorul = MAX(toate) + 1
4. NU reutiliza un număr existent — chiar dacă brief-ul anterior a fost rescris sau repurposed

**De ce**: m4-403 a fost reutilizat (SMSads audit → NexusOS audit), cauzând confuzie în tracking. m4-405 a fost asignat atât unui brief manual cât și daemon-ului.

### Pas 6: WRITE
Abia acum scrie brieful în `~/.codex/genie-to-codex.md`.

---

## 3. Cortex Logging

```json
{
  "text": "Codex brief m4-NNN written following CODEX-H-001 template. Checklist: LOAD-VERIFY-WRITE passed.",
  "collection": "procedures",
  "metadata": {
    "type": "codex-brief-log",
    "procedure": "CODEX-BRIEF-PREWRITE-CHECKLIST",
    "rule_id": "CODEX-H-001",
    "tags": ["codex", "brief", "checklist"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (Flux B și C) — orice moment în care Genie creează un brief Codex
- Post-H skill extraction — verificare retrospectivă

### WHEN
- La FIECARE task scris în `~/.codex/genie-to-codex.md`
- Fără excepții, indiferent de urgență sau complexitate

### HOW (violation detection)
- Brief > 100 linii → violation (wall of text)
- Brief conține cod inline > 10 linii → violation (over-specification)
- Brief fără `### Prompt pentru Codex` header → daemon îl ignoră (silent failure)
- Brief fără Goal sau Success Criteria → delivery ambiguă
- Task number collision (same m4-NNN used for 2 different tasks) → violation (ASSIGN NUMBER step skipped)
- Runner: Genie self-check (Pas 3-5), nightly audit `test-all-procedures.js`, Pafi review

### CONNECT
- CODEX-H-001 (hard rule) → template obligatoriu, LOAD-VERIFY-WRITE
- DEV-H-013 (hard rule) → Genie NU scrie cod, doar brief
- `/codex` skill (`~/.claude/skills/codex/SKILL.md`) → primary invocation path for this procedure
- `memory/codex-brief-template.md` v2.1 → sursa de adevăr pentru format
- `codex-daemon.sh` → parsează `### Prompt pentru Codex` header
- `procedure-health.json` → entry `codex-brief-prewrite-checklist`

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| codex-brief-template.md | Sursa de adevăr format | `memory/codex-brief-template.md` |
| codex-daemon.sh | Parsează și execută briefs | `~/.openclaw/scripts/codex-daemon.sh` |
| genie-to-codex.md | Fișierul de queue | `~/.codex/genie-to-codex.md` |
| model-routing-table.md | Selecție model corect | `memory/rules/model-routing-table.md` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Brief length | Linii per brief | < 100 |
| Code inline | Linii cod în brief | 0 (max 10) |
| Template compliance | Secțiuni prezente / total | 100% |
| Rework rate | Briefuri rescrise după feedback | < 10% |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există: CODEX-H-001 + DEV-H-013
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT
- [x] Procesul din WHERE (WISH Step H) referențiază această procedură
- [x] Entry adăugat în `procedure-health.json`
- [x] Salvat în Cortex
- [x] Descrie CE nu CUM (zero cod inline)

---

## Lesson Learned (2026-02-27)

m4-153 și m4-154 scrise cu ~280-300 linii cod complet. Pafi a observat, nu Genie. Fix: rescrise la ~70-75 linii, zero cod inline. Pattern: presupunerea că știi un format din memorie → greșeli. Citește sursa de adevăr MEREU.
