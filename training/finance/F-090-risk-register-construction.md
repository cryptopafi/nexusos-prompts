---
type: procedure
created: 2026-03-22
status: active
slug: f-090-risk-register-construction
tags: [procedure, nexus]
---

# Comprehensive Risk Register Construction — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui registru de riscuri complet care identifică, scorează, asignează proprietari și definește măsuri de mitigare pentru toate riscurile materiale ale unei organizații sau portofoliu.

---

## 1. Problema

Organizațiile care nu au un risk register formalizat gestionează riscurile reactiv — intervin după materializare, nu preventiv. Un registru incomplet (care omite categorii de risc sau nu are ownership clar) oferă o falsă securitate. Această procedură acoperă: identificarea completă pe 5 categorii, scoring Probabilitate × Impact, asignare proprietari, definire controale, și monitorizare trimestrială.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Identificarea tuturor riscurilor materiale pe 5 categorii
Parcurge sistematic toate categoriile pentru a asigura acoperire completă:

**A. Riscuri Strategice**: concurență intensificată, schimbări în preferințele clienților, obsolescență model de business, parteneriate cheie eșuate, M&A nereușit.

**B. Riscuri Operaționale**: eșec sisteme IT critice, erori de proces (manual/automatizat), pierderea personalului cheie, dezastre naturale/BCP, frauda internă, probleme calitate produse.

**C. Riscuri Financiare**: volatilitatea cursului valutar, riscul de lichiditate (nu poți plăti datoriile scadente), riscul de credit al clienților/partenerilor, creșterea dobânzilor, pierderi de investiții.

**D. Riscuri de Conformitate/Regulatorii**: modificări legislative (fiscal, GDPR, sector-specific), amendă sau penalitate regulator, litigii juridice, licențe/autorizații puse în pericol.

**E. Riscuri Reputaționale**: scandal public (media/social media), rating negativ ESG, criză de imagine brand, incident de securitate a datelor clienților.

Listează minimum 3-5 riscuri per categorie.

### Pas 2: Scorarea fiecărui risc — Probabilitate (1-5) × Impact (1-5)
Aplică scale standardizate: **Probabilitate**: 1=Foarte rar (<5%/an), 2=Rar (5-20%/an), 3=Moderat (20-40%/an), 4=Probabil (40-70%/an), 5=Foarte probabil (>70%/an). **Impact**: 1=Neglijabil (<0.1% EBITDA), 2=Minor (0.1-1% EBITDA), 3=Moderat (1-5% EBITDA), 4=Semnificativ (5-15% EBITDA), 5=Catastrofic (>15% EBITDA sau business continuity threat).

Calculează: `Risk Rating = Probabilitate × Impact`. Clasificare: 1-5 = LOW, 6-12 = MEDIUM, 15-25 = HIGH/CRITICAL.

### Pas 3: Calculul rating-ului de risc și prioritizarea
Construiește Heat Map vizuală 5×5 (Probabilitate pe axa Y, Impact pe axa X): zonele roșii (colț dreapta-sus: P≥4, I≥4) = riscuri critice care necesită acțiune imediată. Ordonează toate riscurile descrescător după Risk Rating. Primele 5 riscuri = "Top Risks" care primesc atenție maximă a conducerii. Prezintă heat map în raportul de board trimestrial.

### Pas 4: Asignarea proprietarului de risc (Risk Owner)
Fiecare risc trebuie să aibă UN singur Risk Owner (persoana, nu departamentul). Criteriul de asignare: cine are cel mai mult control asupra evenimentelor care cauzează riscul și resursele pentru mitigare? Documentează: Nume complet și funcție, email de contact, data asignării. Risk Owner este responsabil pentru: implementarea controalelor, raportarea statusului trimestrial, escaladarea dacă riscul se modifică semnificativ.

### Pas 5: Definirea acțiunilor de mitigare și deadline-urilor
Pentru fiecare risc cu Rating ≥ 6 (MEDIUM+), definește: **Strategia de răspuns** — una din 4 opțiuni: (1) Evitare (eliminate activitatea care generează riscul), (2) Reducere (implementează controale care scad probabilitatea sau impactul), (3) Transfer (asigurare, outsourcing, contracte de garantie), (4) Acceptare (risc sub threshold acceptat, monitorizare pasivă). **Acțiuni concrete**: cel puțin 2-3 acțiuni specifice, cuantificabile, cu deadline. **KRI (Key Risk Indicator)**: metrica early warning care semnalează că riscul se materializează.

### Pas 6: Implementarea controalelor și verificarea eficacității
Documentează controalele existente: controale preventive (previn materializarea riscului) vs detective (detectează post-materializare) vs corective (limitează impactul după materializare). Evaluează eficacitatea actuală a controalelor: Eficace/Parțial eficace/Ineficace. Calculează Residual Risk Rating (după aplicarea controalelor). Dacă Residual Risk > Target Risk Appetite → acțiuni suplimentare obligatorii.

### Pas 7: Monitorizare trimestrială și actualizarea registrului
Stabilește cadența: review trimestrial obligatoriu pentru toate riscurile MEDIUM+, review lunar pentru riscurile HIGH/CRITICAL. La fiecare review: actualizează scorurile (risc nou apărut? risc mitizat?), verifică statusul acțiunilor (on track/delayed/completed), adaugă riscuri noi identificate (environment scanează continuu), remove riscuri care au expirat sau au fost eliminate complet. Raportare: Risk Register summary la board trimestrial + top 5 risks cu trend (improving/stable/deteriorating).

---

## 3. Cortex Logging

```json
{
  "text": "Risk Register Construction F-090 executat: 5 categorii de riscuri identificate, Probabilitate x Impact scorat, risk owners asignati, controale definite, monitorizare trimestriala configurata.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-090",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["risk-register", "risk-management", "ERM", "heat-map", "risk-scoring", "controls", "compliance"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-090 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Comprehensive Risk Register Construction" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Risk Register template (Excel/GRC tool) | Documentarea și tracking-ul riscurilor |
| Risk Heat Map vizualizare | Comunicarea riscurilor la nivel de board |
| ISO 31000 / COSO ERM framework | Standard de referință pentru metodologie |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Acoperire categorii risc | 5/5 categorii |
| Riscuri cu Risk Owner asignat | 100% |
| Frecvență review riscuri HIGH | Lunar |
