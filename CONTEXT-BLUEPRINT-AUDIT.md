---
type: procedure
created: 2026-03-22
status: active
slug: context-blueprint-audit
tags: [procedure, nexus]
---

> **⚠️ DEPRECATED (2026-03-22)** — This procedure has been absorbed into **AUDIT-PRO** (`~/.nexus/audit/AUDIT-PRO.md`). Do not use. Use `/audit` command instead.

# CONTEXT-BLUEPRINT-AUDIT — Procedură Standard de Operare

**Status**: DEPRECATED | Absorbed into AUDIT-PRO v1.1 architecture-auditor agent (`~/.nexus/audit/agents/architecture-auditor.md`) on 2026-03-21. Use `/audit` on architecture targets instead.
**Creat**: 2026-02-28
**Versiune**: 1.0
**Regulă asociată**: META-H-001 (Health Check weekly — extinsă pentru context documents)
**Scope**: Audit periodic al CLAUDE.md și fișierelor de context pentru a detecta conținut stale, lipsuri, redundanțe și bloat.

---

## 1. Problema

CLAUDE.md este contextul principal al lui Genie — echivalentul unui "operating system" care se încarcă la fiecare sesiune. Fără audit periodic:
- Secțiuni devin **stale** (informații depășite, versiuni vechi, link-uri moarte)
- Conținut **redundant** se acumulează (aceeași informație în 2+ locuri, duplicare cu fișiere externe)
- Fișierul crește necontrolat (**bloat**) — mai mult context = mai mult noise, mai puțin signal
- Secțiuni noi se adaugă fără a verifica dacă contrazic sau duplică ceva existent (**incoherence**)
- Referințe la fișiere externe (**promptforge.md**, **rules/**, etc.) pot deveni desincronizate

Situații acoperite:
- CLAUDE.md a crescut peste 800 linii și continuă să crească
- Secțiuni v1 coexistă cu v3 (cazul PromptForge — detectat în audit Chapter 1)
- Git merge artifacts rămân nedetectate (cazul `>>>>>>> Stashed changes`)
- Fișiere referențiate nu mai există sau au conținut diferit
- Reguli duplicate între CLAUDE.md inline și rules-hard.md

---

## 2. Procedura

### Pas 1: INVENTORY — Mapare structurală
Listează toate secțiunile H2 și H3 din CLAUDE.md cu line count per secțiune.
Compară cu inventarul precedent (dacă există în Cortex).
Output: tabel cu secțiune | linii | delta vs ultima dată.

### Pas 2: STALENESS CHECK — Conținut depășit
Per fiecare secțiune H2:
- Conține date hardcodate (versiuni, date, numere)? → Verifică dacă sunt actuale
- Conține instrucțiuni care contrazic secțiuni mai noi? → Flag ca stale
- Conține referințe la fișiere/paths? → Verifică că fișierele există
- Conține git merge artifacts (`<<<<<<<`, `=======`, `>>>>>>>`)? → Flag CRITICAL

Output: lista de items stale cu severity (CRITICAL / HIGH / LOW).

### Pas 3: REDUNDANCY CHECK — Duplicări
Identifică conținut duplicat:
- CLAUDE.md inline vs fișiere externe referențiate (promptforge.md, rules-hard.md, etc.)
- CLAUDE.md secțiuni care se repetă sau se contrazic intern
- Informații care ar trebui să fie într-un singur loc (single source of truth)

Regula: dacă o secțiune din CLAUDE.md spune "Full docs: X" dar tot include 50+ linii inline → redundanță. Stub de 10-20 linii e OK, restul trebuie delegat la fișierul extern.

Output: lista de redundanțe cu recomandare (stub/delete/move).

### Pas 4: BLOAT ANALYSIS — Mărime și densitate
- Total linii CLAUDE.md vs target (sub 600 linii = healthy, 600-800 = warning, 800+ = bloated)
- Top 5 cele mai mari secțiuni → candidați la comprimare sau externalizare
- Secțiuni care pot fi mutate la fișiere externe fără pierdere de funcționalitate
- Secțiuni care au fost adăugate recent dar nu sunt încă dovedite utile

Output: recomandări de comprimare cu estimated savings.

### Pas 5: COHERENCE CHECK — Versiuni și referințe
- Toate regulile din CLAUDE.md (H-xxx, S-xxx) există în rules-hard.md sau rules-standard.md?
- Toate procedurile referențiate în CLAUDE.md există în ~/.nexus/procedures/?
- Numerotarea pașilor din secțiuni numerotate e corectă (nu lipsesc pași, nu sunt duplicate)?
- Fișierele din memory/ referențiate în CLAUDE.md au fost actualizate recent?

Output: lista de incoherențe cu severity.

### Pas 6: REPORT — Sinteză și acțiuni
Creează raport structurat:
```
## Context Blueprint Audit Report — {DATE}

### Summary
- Total linii: N (target: <600)
- Stale items: N (C:X H:Y L:Z)
- Redundanțe: N
- Incoherențe: N
- Health score: X/100

### Critical (fix acum)
1. [item]

### High (fix această sesiune)
1. [item]

### Low (backlog)
1. [item]

### Recommended Actions
1. [acțiune cu estimated savings]
```

Salvează raportul în `memory/reports/context-audit-{DATE}.md`.

---

## 3. Cortex Logging

```json
{
  "text": "Context Blueprint Audit {DATE}: CLAUDE.md {N} linii, health score {X}/100, {Y} stale items, {Z} redundanțe. Actions: {summary}",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CONTEXT-BLUEPRINT-AUDIT",
    "rule_id": "META-H-001",
    "tags": ["context-audit", "claude-md", "health-check"],
    "health_score": 0,
    "total_lines": 0,
    "stale_count": 0,
    "redundancy_count": 0,
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- META-H-001 Health Check weekly (Sunday 08:00)
- După orice sesiune care a adăugat >50 linii la CLAUDE.md
- La comanda manuală `audit claude.md` sau `context audit`

### WHEN
- **Periodic**: Weekly (Sunday), ca parte din health check
- **Triggered**: Dacă CLAUDE.md depășește 900 linii → audit obligatoriu înainte de a continua
- **Manual**: Pafi cere explicit

### HOW (violation detection)
- CLAUDE.md > 900 linii fără audit recent (<7 zile) → violation
- Git merge artifacts prezente → violation CRITICAL
- Secțiune stale (referință la fișier inexistent, versiune veche documentată) detectată de test-all-procedures.js → violation
- Runner: `test-all-procedures.js` (daily 02:00) — check line count + merge artifacts
- Runner: Genie self-check la session start dacă CLAUDE.md > 900

### CONNECT
- META-H-001 → extins cu Context Blueprint Audit ca sub-check
- FORGE → această procedură urmează FORGE template
- `test-all-procedures.js` → adaugă check: CLAUDE.md line count + merge artifacts scan
- `procedure-health.json` → entry pentru CONTEXT-BLUEPRINT-AUDIT

### VERIFY (procedural checkpoint — emis ca VK în sesiune)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii 1-6 din §2)
- [ ] Output-ul satisface criteriile din §1? (stale detectat, redundanțe listate, raport generat)
- [ ] VK emis în sesiune? (ambele linii de mai jos vizibile pentru Pafi)
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per procedură FORGE** (per VK-H-001):
1. **Execution VK** (confirmă pașii urmați):
   `✅ [PROC] CONTEXT-BLUEPRINT-AUDIT | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK** (confirmă salvarea în Cortex):
   `✅ [CORTEX] "Context Blueprint Audit {DATE}" | FORGE ✓ | rule: META-H-001 | v1.0`

### MODEL ROUTING (cine execută ce pe această procedură)
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție audit complet | Genie direct (Sonnet) | Auditul necesită session context + CLAUDE.md awareness |
| Remediere stale items | Genie direct | Edit-urile sunt în CLAUDE.md — necesită context |
| Validare automată (line count, merge artifacts) | Deterministic (test-all-procedures.js) | Nu e LLM — e cod |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| CLAUDE.md | Fișierul auditat | `~/.claude/projects/-Users-pafi/CLAUDE.md` |
| promptforge.md | Fișier extern referențiat | `memory/promptforge.md` |
| promptforge-v3.md | Fișier extern referențiat | `memory/promptforge-v3.md` |
| rules-hard.md | Regulile hard referențiate | `memory/rules/rules-hard.md` |
| test-all-procedures.js | Runner automat | `~/.nexus/monitoring/test-all-procedures.js` |
| Cortex KB | Storage pentru rapoarte | `http://localhost:6400/api/store` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| CLAUDE.md line count | Bloat | < 600 linii |
| Stale items | Conținut depășit | 0 CRITICAL, < 3 HIGH |
| Redundanțe | Duplicări | < 2 |
| Health score | Calitate generală | ≥ 80/100 |
| Audit frequency | Cât de des se face | Weekly (≤ 7 zile între audituri) |

---

## Checklist Pre-Publicare (verifică ÎNAINTE de a marca ACTIVE)

- [x] Regulă asociată există în rules-hard.md (META-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent cu cele 3 checks obligatorii
- [x] Procesul din WHERE a fost editat să referențieze această procedură (META-H-001 weekly)
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata: `rule_id`, `type: procedure`, `has_enforcement_loop: true`, `forge_version: "1.3"`
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
