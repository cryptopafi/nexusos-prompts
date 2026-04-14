---
type: procedure
created: 2026-03-22
status: active
slug: m-026-google-pmax-campaign
tags: [procedure, nexus]
---

# Google Performance Max (PMax) Campaign Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea corectă a campaniei Performance Max pentru a valorifica algoritmul Google pe toate rețelele cu assets de calitate maximă și audience signals relevante.

---

## 1. Problema

Performance Max este cel mai puternic format de campanie Google, dar și cel mai ușor de configurat greșit: assets slabe, audience signals lipsă sau obiective neclare duc la cheltuieli mari cu rezultate slabe și o "cutie neagră" imposibil de optimizat. PMax fără setup corect canibalizează campaniile Search existente. Această procedură asigură un setup PMax care maximizează performanța și minimizează conflictul cu alte campanii.

Situații acoperite:
- Prima campanie PMax pentru un cont cu campanii Search active
- PMax pentru e-commerce ca înlocuitor al Smart Shopping
- PMax pentru generare de leads cu feed de produse absent

---

## 2. Procedura

### Pas 1: Evaluează prerequisitele și decide dacă PMax este potrivit
PMax necesită: conversion tracking activ și minimum 30-50 conversii/lună (altfel algoritmul nu are date suficiente), assets de calitate (imagini, video, texte), obiectiv clar. Dacă nu ai conversii suficiente, amână PMax și folosește campanii Search dedicate. Dacă ai campanii Search active pe brand keywords, excludele explicit din PMax înainte de lansare.

### Pas 2: Configurează Asset Groups (minimum 2 Asset Groups per campanie)
Creează Asset Groups separate pentru categorii de produse sau servicii distincte. Fiecare Asset Group necesită: minimum 5 headlines (30 char), 5 long headlines (90 char), 4 descriptions (90 char), 3-5 imagini landscape (1.91:1), 1 imagine square (1:1), 1 logo. Dacă ai video propriu adaugă-l (altfel Google generează automat video de calitate slabă). Ad Strength trebuie să fie "Excellent" pentru fiecare Asset Group.

### Pas 3: Adaugă Audience Signals (cel mai important element)
Audience Signals nu limitează targetarea, ci accelerează learning period. Adaugă: Customer Match list (lista de email-uri clienți existenți), Website visitors (remarketing audience din Google Ads), Custom intent audiences (persoane care caută keywords specifice), Similar audiences la Customer Match. Fără Audience Signals, algoritmul învață mult mai lent și cheltuie mai mult în faza de explorare.

### Pas 4: Configurează URL Expansion și excluderi de pagini
Activează URL Expansion cu atenție: bifează "Send traffic to most relevant URLs" și adaugă excluderi explicite pentru pagini irelevante (blog, pagini de carieră, pagini de login, pagini cu erori 404). Dacă site-ul are pagini în mai multe limbi, excludele pe cele irrelevante pentru piața targetată. Setează Final URL Expansion la "Off" dacă vrei control total asupra URL-urilor.

### Pas 5: Setează bid strategy și buget recomandat
PMax funcționează cel mai bine cu Maximize Conversions (cu Target CPA opțional) sau Maximize Conversion Value (cu Target ROAS opțional). Bugetul zilnic recomandat: minimum 10x CPA target per zi (ex: dacă CPA target = 50 RON, bugetul zilnic minim = 500 RON). Un buget prea mic blochează algoritmul în learning period perpetuu.

### Pas 6: Configurează Brand Safety și excluderi de conținut
În Settings → Brand Safety, setează nivelul de excludere conținut sensibil la "Standard" sau "Full" pentru branduri care nu vor să apară lângă conținut controversat. Adaugă excluderi de plasament pentru site-uri sau aplicații irelevante (deși controlul în PMax este limitat). Dacă rulezi PMax alături de campanii Search, setează prioritizarea campaniei Search pentru brand keywords.

### Pas 7: Stabilește monitoring plan și interpretează rapoartele PMax
PMax nu oferă raportare la nivel de keyword sau plasament standard. Monitorizează: Asset Group performance (asseturile cu "Low" rating trebuie înlocuite în 14 zile), Search terms categories report, Auction insights. Setează Data Exclusions dacă ai perioade cu date atipice (ex: Black Friday). Documentează în Cortex: Asset Groups create, Audience Signals adăugate, ROAS/CPA la 7/30 zile.

---

## 3. Cortex Logging

```json
{
  "text": "M-026 executat: Campanie PMax configurată. Asset Groups: [N]. Ad Strength: [Excellent/Good/N]. Audience Signals adăugate: [tipuri]. URL Expansion: ON/OFF. Bid strategy: [tip]. Target CPA/ROAS: [valoare]. Buget zilnic: [RON/EUR]. Status learning: [activ/ieșit].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-026",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "performance-max", "pmax", "asset-groups", "audience-signals", "smart-bidding", "all-networks"],
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
- PMax lansat fără Audience Signals configurate → violation
- Asset Group cu Ad Strength sub "Good" la lansare → violation
- PMax lansat pe cont cu sub 30 conversii/lună fără documentare motivului → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 2 Asset Groups create cu Ad Strength "Good" sau "Excellent"?
- [ ] Brand keywords excluse din PMax pentru a nu canibaliza campaniile Search?

**VK-uri obligatorii:**
1. `✅ [PROC] M-026 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Performance Max (PMax) Campaign Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Conversion Tracking (M-022) | Prerequisit obligatoriu pentru PMax |
| Google Merchant Center | Feed produse pentru PMax Shopping |
| Customer Match Lists | Audience Signals de calitate |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| ROAS PMax la 30 zile | Egal sau mai bun față de campaniile Standard Shopping |
| Ad Strength Asset Groups | 100% "Good" sau "Excellent" |
| Ieșire din learning period | Maximum 14 zile de la lansare |
