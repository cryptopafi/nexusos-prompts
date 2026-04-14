---
id: CRO-102
title: CRO Research Framework (ResearchXL)
domain: conversion-optimization
source: Complete Conversion Optimization Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# CRO Research Framework (ResearchXL) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Framework ResearchXL (ConversionXL) cu 6 tipuri de research, specific outputs per tip, tool recommendations cu prețuri, și sinteză în Problem Inventory prioritizat. Include heatmap tools (Hotjar $39/mo, Clarity free, FullStory, Crazy Egg) și user testing methodology.

---

## 1. Problema

CRO fără research = optimizare pe bază de instinct. Fără a înțelege CE împiedică utilizatorii să convertească și DE CE, testele sunt ghiceli costisitoare. ResearchXL structurează research-ul în 6 tipuri de date complementare care identifică barierele reale. Fiecare tip răspunde la o întrebare diferită.

Situații acoperite:
- Inițierea unui program CRO pentru un site/funnel nou
- Diagnosticarea scăderii bruște a conversiei
- Prioritizarea unui backlog de teste fără direcție clară

---

## 2. Procedura

### Pas 1: Technical Analysis (fundația — 1-2 zile)
Verifică probleme tehnice ÎNAINTE de orice optimizare de persuasion/UX:

**Audit tehnic checklist**:
| Check | Tool | Target |
|-------|------|--------|
| Core Web Vitals — LCP | PageSpeed Insights | <2.5s |
| Core Web Vitals — FID/INP | PageSpeed Insights | <100ms |
| Core Web Vitals — CLS | PageSpeed Insights | <0.1 |
| JavaScript errors | Browser Console (F12) | 0 erori pe paginile din funnel |
| Broken links | Screaming Frog / Ahrefs | 0 pe paginile critice |
| Cross-browser rendering | BrowserStack / LambdaTest | Consistent pe Chrome, Safari, Firefox, Edge |
| Mobile rendering | Device real + Chrome DevTools | Funcțional pe iOS + Android |
| Forms submission | Test manual | Toate formularele trimit corect |
| HTTPS | Browser check | Toate paginile secure |
| 404 pages | GSC Coverage report | 0 pe paginile importante |

**Output**: Technical Issues Document — listă prioritizată de bug-uri tehnice. Bug-urile tehnice au impact NEGATIV mai mare decât orice element de persuasion suboptimal. Fixează-le PRIMUL.

### Pas 2: Heuristic Analysis — Expert Review (2-3 ore)
Evaluează paginile ca expert CRO folosind 5 criterii (ConversionXL framework):

| Criteriu | Întrebare | Ce cauți |
|---------|-----------|---------|
| **Relevance** | Pagina răspunde la intenția căutării/ad-ului? | Message match, keyword alignment |
| **Clarity** | Propunerea de valoare e clară în 5 secunde? | Headline specific, not vague |
| **Friction** | Ce elemente creează confuzie sau rezistență? | Form fields, steps, complexity |
| **Distraction** | Ce distrage de la acțiunea principală? | Multiple CTAs, nav links, popups |
| **Anxiety** | Ce creează teamă sau îndoială? | Missing trust signals, no guarantee |

**Metodă**: Parcurge fiecare pagină din funnel. Pe screenshot-uri (sau wireframes), notează observațiile colorate per criteriu. Scopul: identificare rapidă a problemelor evidente ÎNAINTE de analiza datelor.

**Output**: Heuristic Scorecard — 1 document cu observații per pagină, scorat pe cele 5 criterii (1-5 per criteriu).

### Pas 3: Analytics Analysis — date cantitative (1-2 zile)
Analizează în GA4 (sau alt analytics tool):

| Analiză | Ce cauți | Cum |
|---------|---------|-----|
| Funnel drop-off | Unde pleacă oamenii? | GA4 → Explore → Funnel Exploration |
| Bounce rate per pagină + sursă | Ce pagini pierd trafic? | GA4 → Pages and screens + Source filter |
| Mobile vs. Desktop CR | Gap de conversie? | GA4 → Segment comparison |
| Time on page + scroll depth | Citesc conținutul? | GA4 events + Hotjar scroll map |
| New vs. Returning | Cine convertește mai bine? | GA4 → Segment |
| Landing page performance | Ce pagini au cel mai mare potențial? | High traffic + low conversion = goldmine |

**Output**: Analytics Findings Document — top 5 pagini cu cel mai mare potențial de improvement (trafic ÎNALT + conversie SCĂZUTĂ), segmentele cu comportament diferit, funnel bottleneck identificat.

### Pas 4: Mouse Tracking Analysis — Heatmaps + Recordings (3-5 zile colectare + 1 zi analiză)
**Tool-uri** (alege unul):
| Tool | Preț | Strengths |
|------|------|-----------|
| **Hotjar** | $39/mo (Plus) | All-in-one: heatmaps, recordings, surveys, form analysis. Industry standard |
| **Microsoft Clarity** | FREE | Unlimited sessions, heatmaps, recordings. No surveys/form analysis |
| **FullStory** | $199/mo+ | Enterprise, session replay avansat, DX integration |
| **Crazy Egg** | $49/mo | Heatmaps excelente, confetti view, scroll map |

**Minim 500 sesiuni sau 7 zile** de date înainte de analiză — mai puțin = concluzii false.

**Analiză** (detalii în CRO-106):
- Click heatmap: Ce clickuiesc pe elemente non-interactive? (confuzie)
- Scroll map: Unde e "cliff"-ul (pierdere bruscă >30% vizitatori)? CTA-ul e deasupra?
- Session recordings: 20+ sesiuni abandonate — pattern-uri de confuzie, hesitation, rage clicks
- Form analysis (Hotjar): Care câmp are cel mai mare drop-off?

**Output**: Behavioral Patterns Document — 5-10 observații cu screenshots, pattern-uri de confuzie/friction.

### Pas 5: Qualitative Research — vocea utilizatorului (1-2 săptămâni)
**3 metode** (minim 2 din 3 obligatorii):

**A) On-site survey** (Hotjar $39/mo):
- Plasează pe pagina cu cel mai mare drop-off
- Întrebări: "Ce te împiedică să finalizezi [acțiunea]?", "Ce informații cauți și nu găsești?", "Ce te-ar convinge să [acțiunea]?"
- Target: 50+ răspunsuri (statistici stabilizate la 50-100)

**B) Customer interviews** (5-8 clienți existenți):
- Întrebări: "Ce te-a convins să cumperi?", "Ce te-a făcut să eziti?", "Ce alternativă luai în calcul?", "Care a fost momentul decisiv?"
- 30 minute/interviu. Înregistrează cu permisiune. Noteaz cuvintele exacte (Voice of Customer)

**C) Post-purchase survey** (email automat):
- Trigger: 24-48h post-conversie
- Întrebare cheie: "Ce te-a convins cel mai mult?" + "Ce aproape te-a făcut să NU cumperi?"

**Output**: Voice of Customer Document — top 5 motive de cumpărare, top 5 bariere, cuvintele exacte (folosibile în copy testing).

### Pas 6: User Testing structurat (1-2 săptămâni)
Recruteaza 5 participanți (regula Nielsen: 5 sesiuni dezvaluie 80% din probleme):
- **Recrutare**: UsabilityHub ($79/mo), UserTesting ($49-299/test), Maze ($99/mo), sau recrutare manuală din target audience
- **Sarcina**: "Imaginează-ți că vrei [to buy/sign up/etc.]. Folosește acest site pentru a face asta. Gândește cu voce tare."
- **Observă** (nu interveni): Unde se blochează? Ce întrebări pun? Unde sunt confuzi? Ce lipsește? Unde dau scroll înapoi (au ratat ceva)?
- **Noteaza**: Problem, severity, page, timestamp
- **Nu**: Nu ajuta participantul. Nu explica ce trebuie făcut. Nu justifica decizii de design

**Output**: Usability Issues Document — listă de probleme ranked by severity × frequency.

### Pas 7: Sinteză în Problem Inventory și prioritizare
După completarea celor 6 tipuri de research, sintetizează:

**Problem Inventory** (Google Sheet/Notion):
| # | Problemă identificată | Surse de research | Categorie | Severity (1-5) | Pagina | Ipoteză de test suggerată |
|---|---------------------|-------------------|-----------|----------------|--------|--------------------------|
| 1 | CTA nu e vizibil pe mobile | Heuristic + Heatmap + User Test | UX/Friction | 5 | Homepage | Mută CTA above fold pe mobile |
| 2 | Lipsa garantiei money-back | Survey + Interviews | Anxiety | 4 | Pricing | Adaugă garantie 30 zile vizibilă |

**Prioritizare**: Sortează după Severity. Problemele identificate de 3+ surse de research = confidence ÎNALTĂ. Problemele identificate de 1 sursă = confidence MEDIE (necesită validare). Transformă top 10 în ipoteze de test → CRO-103 (prioritization matrix).

---

## 3. Cortex Logging

```json
{
  "text": "CRO ResearchXL completat: 6 tipuri research (technical audit, heuristic analysis, GA4 analytics, heatmaps+recordings 500+ sesiuni, on-site survey 50+ responses, user testing 5 participanți), Problem Inventory creat cu 10+ findings prioritizate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-102",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["cro-research", "researchxl", "heuristic-analysis", "user-testing", "problem-inventory", "hotjar", "clarity"],
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
- Teste lansate fără research prealabil → violation
- Problem Inventory creat fără date calitative (doar cantitative) → violation
- User testing cu sub 5 participanți → violation
- Heatmap analysis pe sub 500 sesiuni → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minim 5 din 6 tipuri de research executate?
- [ ] Problem Inventory creat cu severity scoring?
- [ ] Fiecare finding are sursă de research documentată?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-102 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "CRO Research Framework (ResearchXL)" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Hotjar ($39/mo) / Microsoft Clarity (free) | Heatmaps, recordings, surveys |
| Google Analytics 4 | Analiza cantitativă funnel |
| Maze ($99/mo) / UserTesting | User testing structurat |
| PageSpeed Insights / WebPageTest | Technical performance audit |
| Screaming Frog | Technical SEO/broken links audit |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Tipuri de research completate | ≥5 din 6 |
| Findings documentate în Problem Inventory | ≥10 |
| User testing participanți | ≥5 |
| Survey responses | ≥50 |
| Heatmap sessions colectate | ≥500 |
| Durată fază de research | 2-4 săptămâni |
| Ipoteze de test generate | ≥8 testabile |
