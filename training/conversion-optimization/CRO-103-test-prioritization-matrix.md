---
id: CRO-103
title: Test Prioritization Matrix (PIE/ICE)
domain: conversion-optimization
source: Complete Conversion Optimization Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Test Prioritization Matrix (PIE/ICE) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Framework dual PIE + ICE de prioritizare a testelor CRO cu calibration methodology, scoring session protocol, și decision filters. Include diferențele între PIE (additive) și ICE (multiplicative), când se folosește fiecare, și team calibration protocol.

---

## 1. Problema

Backlog-ul CRO crește rapid. Fără un sistem de prioritizare obiectiv, echipele testează ce e ușor sau ce sună interesant, nu ce are cel mai mare impact potențial. Rezultatul: program de testing cu ROI scăzut. PIE și ICE sunt cele mai utilizate frameworks — dar ambele au bias-uri specifice pe care calibrarea le corectează.

Situații acoperite:
- Backlog de teste de prioritizat la începutul unui sprint
- Arbitraj între idei de test cu resurse limitate
- Justificarea priorităților în fața managementului

---

## 2. Procedura

### Pas 1: Popularea backlogului cu ipoteze testabile (minim 20)
Format standard obligatoriu per ipoteză:
"Dacă schimbăm [element specific] din [stare curentă] în [stare nouă], atunci [metrică] va [crește/scădea] cu [X]% deoarece [raționale din research]."

**Surse pentru backlog**:
1. Problem Inventory din CRO-102 (ResearchXL) — SURSA PRINCIPALĂ
2. Best practices CRO per industrie (Baymard Institute, CXL research)
3. Sugestii echipă (product, design, sales, customer success)
4. Competitor research (ce fac ei diferit?)
5. Customer feedback/support tickets
6. Previous test learnings (un test pierdut sugerează de obicei 2 ipoteze noi)

Target: Minim 20 ipoteze în backlog înainte de primul sprint de prioritizare. Sub 20: backlog-ul e prea mic pentru a prioritiza eficient.

### Pas 2: Framework PIE — Potential, Importance, Ease (additive)
Scorează fiecare ipoteză pe 3 dimensiuni (1-10):

| Dimensiune | Întrebarea | Ancorare scoring |
|-----------|-----------|-----------------|
| **P — Potential** | Cât de mult poate crește conversia? | 1-3: <5% uplift estimat. 4-6: 5-15%. 7-9: 15-50%. 10: >50% |
| **I — Importance** | Cât de important e traficul/pagina? | 1-3: <100 vizitatori/zi. 4-6: 100-500. 7-9: 500-2000. 10: >2000 |
| **E — Ease** | Cât de ușor de implementat? | 1-3: >1 săptămână dev. 4-6: 2-5 zile. 7-9: câteva ore. 10: copy change, no dev |

**Score PIE** = (P + I + E) / 3 → Sortează descrescător.

**Când folosești PIE**: Echipe cu backlog nou, fără historical data. PIE este mai simplu și produce un ranking rapid. Dezavantaj: tratează toate dimensiunile egal.

### Pas 3: Framework ICE — Impact, Confidence, Ease (multiplicative)
ICE este varianta cu focus pe VALIDARE — penalizează testele fără evidență:

| Dimensiune | Întrebarea | Ancorare scoring |
|-----------|-----------|-----------------|
| **I — Impact** | Dacă ipoteza e adevărată, cât de mare e câștigul? | Similar cu Potential din PIE |
| **C — Confidence** | Câtă EVIDENȚĂ ai că funcționează? | 1-3: doar gut feel. 4-6: best practice industrie. 7-8: 1 sursă research proprie. 9-10: 2+ surse research proprii (heatmap + survey + user testing) |
| **E — Ease** | Cât de ușor de implementat? | Similar cu PIE |

**Score ICE** = I × C × E (multiplicativ, nu additive!)

**Diferența critică**: ICE este MULTIPLICATIV → un score C de 1 face ICE = aproape 0, indiferent de I și E. Asta penalizează masiv testele fără evidență din research. PIE (additive) ar da unui test cu P=8, I=8, E=8 dar fără research un scor decent — ICE nu.

**Când folosești ICE**: Echipe cu research data (CRO-102 completat), backlog matur. Folosește ICE ca primary, PIE ca secondary/sanity check.

### Pas 4: Calibration session cu echipa (30-45 minute)
Scoring-ul subiectiv produce rezultate inconsistente. Calibration session corectează:

**Protocol**:
1. Fiecare membru scorează INDEPENDENT top 15 ipoteze (15 minute, silent scoring)
2. Afișează scorurile pe ecran — identifică diferențele mari (>3 puncte pe o dimensiune)
3. Discutați diferențele: "Tu ai dat Potential 8, eu am dat 3 — de ce?" (10 minute)
4. Agreați pe definiții SPECIFICE per dimensiune per business (ex: "la noi, Ease 8 = sub 4 ore dev, Ease 3 = 1 sprint complet")
5. Re-scorați cu definițiile agreate (10 minute)
6. Rezultat: scoruri calibrate + buy-in din echipă pentru roadmap

**Beneficiu secundar**: Sesiunea de calibrare creează engagement și ownership pe roadmap-ul de testare — echipa nu simte că le e impus.

### Pas 5: Decision filters (override rules)
Înainte de a finaliza ordinea, aplică FILTRE care pot override scoring-ul:

| Filtru | Regulă |
|--------|--------|
| **Blocant tehnic** | Test depinde de feature nelansat → mută la coada, indiferent de scor |
| **Sezonalitate** | Test relevant doar pentru Black Friday → schedule corect, nu acum |
| **Dependențe** | Test A și B pe aceeași pagină → ruleaza SECVENȚIAL, nu paralel (contamination!) |
| **Revenue risk** | Test cu potențial de a SCĂDEA semnificativ conversia → necesită aprobare management |
| **Quick win** | Test cu scor mediu DAR Ease = 10 (copy change, 10 minute implementation) → rulează imediat, nu bloca pipeline |
| **Strategic** | Test cerut explicit de CEO/product → evaluează, nu ignora — dar scorează obiectiv |

### Pas 6: Roadmap de testare pe 3 luni (Gantt vizual)
Transformă backlogul prioritizat în roadmap concret:

| Sprint | Săpt | Test principal | Test secundar |
|--------|------|---------------|--------------|
| 1 | 1-2 | Test #1 (scor cel mai mare) | Test #3 (pe altă pagină) |
| 2 | 3-4 | Test #2 | Test #5 |
| 3 | 5-6 | Test #4 | - (holiday period, pauză) |
| ... | ... | ... | ... |

**Reguli roadmap**:
- NICIODATĂ 2 teste pe aceeași pagină simultan
- Include buffer de 1 săptămână între sprinturi pentru implementare câștigători
- Vizualizează în Google Sheets (Gantt simplu), Monday.com, sau Asana
- Prezintă stakeholderilor cu justificările PIE/ICE per test

### Pas 7: Revizuirea post-sprint (backlog ca document viu)
După fiecare sprint (2-4 săptămâni):
1. **Re-evaluează scorurile** cu learningurile noi:
   - Test câștigător pe pagina X → CREȘTE confidence (C) pe ipoteze similare pe alte pagini
   - Test pierdut → REDUCE prioritatea ipotezelor înrudite
   - Finding surpriză → generează 2-3 ipoteze NOI
2. **Adaugă ipoteze noi** din: test learnings, noi observații analytics, feedback client, competitor changes
3. **Elimină ipoteze invalidate** de teste anterioare
4. **Re-sortează** backlogul
5. Backlog-ul este un DOCUMENT VIU — nu un plan static. Review la fiecare sprint.

---

## 3. Cortex Logging

```json
{
  "text": "Test prioritization PIE+ICE implementată: backlog 20+ ipoteze în format standard, scoring PIE+ICE calculat, calibration session cu echipa executată, decision filters aplicate, roadmap 3 luni vizualizat și aprobat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-103",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["test-prioritization", "pie-framework", "ice-framework", "cro", "calibration", "roadmap"],
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

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Backlog sub 10 ipoteze → violation
- Scoring fără calibration session → violation
- 2 teste paralele pe aceeași pagină → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minim 20 ipoteze în backlog cu format standard?
- [ ] Calibration session executată cu echipa?
- [ ] Roadmap 3 luni creat și prezentat?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-103 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Test Prioritization Matrix (PIE/ICE)" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Sheets / Notion | Backlog management și scoring |
| PIE/ICE Template (downloadable) | Calcul automat scoruri |
| Gantt tool (Google Sheets/Asana) | Roadmap vizualizare |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Ipoteze în backlog | ≥20 active |
| % ipoteze cu scor PIE/ICE | 100% |
| Calibration session | per sprint |
| Roadmap actualizat | la fiecare sprint |
| ROI program testare | monitorizat trimestrial |
| Win rate pe teste cu ICE Confidence ≥7 | ≥40% (vs. ≥25% overall) |
