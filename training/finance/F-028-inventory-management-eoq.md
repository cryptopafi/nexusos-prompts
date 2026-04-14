---
type: procedure
created: 2026-03-22
status: active
slug: f-028-inventory-management-eoq
tags: [procedure, nexus]
---

# Inventory Management Optimization (EOQ Model) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Optimizarea nivelurilor de inventar prin modelul EOQ (Economic Order Quantity) pentru minimizarea costurilor totale de deținere și comandare.

---

## 1. Problema

Suprastocarea imobilizează capitalul și crește costurile de depozitare, iar substocarea duce la stockout-uri și pierderi de vânzări. Această procedură acoperă situațiile în care un manager financiar sau operations manager trebuie să determine cantitatea optimă de comandă și punctul de recomandare pentru fiecare SKU sau categorie de produs.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Colectare date de cerere și costuri
Adună pentru fiecare SKU / categorie: (D) cererea anuală în unități (din istoricul vânzărilor sau forecasting), (S) costul per comandă (processing, transport, primire — toate costurile fixe per order), (H) costul de deținere per unitate per an (capital cost: WACC × unit cost; depozitare; asigurare; obsolescență estimată). Verifică că datele acoperă minimum 12 luni pentru a capta sezonalitatea.

### Pas 2: Calculare EOQ
Formula: `EOQ = √(2 × D × S / H)`. Calculează EOQ pentru fiecare SKU sau categorie principală. Verifică sensibilitatea: calculează costul total (TC = D/Q × S + Q/2 × H) la ±20% față de EOQ pentru a vedea platitudinea curbei de cost. Interpretare: dacă curba este plată în jurul EOQ, există flexibilitate; dacă este ascuțită, aderența la EOQ este critică.

### Pas 3: Calculare reorder point și safety stock
`Lead Time Demand = (D / 365) × Lead Time (zile)`. `Safety Stock = Z × σ_demand × √Lead Time`, unde Z = 1.65 pentru service level 95% sau Z = 2.05 pentru 98%. Formula: `Reorder Point (ROP) = Lead Time Demand + Safety Stock`. Documentează lead time-ul per furnizor și variabilitatea acestuia (σ_LT). Calculează și dacă safety stock trebuie ajustat pentru seasonal demand.

### Pas 4: Construire politică de inventar
Documentează pentru fiecare categorie: EOQ (cantitate de comandă), ROP (punctul de declanșare comandă), Safety Stock (buffer), și Max Stock (EOQ + Safety Stock). Creează un document de politică de inventar cu tabelul complet. Definește excepțiile: SKU-uri cu cerere foarte variabilă (folosesc alt model — e.g., min-max), produse cu shelf life, produse strategice critice.

### Pas 5: Implementare în ERP / sistem de management
Introduce parametrii EOQ, ROP, și Safety Stock în sistemul ERP (SAP, Oracle, NetSuite etc.) pentru automatizarea generării de comenzi de achiziție. Testează logica de trigger cu o simulare pe date istorice: ar fi generat comenzile la momentul corect? Ajustează parametrii dacă testul arată stockout-uri sau suprastocare frecventă.

### Pas 6: Monitorizare și revizuire lunară/trimestrială
KPI dashboard: Inventory Turnover Ratio (COGS/Avg Inventory), Days Inventory Outstanding (DIO), Stockout Rate (%), Fill Rate (%). Compară costul total actual cu costul teoretic EOQ. Revizuiește parametrii trimestrial sau când se schimbă semnificativ cererea (>20%), costul comenzilor, sau lead time-urile. Documentează deviațiile și acțiunile corective.

---

## 3. Cortex Logging

```json
{
  "text": "Inventory Management Optimization (EOQ Model) executat: EOQ calculat per SKU, reorder point și safety stock definite, politică inventar documentată, implementare ERP planificată",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-028",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["inventory-management", "EOQ", "working-capital", "operations", "corporate-finance", "intermediate"],
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
- EOQ calculat fără safety stock și ROP → violation
- Politică de inventar documentată fără excepții definite → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-028 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Inventory Management Optimization (EOQ Model)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Istoricul vânzărilor (12+ luni) | Date cerere pentru D |
| AP / Purchasing data | Costul per comandă (S) |
| Warehouse cost data | Costul de deținere (H) |
| ERP system (SAP/Oracle/NetSuite) | Implementare parametri EOQ |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Inventory Turnover Ratio | ≥ industrie median |
| Stockout Rate | < 2% |
| Reducere cost total inventar vs. baseline | Documentată și cuantificată |
