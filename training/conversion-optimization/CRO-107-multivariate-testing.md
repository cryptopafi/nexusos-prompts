---
id: CRO-107
title: Multivariate Testing Setup and Analysis
domain: conversion-optimization
source: Complete Conversion Optimization Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Multivariate Testing Setup and Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: MVT methodology cu sample size requirements (MDE 5-10%, 95% confidence, 80% power per combinație), tool specifics (VWO, Optimizely, Convert.com), interaction effects analysis, și decision criteria pentru MVT vs. sequential A/B testing.

---

## 1. Problema

A/B testing testează un element pe rând — eficient dar lent. MVT testează combinații de elemente SIMULTAN, dezvaluind interacțiuni (un headline poate performa diferit cu imagini diferite). Dar MVT necesită MULT mai mult trafic — un MVT lansat pe trafic insuficient nu atinge niciodată semnificație și irosește săptămâni. Decizia MVT vs. A/B trebuie luată pe baza de calcul, nu intuiție.

Situații acoperite:
- Pagini high-traffic (>10,000/lună pe pagina testată) cu multipli candidați de optimizare
- Identificarea combinației optime headline + CTA + hero image
- Testare avansată după epuizarea câștigurilor din A/B teste individuale

---

## 2. Procedura

### Pas 1: Evaluarea eligibilității — MVT vs. Sequential A/B
MVT necesită semnificativ mai mult trafic. Formula de decizie:

```
Sample size MVT = Sample size A/B × Număr de combinații
```

**Exemplu concret**:
- A/B test: 2 variante headline → 7,000/variantă = 14,000 total (la 2% baseline CR, 10% MDE)
- MVT: 3 headlines × 2 CTAs = **6 combinații** → 7,000 × 6 = **42,000 total**
- Dacă pagina are 2,000 vizitatori/zi → A/B = 7 zile, MVT = 21 zile
- Dacă pagina are 500 vizitatori/zi → A/B = 28 zile, MVT = **84 zile** → NU fă MVT!

**Regula de decizie**:
| Durata estimată MVT | Decizie |
|---------------------|---------|
| <4 săptămâni | ✅ MVT — go ahead |
| 4-8 săptămâni | 🟡 Evaluează — merită complexitatea? |
| >8 săptămâni | ❌ Sequential A/B tests — divide și testează individual |

**Calculator**: abtestguide.com/calc (setează number of variations = total combinații MVT).

### Pas 2: Selectarea elementelor și variantelor
Principiu cheie: testează elemente care PROBABIL interacționează (nu independente):

**Combinații bune pentru MVT** (interacțiune probabilă):
- Headline + Subheadline (mesajul complet)
- Headline + Hero image (mesaj + vizual)
- CTA text + CTA culoare (acțiune + vizual)
- Form headline + Form CTA button (context + acțiune)

**Combinații SLABE pentru MVT** (probabil independente → testează A/B separat):
- Header color + Footer text (zero legătură)
- Logo size + testimonial position (independente)

**Limită critică**: Max 2-3 elemente × 2-3 variante = **4-9 combinații**. Peste 9 combinații: traficul necesar explodează exponențial. 3 × 3 × 3 = 27 combinații = practic imposibil de atins semnificație pe majoritatea site-urilor.

### Pas 3: Configurarea MVT în testing platform
**Tool-uri cu suport MVT nativ**:
- **VWO** ($199-999/mo): MVT builder vizual, interaction analysis built-in. RECOMANDAT
- **Optimizely** ($50K+/an): MVT enterprise, Bayesian statistics, early stopping intelligent
- **Convert.com** ($99-499/mo): MVT cu privacy focus, stat engine robust

**Setup VWO** (exemplu):
1. Create → Multivariate Test
2. Select page URL
3. Add elements (headline, CTA, image)
4. Add variations per element
5. Platform auto-generates all combinations (ex: 3H × 2CTA = 6)
6. Set primary goal (conversion event)
7. Set traffic allocation (100% sau mai puțin dacă risc)
8. Preview FIECARE combinație pe desktop + mobile
9. Set minimum sample size per combinație

### Pas 4: Rularea și monitorizarea inițială (primele 48h critice)
**Lansare și verificare**:
- Verifică primele 48h: distribuția traficului uniformă între combinații (deviatii >15% = problemă)
- Testează conversie pe fiecare combinație cu tranzacție reală
- Verifică vizual fiecare combinație pe device-uri diferite
- Dacă problemă tehnică în 48h: OPREȘTE, corectează, RELANSEAZĂ cu date fresh (datele contaminate nu sunt recuperabile)

**NU fă "peeking"** (nu verifica rezultate zilnic hoping for significance). Setează alert automat la atingerea sample size-ului.

### Pas 5: Analiza efectelor principale (Main Effects)
MVT produce 2 tipuri de insights — analizează-le în ordine:

**Main Effects** (element individual):
- Care variantă de headline performează mai bine ÎN MEDIE (indiferent de combinație)?
- Care CTA text performează mai bine ÎN MEDIE?
- Main effects ating semnificanță statistică MAI REPEDE (agregă trafic din toate combinațiile)
- Raportează per element: "Headline B outperforms Headline A cu +12% (p=0.03)"

**Interpretare Main Effects**:
- Dacă un element are main effect puternic (>10% uplift, p<0.05) dar altul nu → elementul cu effect puternic contează mai mult pentru conversie
- Main effects care nu sunt semnificative → elementul respectiv nu influențează conversia semnificativ → nu mai testa variante pe el

### Pas 6: Analiza interacțiunilor (Interaction Effects)
**Interaction effect** = când combinația specifică performează DIFERIT decât suma efectelor individuale:
- Headline A + CTA 1: CR 3.2%
- Headline A + CTA 2: CR 3.0%
- Headline B + CTA 1: CR 2.8%
- Headline B + CTA 2: CR 4.1% ← **interacțiune!** Headline B + CTA 2 performează mult mai bine decât s-ar aștepta din main effects

**Cum detectezi**: VWO/Optimizely raportează interaction effects. Alternativ: compară CR per combinație cu CR estimat din main effects. Diferențe >5% = interacțiune semnificativă.

**De ce contează**: Dacă ai fi testat Headline și CTA separat (A/B sequential), ai fi ales Headline A (main effect mai bun) și CTA 2 (main effect similar), ratând combinația Headline B + CTA 2 care e CÂȘTIGĂTOAREA reală.

**Documentează** toate interacțiunile — chiar și cele non-significative. Sunt insights despre cum funcționează messaging-ul.

### Pas 7: Implementare și iterare
**La atingerea semnificatiei**:
1. Identifică combinația câștigătoare (cel mai mare uplift, ≥95% confidence, sample size suficient PER COMBINAȚIE)
2. Implementeaza ca noua versiune control
3. Documentează în Test Registry:
   - Combinația câștigătoare
   - Main effects per element (ranked)
   - Interaction effects descoperite
   - Uplift total
   - Learnings: ce am descoperit despre cum interacționează elementele?

**Post-MVT, continuă cu A/B**:
- Elementele cu main effect slab → exclude din testare viitoare
- Elementele cu main effect puternic → testează variante noi (A/B)
- Interacțiunile descoperite → testează alte combinații pe aceleași elemente

**MVT este un test EXPLORATOR** — descoperă cum interacționează elementele. A/B-ul este un test de CONFIRMARE — validează câștigătorul pe sample size mai mare.

---

## 3. Cortex Logging

```json
{
  "text": "MVT executat: eligibilitate trafic verificată (durata <8 săpt), 2-3 elemente × 2-3 variante testate (4-9 combinații), main effects analizate per element, interaction effects documentate, combinația câștigătoare implementată cu ≥95% confidence.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-107",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["multivariate-testing", "mvt", "cro", "interaction-effects", "main-effects", "vwo", "optimizely"],
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
- MVT lansat fără sample size calculation per combinație → violation
- >9 combinații fără justificare solidă → violation
- Durata estimată >8 săptămâni fără reevaluare → violation
- Test oprit înainte de semnificație PER COMBINAȚIE → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Sample size per combinație calculat (nu doar per element)?
- [ ] Durata estimată <8 săptămâni?
- [ ] Preview fiecărei combinații pe desktop + mobile?
- [ ] Main effects + interaction effects documentate?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-107 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Multivariate Testing Setup and Analysis" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| VWO ($199/mo) / Optimizely | MVT platform cu interaction analysis |
| Convert.com ($99/mo) | MVT privacy-focused alternativă |
| ABTestGuide Calculator | Sample size per combinație |
| Evan Miller Calculator | Significance verification |
| Google Analytics 4 | Tracking conversii per combinație |
| Test Registry (Sheets/Notion) | Documentare rezultate |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Combinații testate per MVT | 4-9 (max) |
| Durata test | <8 săptămâni |
| Confidence level câștigător | ≥95% |
| Main effects documentate | per fiecare element |
| Interaction effects documentate | toate combinațiile analizate |
| Uplift combinație câștigătoare vs. control | ≥10% |
| Sample size per combinație | ≥calculated minimum |
