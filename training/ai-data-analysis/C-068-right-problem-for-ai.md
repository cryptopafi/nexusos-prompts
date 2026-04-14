---
type: procedure
created: 2026-03-22
status: active
slug: c-068-right-problem-for-ai
tags: [procedure, nexus]
---

# Identify Right Problem Types for AI (Trustworthy AI) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea rapidă a oricărei probleme pentru a determina dacă AI poate oferi o soluție de încredere, prevenind utilizarea AI în contexte nepotrivite sau cu risc înalt.

---

## 1. Problema

Nu toate problemele sunt potrivite pentru AI. Utilizarea AI pentru probleme cu ambiguitate înaltă, date insuficiente sau consecințe grave ale erorii poate produce daune mai mari decât lipsa AI. Această procedură oferă un framework simplu de decizie GO/NO-GO pentru orice problemă candidată la soluție AI.

Situații acoperite:
- Evaluarea rapidă a unei solicitări AI ad-hoc
- Decizia de a folosi AI vs. metodă tradițională pentru o analiză
- Screening inițial al problemelor înainte de investiție în ACHIEVE (C-062)
- Training pentru a dezvolta intuiția de problem-fitness pentru AI

> **Safety Note**: Utilizarea AI fără evaluarea explicitată a riscurilor poate produce daune în contexte high-stakes (medicale, financiare, juridice). Această procedură este obligatorie înainte de deploymentul oricărui sistem AI cu impact asupra utilizatorilor reali. AI rămâne instrument de suport — decizia finală aparține specialistului uman în scenarii cu risc înalt.

---

## 2. Procedura

### Pas 1: Categorize — Clasifică tipul fundamental al problemei
Identifică categoria principală: (a) **Pattern Recognition** — AI excelent (clasificare imagini, spam, anomalii); (b) **Generation** — AI bun cu supervision (text, cod, design); (c) **Classification** — AI excelent dacă există date de training; (d) **Prediction** — AI bun dacă datele istorice sunt suficiente; (e) **Optimization** — AI bun pentru spații de soluții mari; (f) **Judgment/Ethics** — AI inadecvat singur, necesită uman.

### Pas 2: Check Data — Verifică disponibilitatea și calitatea datelor
Răspunde la 3 întrebări: (1) Există suficiente date reprezentative? (sub 100 exemple pentru ML supervised = risc); (2) Datele sunt curate și recente? (date vechi/biased = output degradat); (3) Există ground truth verificabil? (fără ground truth = imposibil de validat output). Dacă oricare răspuns este NO → risc ridicat, continuă cu precauție.

### Pas 3: Assess Ambiguity — Evaluează nivelul de ambiguitate
Problemele cu ambiguitate înaltă necesită judecată umană. Scorează: (1-5): 1 = problemă complet definită cu criterii obiective clare; 5 = problemă deschisă cu criterii subiective și context emoțional/relațional. Score ≥4 → AI singur inadecvat, necesită human-in-the-loop obligatoriu.

### Pas 4: Evaluate Risk — Evaluează toleranța la eroare
Determină consecința unui output AI greșit: **Low stakes** (draft de email, brainstorming) → AI autonom OK; **Medium stakes** (analiză pentru decizie internă) → AI cu review uman; **High stakes** (decizie financiară majoră, medicală, juridică, afectează drepturi) → AI ca instrument de suport, decizia finală la specialist uman.

### Pas 5: Go/No-Go — Ia decizia și documentează
Pe baza pașilor 1-4, decide: **GO** (AI autonom), **GO WITH REVIEW** (AI + validare umană), sau **NO-GO** (metodă tradițională sau AI doar ca research tool). Documentează decizia cu justificare scurtă pentru fiecare criteriu evaluat.

### Pas 6: Document — Înregistrează rațiunea deciziei
Salvează decizia în formatul: problemă | categorie | disponibilitate date | nivel ambiguitate | toleranță risc | decizie | justificare. Folosește ca referință pentru probleme similare viitoare și pentru training echipă.

---

## 3. Cortex Logging

```json
{
  "text": "AI Problem Fitness evaluat pentru [problemă]. Categorie: [tip]. Date: [OK/risc]. Ambiguitate: [1-5]. Risc: [low/medium/high]. Decizie: [GO/GO WITH REVIEW/NO-GO].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-068",
    "domain": "ai-data-analysis",
    "rule_id": "META-H-002",
    "tags": ["trustworthy-ai", "problem-selection", "ai-fitness", "go-no-go", "risk-assessment"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La orice solicitare de utilizare AI pentru o problemă nouă sau cu miză ridicată

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Decizie GO luată fără evaluarea riscului (Pasul 4) → violation critică
- Problemă high-stakes aprobată pentru AI autonom fără review uman → violation critică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-data-analysis
- C-062 ACHIEVE Framework → evaluare aprofundată post-screening

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] C-068 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Identify Right Problem Types for AI (Trustworthy AI)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Descriere problemă completă | Input pentru categorisire |
| Inventar de date disponibile | Input Pasul 2 |
| Criterii de toleranță la risc per domeniu | Referință Pasul 4 |
| Cortex | Logging decizii și raționale |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Decizii GO/NO-GO documentate cu justificare | 100% |
| High-stakes problems cu NO-GO sau REVIEW | 100% |
