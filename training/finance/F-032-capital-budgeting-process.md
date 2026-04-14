---
type: procedure
created: 2026-03-22
status: active
slug: f-032-capital-budgeting-process
tags: [procedure, nexus]
---

# The Capital Budgeting Process (Organizational) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definirea și aplicarea procesului organizațional de capital budgeting de la identificarea oportunităților până la implementare și monitorizare post-investiție.

---

## 1. Problema

Fără un proces organizațional clar, investițiile sunt aprobate ad-hoc, cu criterii inconsistente și fără monitorizare. Această procedură acoperă situațiile în care un CFO sau board trebuie să structureze sau să auditeze procesul de alocare a capitalului pe termen lung, de la pipeline de proiecte la governance și post-mortem.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificare și catalogare oportunități de investiție
Stabilește canalele prin care intră propunerile de investiție: strategic planning (top-down), departamente operaționale (bottom-up), M&A team. Clasifică propunerile pe tipuri: strategice (new markets/products), operaționale (eficiență/automatizare), de înlocuire (asset replacement), de conformitate (regulatory/safety). Creează un pipeline tracker cu toate propunerile primite, statutul, și sponsorul intern.

### Pas 2: Definire criterii de evaluare și hurdle rate
Stabilește criteriile obligatorii: NPV > 0, IRR > Hurdle Rate (WACC + risk premium), Payback < pragul politicii (ex: max 5 ani). Definește criterii calitative: aliniere strategică (scor 1-5), risc de execuție (Low/Medium/High), sinergii cu activitățile existente. Setează hurdle rate diferențiat pe tipuri de risc: core business = WACC, adjacențe = WACC+3%, noi piețe = WACC+5%.

### Pas 3: Screening preliminar
Fiecare propunere de investiție trece prin un screening de 1-2 pagini: problema/oportunitatea, investiția estimată, beneficiile așteptate, și un calcul rough NPV/IRR (nu model detaliat). Investment Committee face screening în max 2 săptămâni: Respins / Progresează la analiză detaliată / Solicită informații suplimentare. Documentează motivația pentru fiecare decizie.

### Pas 4: Analiză financiară detaliată (model conform F-031)
Proiectele care trec screening-ul primesc resurse pentru construirea modelului complet (F-031). Analistul financiar responsabil construiește modelul cu scenarii, sensitivity, și prezentare stakeholders. Due diligence tehnic și operațional (validarea ipotezelor cu departamentele relevante). Business case final: document de max 10 pagini + Excel model.

### Pas 5: Prezentare și aprobare Investment Committee
Prezintă business case-ul Investment Committee (IC) format din CFO, CEO, și, dacă relevant, Board. IC evaluează: soliditatea financiară (model F-031), riscurile identificate și mitigarea lor, resursele necesare (capital + HR + timp management), impactul asupra strategiei. Decizie formală cu criteriu de vot definit (ex: 2/3 aprobare). Documentează condițiile de aprobare.

### Pas 6: Implementare cu milestone tracking
Post-aprobare: creează un project charter cu milestones, buget detaliat, și responsabil. Setează check-in-uri trimestriale: actual CapEx vs. budget, status milestone-uri, revised forecast. Alertează IC dacă există depășire >10% sau deviere semnificativă de la plan. Documentează change requests dacă scope-ul se modifică.

### Pas 7: Post-Investment Review (PIR)
La 12 luni și 24 luni de la finalizarea proiectului: compară actual vs. ipoteze din business case (revenue, costuri, FCF, NPV realizat). Identifică ce a funcționat și ce nu — lessons learned documentate. PIR-urile informează îmbunătățirea procesului de capital budgeting pentru ciclul următor. Rezultatele PIR sunt parte din evaluarea performanței sponsorului de proiect.

---

## 3. Cortex Logging

```json
{
  "text": "The Capital Budgeting Process (Organizational) executat: pipeline structurat, criterii definite, governance stabilit, proces PIR implementat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-032",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["capital-budgeting", "investment-process", "governance", "corporate-finance", "intermediate"],
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
La fiecare execuție a procedurii în context real sau training

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Aprobare fără criterii financiare explicite (NPV/IRR) → violation
- Lipsă Post-Investment Review în proces → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance
- F-031 → modelul financiar pentru analiza detaliată

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-032 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "The Capital Budgeting Process (Organizational)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| F-031 Capital Budgeting Full Model | Analiza financiară detaliată |
| Investment Committee (IC) | Organism de decizie |
| Pipeline tracker (Excel/software PM) | Urmărire propuneri |
| Post-Investment Review template | Framework evaluare ex-post |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Timp de screening preliminar | ≤ 2 săptămâni |
| Proiecte cu PIR efectuat | 100% din proiectele finalizate |
| Acuratețe forecast vs. actual (PIR) | Deviație documentată și analizată |
