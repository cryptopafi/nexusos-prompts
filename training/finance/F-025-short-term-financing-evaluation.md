---
type: procedure
created: 2026-03-22
status: active
slug: f-025-short-term-financing-evaluation
tags: [procedure, nexus]
---

# Short-Term Financing Source Evaluation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea și selectarea surselor optime de finanțare pe termen scurt pentru acoperirea nevoilor de working capital.

---

## 1. Problema

Alegerea ad-hoc a surselor de finanțare pe termen scurt fără o evaluare structurată duce la costuri excesive și expunere la riscuri de lichiditate. Această procedură acoperă situațiile în care o companie identifică un gap de working capital și trebuie să aleagă între multiple instrumente: linie de credit, factoring, commercial paper, trade credit extins, sau alte surse.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Cuantificare nevoi de finanțare pe termen scurt
Determină suma necesară (din F-024 cash gap analysis), durata necesității (zile / săptămâni), frecvența (one-time vs. recurentă), și urgența (când sunt banii necesari). Calculează working capital gap: `WC Gap = Current Assets - Current Liabilities (fără short-term debt)`. Identifică dacă nevoia este structurală sau sezonieră.

### Pas 2: Inventariere opțiuni disponibile
Listează toate sursele aplicabile: (1) Linie de credit revolving bancară, (2) Factoring / Invoice discounting, (3) Commercial paper (dacă compania are acces la piețe), (4) Trade credit extins (negociere DPO cu furnizori), (5) Supply chain financing / reverse factoring, (6) Overdraft bancar, (7) Intra-group loans. Verifică disponibilitatea fiecărei surse (limite existente, condiții de eligibilitate).

### Pas 3: Evaluare cost (APR total all-in)
Calculează costul efectiv anual (APR) pentru fiecare opțiune: Linie de credit = EURIBOR/SOFR + marjă + commission de neutilizare. Factoring = fee% × (360/zile) = APR echivalent. Trade credit: dacă renunți la discount 2/10 net 30 → APR = (2%/98%) × (360/20) = ~36.7%. Documentează toate fee-urile hidden (arrangement fee, legal fee, insurance).

### Pas 4: Evaluare non-cost (disponibilitate, covenants, flexibilitate)
Evaluează: viteza de activare (ore/zile), covenant-uri asociate (financial covenants, negative pledges), flexibilitate rambursare (revolving vs. fixed term), confidențialitate (factoring vizibil pentru clienți?), impact relație bancară, rating impact. Construiește o matrice de scoring multi-criteriu (cost 40%, disponibilitate 25%, flexibilitate 20%, covenants 15%).

### Pas 5: Ranking și selecție mix optim
Clasifică sursele după scorul total din matricea multi-criteriu. Selectează sursa primară și o alternativă de backup. Determină dacă este optim să folosești un mix (de ex. 60% linie de credit + 40% factoring selectiv). Calculează costul mediu ponderat al mixului selectat.

### Pas 6: Documentare termeni și covenants
Creează un document de sinteză cu: termenii principali ai fiecărei facilități utilizate, toate covenant-urile financiare (cu valorile actuale vs. praguri), datele de revizie și scadențele, persoana responsabilă de monitorizare. Setează alerte calendar pentru datele cheie (reînnoire, reporting, covenant testing).

---

## 3. Cortex Logging

```json
{
  "text": "Short-Term Financing Source Evaluation executat: opțiuni inventariate, APR calculat per sursă, matrice scoring completată, mix optim selectat și documentat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-025",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["short-term-financing", "working-capital", "treasury", "corporate-finance", "intermediate"],
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
- Selecție făcută fără calcul APR comparativ → violation
- Covenant-uri neextrase și nedocumentate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-025 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Short-Term Financing Source Evaluation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Cash gap forecast (F-024) | Dimensionare nevoi finanțare |
| Oferte bancare / term sheets | Date cost și termeni |
| EURIBOR / SOFR rates (Bloomberg) | Rata de referință pentru calcul APR |
| Excel / matrice scoring | Evaluare multi-criteriu |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Surse evaluate | Minimum 3 opțiuni distincte |
| APR calculat | Pentru 100% din opțiunile evaluate |
| Covenant-uri documentate | 100% din facilitățile selectate |
