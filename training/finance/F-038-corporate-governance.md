---
type: procedure
created: 2026-03-22
status: active
slug: f-038-corporate-governance
tags: [procedure, nexus]
---

# Corporate Governance and Stakeholder Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiza structurii de corporate governance și a relațiilor cu stakeholderii pentru identificarea riscurilor și oportunităților de îmbunătățire.

---

## 1. Problema

Guvernanța corporativă slabă crește riscul de agency conflicts, fraud, și distrugere de valoare pe termen lung. Această procedură acoperă situațiile de due diligence pre-investiție, evaluare internă, pregătire raport anual, sau ESG scoring unde analistul trebuie să evalueze calitatea governance și alinierea stakeholderilor.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Mapare stakeholders
Identifică toți stakeholder-ii relevanți: Primari (acționari majori, board of directors, management executiv, angajați), Secundari (creditori/bănci, clienți, furnizori, comunitate locală), Reglementatori (SEC/FSA/ASF, autorități fiscale, autorități de concurență). Documentează: interesele fiecăruia, puterea de influență (High/Medium/Low), și alinierea cu obiectivele companiei.

### Pas 2: Analiză interese și putere stakeholders
Construiește o matrice Power-Interest: (1) High Power + High Interest = Manage closely, (2) High Power + Low Interest = Keep satisfied, (3) Low Power + High Interest = Keep informed, (4) Low Power + Low Interest = Monitor. Identifică conflictele de interese principale: management vs. acționari (agency problem), acționari majoritari vs. minoritari, acționari vs. creditori (debt overhang). Cuantifică impactul potențial al fiecărui conflict.

### Pas 3: Review compoziție board of directors
Analizează: dimensiunea board-ului (optim 7-11 membri pentru companii medii), % directori independenți (best practice: >50%), diversitate (gen, expertiză, background geografic), existența comitetelor obligatorii (Audit Committee, Compensation Committee, Nominating/Governance Committee). Evaluează independența reală față de management (tenure lung, relații comerciale, family ties sunt red flags). Benchmarkează față de best practice și peer companies.

### Pas 4: Evaluare mecanisme de governance
Audit Committee: independență, competență financiară a membrilor, relație cu auditorul extern, rotație auditor. Compensation Committee: aliniere remunerație executivi cu performanța pe termen lung (vs. short-term), existența clawback provisions, transparența disclosures. Internal controls: SOX compliance (dacă aplicabil), existența și eficacitatea funcției de audit intern, risk management framework. Whistleblower policy: anonimitate, non-retaliation protection.

### Pas 5: Identificare riscuri de governance
Listează riscurile identificate: concentrare putere la un singur individ (CEO duality), acționar de control cu drepturi speciale de vot (dual-class shares), tranzacții cu părți afiliate (related party transactions — volume și termeni), procese sau investigații reglementare în curs, turnover management ridicat (red flag), concentrare ownership >50% fără protecții pentru minoritari.

### Pas 6: Benchmark față de best practices
Compară față de: Principles of Corporate Governance (OECD), ISS Governance Quality Score, MSCI ESG Rating componenta governance, proxy advisor guidelines (ISS/Glass Lewis). Calculează un scor intern de governance (1-100) pe dimensiunile: Board (30%), Accountability (25%), Shareholder Rights (25%), Transparency (20%). Identifică gap-urile față de best practice.

### Pas 7: Raport de governance și recomandări
Sintetizează: Strengths (ce funcționează bine), Weaknesses (gap-uri față de best practice), Opportunities (îmbunătățiri cu impact ridicat), Threats (riscuri de exploatare a slăbiciunilor). Formulează 3-5 recomandări prioritare cu acțiuni concrete, responsabili, și timeline. Prezintă board-ului sau comitetului de governance.

---

## 3. Cortex Logging

```json
{
  "text": "Corporate Governance and Stakeholder Analysis executat: stakeholders mapați, board evaluat, mecanisme governance revizuite, benchmark efectuat, raport cu recomandări finalizat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-038",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["corporate-governance", "stakeholder-analysis", "ESG", "board-analysis", "corporate-finance", "intermediate"],
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
- Analiză governance fără benchmark față de best practice → violation
- Raport fără recomandări acționabile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-038 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Corporate Governance and Stakeholder Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Raport anual / Proxy Statement (DEF 14A) | Date board și ownership |
| ISS / Glass Lewis governance reports | Benchmark external |
| OECD Principles of Corporate Governance | Standard de referință |
| SEC EDGAR (dacă companie publică) | Filing-uri proxy și ownership |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Stakeholders identificați | Toate categoriile principale |
| Scor governance calculat | Pe minimum 4 dimensiuni |
| Recomandări formulate | 3-5 acțiuni cu responsabili și timeline |
