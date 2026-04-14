---
type: procedure
created: 2026-03-22
status: active
slug: f-027-accounts-payable-management
tags: [procedure, nexus]
---

# Accounts Payable Management Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Optimizarea procesului de gestionare a datoriilor comerciale pentru maximizarea DPO și îmbunătățirea cash conversion cycle fără deteriorarea relațiilor cu furnizorii.

---

## 1. Problema

Plata prematură a furnizorilor imobilizează capitalul de lucru inutil, în timp ce plata tardivă deteriorează relațiile și poate atrage penalități. Această procedură acoperă situațiile în care un CFO sau AP manager trebuie să optimizeze termenele de plată, să evalueze dynamic discounting, și să maximizeze float disponibil.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Calculare și benchmark DPO
Formula: `DPO = (Accounts Payable / COGS) × 365`. Calculează DPO pentru ultimele 4 trimestre. Benchmarkează față de industrie (DPO mai mare este mai bun pentru cash, dar trebuie să fie rezonabil față de practici sectoriale). Calculează DPO pe categorie de furnizori (materii prime, servicii, utilități, capital goods). Identifică furnizorii unde DPO este sub media industriei.

### Pas 2: Review termeni de plată pe furnizori
Creează un registru complet: furnizor, valoare anuală cumpărată, termeni contractuali actuali (net 30/60/90), termeni efectivi (când se plătește în realitate), discount-uri disponibile (2/10 net 30 etc.). Identifică: (a) furnizorii unde se plătește mai devreme decât termenul contractual (oportunitate de extindere), (b) furnizorii unde se plătesc penalități de întârziere.

### Pas 3: Identificare oportunități de extindere termeni
Prioritizează negocierea extinderii termenilor cu furnizorii mari (top 20% după valoare = 80% din AP). Pregătește argumentele de negociere: volum de cumpărare, loialitate, plată consistentă. Propune extensie de 15-30 zile suplimentare în schimbul unui angajament de volum sau exclusivitate. Calculează beneficiul de cash flow al extinderii pentru fiecare furnizor negociat.

### Pas 4: Evaluare dynamic discounting
Dynamic discounting = plătești mai devreme decât termenul contractual în schimbul unui discount. Calculează dacă merită: `APR discount = (discount% / (100% - discount%)) × (365 / (net_days - discount_days))`. Dacă APR discount > costul capitalului companiei → refuzi early payment. Dacă APR discount < costul capitalului → acceptă discountul (generezi return mai bun decât cash investit în altceva). Evaluează pe fiecare furnizor separat.

### Pas 5: Optimizare timing plăți pentru maximizare float
Nu plăti înainte de scadență contractuală (dacă nu există discount avantajos). Centralizează plățile în 2-3 runs săptămânale pentru eficiență operațională. Sincronizează plățile cu intrările de cash anticipate (din F-024 forecast). Identifică plățile de nivel mare care pot fi eșalonate cu acordul furnizorului. Implementează un sistem de approval pentru plăți urgente (ad-hoc) în afara run-urilor standard.

### Pas 6: Monitorizare lunară DPO și relații furnizori
KPI dashboard lunar: DPO (total și pe segment), % facturi plătite în termen, % facturi cu penalități, Supplier Satisfaction Score (sondaj semestrial). Monitorizează semnalele de deteriorare relație: creșterea prețurilor la reînnoire, reducerea termenelor oferite, cereri de scrisori de garanție. Raportează CFO cu variație față de target.

---

## 3. Cortex Logging

```json
{
  "text": "Accounts Payable Management Optimization executat: DPO calculat și benchmarkat, termeni furnizori revizuiți, oportunități extensie identificate, dynamic discounting evaluat",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-027",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["accounts-payable", "DPO", "working-capital", "supplier-management", "corporate-finance", "intermediate"],
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
- Optimizare DPO fără evaluarea impactului pe relațiile cu furnizorii → violation
- Dynamic discounting evaluat fără comparare cu costul capitalului → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-027 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Accounts Payable Management Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ERP / AP module | Registru furnizori și termeni |
| Contracte furnizori | Termeni de plată contractuali |
| Cash forecast (F-024) | Sincronizare timing plăți |
| Supply Chain Finance platform | Dynamic discounting (dacă disponibil) |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| DPO target | ≥ industrie median |
| % facturi plătite în termen contractual | > 95% |
| Beneficiu cash flow din optimizare DPO | Cuantificat în EUR/USD |
