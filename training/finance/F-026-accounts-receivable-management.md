---
type: procedure
created: 2026-03-22
status: active
slug: f-026-accounts-receivable-management
tags: [procedure, nexus]
---

# Accounts Receivable Management Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Optimizarea procesului de gestionare a creanțelor pentru reducerea DSO și îmbunătățirea cash conversion cycle.

---

## 1. Problema

DSO (Days Sales Outstanding) ridicat imobilizează capitalul de lucru și crește riscul de bad debt. Această procedură acoperă situațiile în care un CFO sau treasury manager trebuie să diagnosticheze, benchmarke, și optimizeze procesul AR, de la credit scoring la colectare și early payment programs.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Calculare și benchmark DSO
Formula: `DSO = (Accounts Receivable / Revenue) × 365`. Calculează DSO pentru ultimele 4 trimestre pentru a identifica trendul. Benchmarkează față de industrie (surse: Dun & Bradstreet, industrie rapoarte). Calculează și Best Possible DSO: `BPDSO = (Current AR / Revenue) × 365` — arată câte zile ar dura dacă nimeni nu ar fi overdue.

### Pas 2: Analiza aging buckets AR
Construiește raportul de aging: sumele în buckets 0-30 zile (current), 31-60 zile (overdue 1), 61-90 zile (overdue 2), 91-120 zile (overdue 3), >120 zile (bad debt risk). Calculează % din total AR în fiecare bucket. Identifică top 10 clienți după suma overdue și vechimea restanței. Clasifică riscul: Verde (0-30), Galben (31-60), Portocaliu (61-90), Roșu (>90).

### Pas 3: Implementare credit scoring pentru clienți noi
Definește criteriile de credit scoring: situații financiare (dacă disponibile), istoricul de plăți cu compania, referințe bancare, D&B / credit bureau report, industria clientului și ciclul de cash al acesteia. Setează limite de credit pe baza scorului: Scor A → 30 zile net, Scor B → 14 zile net + garanție, Scor C → plată în avans sau LC.

### Pas 4: Design proces de colectare escalat
Definește escalation ladder: Ziua 1 → factură emisă + confirmare recepție. Ziua T+5 (înainte de scadență) → reminder automat email. Ziua T+15 (overdue) → contact telefonic account manager. Ziua T+30 → scrisoare formală / suspendare credit. Ziua T+60 → escalare la management superior + legal notice. Ziua T+90 → colectare externă / acțiune în judecată.

### Pas 5: Evaluare program early payment discount
Calculează costul programului: dacă oferi 2% discount pentru plată în 10 zile (față de net 30), costul APR echivalent este ~36.7%. Evaluează: este mai ieftin decât costul liniei de credit? Câți clienți ar adopta discountul? Impactul net pe DSO și cash flow. Implementează selectiv pentru clienții mari cu DSO ridicat.

### Pas 6: Implementare și monitorizare lunară
Setează KPI dashboard lunar: DSO (total și pe segment), % AR >60 zile, Bad Debt Rate (bad debts / revenue), Collection Effectiveness Index (CEI). Revizuiește lunar și ajustează limite de credit. Raportează CFO-ului cu varianță față de target și acțiunile corective planificate.

---

## 3. Cortex Logging

```json
{
  "text": "Accounts Receivable Management Optimization executat: DSO calculat și benchmarkat, aging analizat, credit scoring implementat, process de colectare definit",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-026",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["accounts-receivable", "DSO", "working-capital", "credit-management", "corporate-finance", "intermediate"],
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
- DSO calculat fără benchmark față de industrie → violation
- Plan de colectare fără escalation ladder definit → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-026 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Accounts Receivable Management Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ERP / Accounting system | Raport aging AR |
| D&B / Credit Bureau | Credit scoring clienți noi |
| Industry benchmarks (D&B, sectorial) | Comparare DSO |
| CRM / Collections software | Automatizare escalation |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| DSO target | ≤ industrie median |
| % AR >60 zile | < 10% din total AR |
| Collection Effectiveness Index (CEI) | > 85% |
