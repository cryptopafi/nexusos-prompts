---
id: CRO-101
title: A/B Testing Methodology SOP
domain: conversion-optimization
source: Complete Conversion Optimization Course / CRO Landing Page Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# A/B Testing Methodology SOP — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Metodologia completă pentru A/B testing cu statistical significance (95% confidence, 80% power, MDE 5-10%), tool specifics (VWO, Optimizely, Convert.com, GA4 experiments), și conversion benchmarks per industrie. Include sample size calculators și anti-contamination protocol.

---

## 1. Problema

Programele de A/B testing fără metodologie produc teste izolate fără learninguri cumulative, testează elemente cu impact mic, sau sunt oprite prematur. Benchmark: doar 1 din 3 teste produce un câștigător statisticemnt semnificativ — dar fiecare test (inclusiv pierdut) produce un learning valoros dacă este documentat corect.

Situații acoperite:
- Lansarea unui program de CRO sistematic
- Scalarea de la teste ad-hoc la calendar structurat
- Integrarea A/B testing în product development cycle

---

## 2. Procedura

### Pas 1: Eligibilitate trafic — Sample size calculation
Calculează dacă site-ul are trafic suficient ÎNAINTE de a configura orice test:

**Formula sample size**: Pentru a detecta un MDE (Minimum Detectable Effect) cu parametrii standard:
- **Confidence level**: 95% (α = 0.05) — standard industrie
- **Statistical power**: 80% (β = 0.20) — probabilitatea de a detecta un efect real
- **MDE**: 5-10% relative improvement — sub 5% necesită sample size enorm

**Sample size per variantă** (estimări orientative):

| Baseline CR | MDE 5% | MDE 10% | MDE 20% |
|-------------|--------|---------|---------|
| 1% | ~310,000 | ~78,000 | ~19,500 |
| 2% | ~155,000 | ~39,000 | ~9,800 |
| 5% | ~62,000 | ~15,500 | ~3,900 |
| 10% | ~31,000 | ~7,800 | ~1,950 |

**Calculatoare online** (obligatoriu, nu estimări de cap):
- **Evan Miller's Calculator**: evanmiller.org/ab-testing/sample-size.html — cel mai folosit
- **VWO Calculator**: vwo.com/tools/ab-test-sample-size-calculator
- **ABTestGuide**: abtestguide.com/calc — include duration estimation

**Regula de decizie**: Dacă durata estimată a testului > 8 săptămâni → NU rula A/B test clasic. Alternativele: user testing (5 participanți), heatmap analysis, survey, sau accept best-practice change fără test.

**Conversion benchmarks per industrie** (pentru calibrare MDE):
| Industrie | CR mediu | Range |
|-----------|---------|-------|
| SaaS (trial/demo) | 3-5% | 1-10% |
| E-commerce | 2-3% | 1-5% |
| Lead Generation | 5-15% | 2-25% |
| B2B Services | 2-5% | 1-8% |

### Pas 2: Test Registry — documentarea ipotezelor înainte de lansare
Creează un Google Sheet / Notion "Test Registry" cu coloane:

| Coloană | Scop |
|---------|------|
| ID test | Referință unică (CRO-T-001, etc.) |
| Pagina testată | URL specific |
| Element testat | Ce se schimbă (headline, CTA, layout, etc.) |
| Ipoteză | Format obligatoriu (vezi mai jos) |
| Status | Planned / Running / Completed / Invalid |
| Variante | Descriere control + challenger |
| Sample size necesar | Calculat cu tool |
| Data start / end | Cronologie |
| Rezultat | Winner / Loser / Inconclusive |
| Uplift + CI | Uplift % cu confidence interval |
| Learnings | Ce am învățat (cel mai important câmp!) |

**Format ipoteză obligatoriu**: "Dacă schimbăm [element specific] din [stare curentă] în [stare nouă], atunci [metrica] va crește cu [X]% deoarece [rationale din research/data]."

Exemplu: "Dacă schimbăm CTA button text din 'Submit' în 'Get My Free Audit', atunci form completion rate va crește cu 15% deoarece user testing a arătat că 4/5 participanți nu știau ce primesc la click pe 'Submit'."

### Pas 3: Ierarhia elementelor de testat (impact descrescător)
Testează în ordinea impactului potențial — nu pierde trafic pe teste cu impact mic:

| Prioritate | Element | Impact potențial | Exemplu |
|-----------|---------|-----------------|---------|
| 1 | Oferta / propunerea de valoare | MAJOR (50-300% uplift posibil) | Free trial 14 days vs. 30 days |
| 2 | Headline H1 | MARE (20-100%) | Benefit-focused vs. feature-focused |
| 3 | Layout și structura paginii | MARE (15-50%) | Long-form vs. short-form |
| 4 | CTA — text, poziție | MEDIU (10-30%) | "Start Free" vs. "Get Started" |
| 5 | Hero image / video | MEDIU (5-20%) | Product screenshot vs. person using product |
| 6 | Social proof | MEDIU (5-15%) | Testimonial text vs. video testimonial |
| 7 | Form (lungime, câmpuri) | MEDIU (10-25% per câmp eliminat) | 5 fields vs. 3 fields |
| 8 | Micro-copy | MIC (2-10%) | Button label, helper text |

**NU** testa culoarea unui buton înainte de a testa propunerea de valoare.

### Pas 4: Tool setup și anti-contamination protocol
**Tool-uri recomandate** (2026):
- **VWO** ($199-999/mo): Cel mai complet, include heatmaps + surveys. Best for teams
- **Optimizely** ($50K+/an): Enterprise, cel mai robust statistic engine. Feature flags incluse
- **Convert.com** ($99-499/mo): Privacy-focused (GDPR-friendly), flicker-free. Best value
- **GA4 Experiments** (gratuit): Basic A/B testing prin GA4 + Google Optimize successor. Limitat dar free

**Anti-contamination checklist** (ÎNAINTE de lansare):
- [ ] Distribuție trafic uniformă (50/50) verificată în preview
- [ ] Cookie duration = durata testului (utilizatorii reveniți văd aceeași variantă)
- [ ] IP-uri interne filtrate (angajați, agenție)
- [ ] Testul NU coincide cu: Black Friday, lansare produs, campanie PPC majoră, seasonality extremă
- [ ] Tracking conversii funcțional pe AMBELE variante (testează cu tranzacție reală)
- [ ] Un singur element modificat per variantă (nu multiple changes simultane — invalidează rezultatele)
- [ ] QA pe toate device-urile (desktop, mobile, tablet) pe ambele variante

### Pas 5: Durata minimă și regula ciclurilor complete
Reguli de durată (NU negociabile):
- **Minim 7 zile calendaristice** (1 ciclu săptămânal complet) — weekendurile au comportament DIFERIT
- **Minim 2 cicluri business complete** (14 zile recomandat)
- **Sample size atins** pe ambele variante (nu doar pe una)
- **NU opri testul** la prima semnificație statistică — "peeking problem": dacă verifici zilnic, vei vedea false positives. Definește sample size înainte și rulează până la atingere
- **Exception**: Oprire de urgență dacă o variantă produce conversie ZERO sau erori tehnice

### Pas 6: Analiza post-test și documentare learnings
La încheierea testului (sample size atins + minimum 7 zile):
1. **Export date brute** din tool (nu te baza doar pe dashboard-ul tool-ului)
2. **Calculează manual** (verificare): Evan Miller's significance calculator
3. **Documentează în Test Registry**:
   - Câștigătorul (sau "inconclusive")
   - Uplift real + 95% confidence interval
   - Sample size per variantă
   - Durata testului
   - **Learning principal**: Ce am învățat despre utilizatori? (cel mai important)
   - **Ipoteze noi sugerate**: Ce ar trebui testat next?
4. Un test "pierdut" cu un learning documentat = la fel de valoros ca un câștigător
5. **Segmentare post-hoc**: Analizează rezultatele pe segmente (mobile vs. desktop, new vs. returning, sursa trafic) — uneori un test "inconclusive" global este câștigător pe un segment specific

### Pas 7: Sprint planning și roadmap de testare pe 3 luni
Organizează testele în sprint-uri de 2-4 săptămâni:
- **Sprint format**: 1 test principal (pagină high-traffic) + 1 test secundar (pagină lower-traffic, dacă traficul permite)
- **Roadmap 3 luni**: Planifică 6-8 teste. Prioritizează cu PIE/ICE (CRO-103)
- **Quarterly review**: La fiecare 3 luni, review cu stakeholders: teste rulate, win rate, total uplift, ROI program CRO, learnings cumulative, plan next quarter
- **Win rate target**: ≥30% din teste câștigătoare (industrie benchmark: 25-35%)
- **Learning rate target**: 100% teste cu learning documentat

---

## 3. Cortex Logging

```json
{
  "text": "Program A/B testing sistematic: eligibilitate trafic verificată cu Evan Miller calculator, Test Registry creat cu ipoteze pre-documentate, ierarhie impact definită, tool configurat (VWO/Convert/Optimizely), anti-contamination checklist executat, sprint schedule activ.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-101",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["ab-testing", "cro", "testing-methodology", "statistical-significance", "test-registry", "sample-size", "mde"],
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
- Test lansat fără ipoteză documentată prealabil → violation
- Test oprit înainte de 7 zile + sample size complet → violation
- Mai mult de un element modificat în aceeași variantă → violation
- Sample size necalculat cu tool → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Sample size calculat cu tool (nu estimat)?
- [ ] Test Registry actualizat cu ipoteză înainte de lansare?
- [ ] Anti-contamination checklist completat?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-101 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "A/B Testing Methodology SOP" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| VWO ($199/mo) / Convert.com ($99/mo) | A/B testing platform |
| Evan Miller Calculator | Sample size și significance |
| ABTestGuide Calculator | Duration estimation |
| Google Analytics 4 | Tracking conversii |
| Google Sheets / Notion | Test Registry |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Teste rulate per trimestru | ≥6 |
| % teste cu ipoteză documentată prealabil | 100% |
| % teste câștigătoare | ≥30% (benchmark: 25-35%) |
| Uplift mediu per test câștigător | ≥8% |
| % teste cu learning documentat | 100% |
| Test Registry actualizat | 100% |
| Anti-contamination checklist per test | 100% |
