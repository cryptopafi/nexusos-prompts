---
type: procedure
created: 2026-03-22
status: active
slug: m-023-google-ads-bid-strategy
tags: [procedure, nexus]
---

# Google Ads Bid Strategy Selection and Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Selectarea bid strategy corecte în funcție de faza campaniei și obiectiv, și optimizarea ei continuă pentru eficiență maximă.

---

## 1. Problema

Alegerea greșită a bid strategy este una dintre cele mai frecvente cauze de performanță slabă în Google Ads: campanii noi puse direct pe Target CPA fără date de conversie se blochează în "learning period" infinit, iar campanii mature pe Manual CPC lasă bani pe masă. Tranziția prematură sau tardivă între strategii costă semnificativ. Această procedură standardizează selecția și evoluția bid strategy pe parcursul ciclului de viață al campaniei.

Situații acoperite:
- Selecția bid strategy pentru o campanie nouă fără date istorice
- Tranziția de la Manual CPC la Smart Bidding după acumularea datelor
- Optimizarea unui Target CPA sau Target ROAS care nu performează la target

---

## 2. Procedura

### Pas 1: Evaluează faza campaniei și datele disponibile
Determină în ce fază se află campania: Faza 1 (0-30 conversii/lună) = date insuficiente pentru Smart Bidding, Faza 2 (30-100 conversii/lună) = eligibil pentru Smart Bidding cu target conservativ, Faza 3 (100+ conversii/lună) = Smart Bidding complet operațional. Această evaluare dictează toată decizia de bid strategy.

### Pas 2: Selectează bid strategy conform fazei
Faza 1: folosește Maximize Clicks cu Max CPC bid limit (setează limita la 150% din CPC mediu estimat din Keyword Planner). Faza 2: trece la Target CPA cu target setat la 150% din CPA-ul real actual (nu la CPA target dorit). Faza 3: optimizează Target CPA sau trece la Target ROAS dacă ai valori de conversie. Niciodată nu sări direct de la Maximize Clicks la Target ROAS.

### Pas 3: Configurează bid strategy și setează portfolio (dacă aplicabil)
Aplică bid strategy la nivel de campanie, nu ad group. Dacă rulezi mai mult de 3 campanii cu același obiectiv, creează un Portfolio Bid Strategy (Tools → Bid Strategies) pentru a agrega datele de conversie și a accelera ieșirea din learning period. Setează campania în modul de bid strategy ales și documentează data setării.

### Pas 4: Monitorizează learning period (primele 7-14 zile)
Nu modifica bid strategy în primele 7 zile după setare — orice schimbare repornește learning period. Monitorizează zilnic: impresii, clicks, conversii, CPA/ROAS actual vs. target. Dacă după 14 zile campania nu iese din "Limited - Learning", verifică volumul de conversii și considera relaxarea target-ului.

### Pas 5: Ajustează target-ul incremental (10-20% per săptămână)
Nu ajusta target CPA/ROAS cu mai mult de 20% o dată — schimbările bruște repornesc learning period. Dacă CPA actual este sub target cu 20%+, reduce target-ul CPA cu 10-15% săptămânal pentru a eficientiza cheltuiala. Dacă ROAS actual depășește target-ul consistent, crește target-ul ROAS cu 10-15% săptămânal.

### Pas 6: Adaugă bid adjustments pentru semnale cheie
Configurează bid adjustments pentru: Devices (dacă mobile converge la CPL diferit), Locations (dacă anumite zone convertesc mai bine), Ad Scheduling (dacă ai ore de vârf identificate), Audiences (RLSA - ajustare pozitivă pentru vizitatori site sau cumpărători anteriori). Limitează bid adjustments la maximum ±30% per categorie pentru a nu perturba algoritmul.

### Pas 7: Documentează evoluția și setează review schedule
Salvează în Cortex: bid strategy curentă, target setat, data setării, performanța la 7/14/30 zile. Setează review la 30 de zile pentru evaluarea tranziției la faza următoare. Documentează orice schimbare de bid strategy cu motivul și rezultatul așteptat.

---

## 3. Cortex Logging

```json
{
  "text": "M-023 executat: Bid strategy configurată pentru campania [Nume]. Faza: [1/2/3]. Strategie selectată: [tip]. Target: [CPA/ROAS valoare]. Portfolio bid strategy: DA/NU. Learning period status: [activ/ieșit]. CPA actual vs target: [%].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-023",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "bid-strategy", "smart-bidding", "target-cpa", "target-roas", "manual-cpc", "learning-period"],
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
- Target CPA/ROAS modificat cu mai mult de 20% într-o singură ajustare → violation
- Smart Bidding activat pe campanie cu sub 30 de conversii/lună → violation
- Bid strategy modificată în primele 7 zile de la setare fără motiv documentat → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Faza campaniei evaluată corect înainte de selecție?
- [ ] Target setat la 150% din CPA actual la tranziția la Smart Bidding?

**VK-uri obligatorii:**
1. `✅ [PROC] M-023 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Ads Bid Strategy Selection and Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Ads Account | Platformă principală |
| Conversion Tracking (M-022) | Prerequisit pentru Smart Bidding |
| Google Ads Reports | Monitorizare CPA/ROAS actual |
| Portfolio Bid Strategies | Agregare date pentru campanii multiple |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CPA actual vs. target la 30 zile | Maximum +10% față de target |
| Timp în learning period | Maximum 14 zile per ciclu |
| Ajustare incrementală target | Maximum 20% per săptămână |
