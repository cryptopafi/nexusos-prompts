---
id: CRO-106
title: Heatmap Analysis and User Behavior Interpretation
domain: conversion-optimization
source: Complete Conversion Optimization Course / CRO Landing Page Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Heatmap Analysis and User Behavior Interpretation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Metodologia completă de colectare, interpretare și transformare a heatmaps, scroll maps, session recordings și form analytics în ipoteze CRO acționabile. Include tool comparison cu prețuri (Hotjar $39/mo, Clarity free, FullStory, Crazy Egg), GDPR compliance, și triangulation methodology.

---

## 1. Problema

Heatmaps sunt colectate dar rareori interpretate corect. Echipele văd "zone roșii" și trag concluzii greșite. Fără o metodologie structurată de interpretare, heatmap data produce false insights. Corect interpretate, heatmaps dezvaluie pattern-uri de confuzie, friction și abandon invizibile în analytics clasic.

Situații acoperite:
- Diagnosticarea unei pagini cu conversie suboptimă
- Validarea/invalidarea unei ipoteze de test CRO
- Research pre-test pentru generare ipoteze noi

---

## 2. Procedura

### Pas 1: Setup tracking — tool selection și GDPR compliance

**Tool comparison** (2026):
| Tool | Preț | Heatmaps | Recordings | Surveys | Form Analysis | GDPR |
|------|------|----------|------------|---------|---------------|------|
| **Hotjar** | $39/mo (Plus) | ✅ | ✅ 100 sessions/day | ✅ | ✅ | ✅ GDPR-ready |
| **Microsoft Clarity** | **FREE** | ✅ | ✅ Unlimited | ❌ | ❌ | ✅ |
| **FullStory** | $199/mo+ | ✅ | ✅ Advanced | ❌ | ✅ | ✅ |
| **Crazy Egg** | $49/mo | ✅ Best | ✅ Limited | ❌ | ❌ | ✅ |

**Recomandare**: Start cu Microsoft Clarity (free, unlimited) pentru recordings. Adaugă Hotjar ($39/mo) pentru surveys + form analysis. FullStory/Crazy Egg pentru enterprise.

**GDPR compliance obligatorie**:
- [ ] Mask-ează câmpuri formular (parole, date personale, carduri) — configurează în tool
- [ ] Exclud IP-urile angajaților
- [ ] Adaugă mention în privacy policy: "Folosim [tool] pentru a înțelege cum vizitatorii interacționează cu site-ul"
- [ ] Cookie consent: heatmap tracking = analytics cookies (nu marketing) — dar trebuie menționat

**Sample rate**: 100% pentru pagini cu <500 vizitatori/zi. 30-50% pentru pagini cu trafic mare (reduce storage).

**Minim 500 sesiuni SAU 7 zile** de date înainte de analiză.

### Pas 2: Click heatmap analysis
Deschide click heatmap. Întrebări de diagnosticare:

| Pattern | Ce indică | Acțiune |
|---------|----------|---------|
| Click-uri pe elemente NON-interactive (imagine, text decorativ) | Utilizatorii cred că e clickabil | Fă-l clickabil (adaugă link) sau fă-l să arate non-clickabil |
| CTA primar nu primește click-uri | Invizibil sau neconvingător | Mărește, schimbă culoare contrastantă, reformulează text |
| Click-uri pe navigația secundară/footer | Nu găsesc ce caută pe pagină | Conținutul principal nu răspunde la nevoie |
| Click-uri pe imagini de produs fără zoom | Vor să vadă detalii | Adaugă zoom/lightbox pe imagini |
| "Dead clicks" (click fără efect) | Frustrare | Investighează — element broken sau expectation mismatch |
| Click-uri concentrate pe un singur element | Atenție captată de element neimportant | Redistribuie atenția vizuală spre CTA |

**Screenshot**: Documentează fiecare pattern anormal cu screenshot + anotare.

### Pas 3: Scroll map analysis
Scroll map arată CE PROCENT din vizitatori ajunge la fiecare secțiune:

**Interpretare**:
- **"Cliff"** = zona unde >30% din vizitatori dispar brusc. Tot conținutul DEASUPRA cliff-ului e cel mai important
- **CTA deasupra cliff-ului?** Dacă nu → mută mai sus. 50%+ din vizitatori nu ajung la CTA = 50% conversie pierdută
- **Mobile cliff**: Pe mobile, cliff-ul e MULT mai sus (ecran mic). Verifică SEPARAT
- **"False bottom"**: Secțiune care arată ca și cum pagina s-a terminat → vizitatorii nu scrollează mai departe. Fix: adaugă indicator vizual "mai este conținut" sau restructurează layout
- **Social proof sub cliff**: Testimonials, case studies, trust signals trebuie vizibile ÎNAINTE de cliff

**Regula**: Content critic (CTA, value prop, trust signals) trebuie plasat unde ≥70% din vizitatori ajung.

### Pas 4: Session recordings analysis (20+ sesiuni obligatorii)
Vizualizează 20+ sesiuni de la utilizatori care au ABANDONAT (nu au convertit):

**Ce documentezi per sesiune**:
- **Scroll back & forth**: Indică că au ratat ceva sau compară secțiuni
- **Hover lung fără acțiune**: Hesitation — cititorul gândește, ezită
- **Rage clicks**: Click-uri rapide repetate pe un element = frustrare
- **Mouse movement toward X/back**: Intent de abandon
- **Form field re-fill**: Câmpul e confuz sau a dat eroare
- **Time spent pe o secțiune**: >30 secunde = citesc cu atenție SAU sunt blocați

**NU baza concluzii pe o singură sesiune** — caută PATTERN-URI în minim 5-10 sesiuni. Un pattern repetitiv = finding valid.

**Tip**: Clarity oferă "Smart Events" care identifică automat: rage clicks, dead clicks, quick backs — folosește-le ca starting points.

### Pas 5: Form analytics (dacă pagina are formular)
Hotjar Form Analysis sau FullStory form analytics:

| Metrică | Ce arată | Threshold problemă |
|---------|---------|-------------------|
| Field drop-off rate | Care câmp pierde cei mai mulți users | >15% drop-off pe un câmp |
| Time to complete per field | Care câmp durează cel mai mult | >30s pe un câmp simplu |
| Field re-fills | Care câmp e completat greșit/re-completat | >10% re-fill rate |
| Blank submissions | Câmpuri trimise goale | Trebuie required validation |

**Acțiuni per finding**:
- Câmp cu >15% drop-off: Elimină-l (dacă nu e esențial) sau reformulează label-ul
- Câmp care durează >30s: Simplifică (dropdown vs. text liber, auto-complete, pre-fill)
- Câmp re-completat frecvent: Adaugă helper text / placeholder mai clar / input validation real-time

### Pas 6: Triangularea cu analytics și survey data
Heatmaps singure NU explică "de ce". Coroborează cu alte surse:

| Heatmap finding | Analytics coroborare | Survey coroborare | Concluzie |
|----------------|---------------------|-------------------|-----------|
| Nimeni nu clickuiește CTA | Bounce rate 80% pe mobil | "Nu am găsit butonul" | CTA invizibil pe mobil → mută above fold |
| Scroll cliff la 40% | Time on page: 8s average | - | Conținutul nu captează atenție → restructurează above fold |
| Rage clicks pe preț | - | "Prețul nu include TVA?" | Informație de preț neclară → adaugă "VAT included" |

**Regula triangulării**: O singură sursă = ipoteză SLABĂ. Două surse = ipoteză MEDIE. Trei surse = ipoteză PUTERNICĂ → confidence score ridicat în ICE framework.

### Pas 7: Transformarea în ipoteze testabile
Pentru fiecare pattern de comportament anormal documentat:

**Format ipoteză**:
"Am observat că [X]% din utilizatorii [segment] [comportament observat] pe [pagina/elementul]. Ipoteză: [element/cauză] produce [problema]. Dacă schimbăm [element] din [current] în [proposed], atunci [metrică] va [direcție] cu [estimat]% deoarece [raționale din research triangulat]."

**Exemplu**:
"Am observat că 65% din utilizatorii mobili nu ajung la CTA-ul principal (scroll cliff la 45% pe mobile). Ipoteză: CTA-ul este plasat prea jos pe mobil. Dacă adăugăm un sticky CTA button pe mobile, form submissions va crește cu 15-20% deoarece heatmap + analytics confirmă că utilizatorii mobili nu scrollează suficient."

Adaugă fiecare ipoteză în Test Backlog (CRO-103) cu sursa "heatmap analysis" și confidence score bazat pe triangulare.

---

## 3. Cortex Logging

```json
{
  "text": "Heatmap analysis completată: click/scroll/recording analizate pe 500+ sesiuni, 20+ recordings de abandonuri, form analytics per câmp, pattern-uri de confuzie/friction identificate, triangulare cu analytics+survey, 5+ ipoteze de test generate cu confidence scoring.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-106",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["heatmap", "session-recordings", "user-behavior", "cro", "hotjar", "clarity", "scroll-map", "form-analysis"],
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
- Analiza pe sub 500 sesiuni sau 7 zile → violation
- Date personale vizibile în recordings (nemascare) → violation GDPR critică
- Concluzii bazate pe o singură sesiune (nu pattern) → violation
- Ipoteze generate fără triangulare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] GDPR: date sensibile mascate?
- [ ] Minim 500 sesiuni sau 7 zile colectate?
- [ ] Minim 20 recordings vizualizate?
- [ ] Findings triangulate cu minim 2 surse?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-106 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Heatmap Analysis and User Behavior Interpretation" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Hotjar ($39/mo) | Heatmaps, recordings, form analytics, surveys |
| Microsoft Clarity (FREE) | Recordings, heatmaps (unlimited) |
| Crazy Egg ($49/mo) | Heatmaps premium, confetti view |
| FullStory ($199/mo+) | Enterprise session replay |
| Google Analytics 4 | Triangulare date cantitative |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Sesiuni analizate per pagină | ≥500 |
| Session recordings vizualizate | ≥20 (abandonuri) |
| Ipoteze de test generate | ≥5 per analiză |
| Ipoteze triangulate (2+ surse) | 100% |
| GDPR compliance (date mascate) | 100% |
| Pattern-uri documentate cu screenshot | ≥3 per analiză |
