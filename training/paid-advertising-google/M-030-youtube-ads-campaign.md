---
type: procedure
created: 2026-03-22
status: active
slug: m-030-youtube-ads-campaign
tags: [procedure, nexus]
---

# YouTube Ad Campaign Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea completă a campaniei YouTube Ads — de la alegerea formatului video corect până la targetare precisă și optimizare pentru obiectivele de brand sau conversie.

---

## 1. Problema

YouTube Ads sunt ignorate de majoritatea advertiserilor mici și medii din cauza percepției că "ai nevoie de producție video scumpă", dar în realitate video-urile simple de telefon sau screen recordings pot converti excelent cu targetarea corectă. Fără alegerea formatului potrivit și optimizarea audienței, bugetul este consumat pe vizualizări de la utilizatori care nu sunt în publicul țintă. Această procedură standardizează setup-ul YouTube Ads de la producție minimă la campanie activă cu KPI-uri clare.

Situații acoperite:
- Prima campanie YouTube Ads pentru brand sau produs cu video disponibil
- Campanie YouTube pentru remarketing video (remarketing liste din interacțiuni video anterioare)
- Testarea YouTube Ads ca nou canal de achiziție alături de Search și Display

---

## 2. Procedura

### Pas 1: Alege formatul de video ad corect pentru obiectiv
Formatele principale: (1) Skippable In-Stream (TrueView) — poate fi sărit după 5 sec, minimum 12 sec, recomandat pentru awareness și conversii, plătești doar pentru vizualizări de 30 sec sau click; (2) Non-Skippable In-Stream — maximum 15 sec, 100% vizionat, recomandat pentru brand recall; (3) Bumper Ads — 6 sec non-skippable, recomandat pentru reach și frecvență; (4) Video Discovery — apare în search YouTube și related videos, recomandat pentru educație/considerare. Alege 1-2 formate maximum per campanie.

### Pas 2: Pregătește sau optimizează video-ul pentru YouTube Ads
Video-ul trebuie să respecte regula "5 secunde hook": primele 5 secunde trebuie să capteze atenția (brand visible sau mesaj de impact) — deoarece utilizatorul poate sări anunțul după 5 sec. Structura recomandată pentru Skippable: Hook (0-5s) → Problema/Beneficiu (5-15s) → Soluție/Demonstrație (15-25s) → CTA clar (ultimele 5s). Upload video-ul pe canalul YouTube al brandului înainte de a crea campania.

### Pas 3: Configurează campania în Google Ads
Creează campanie nouă → tip "Video". Selectează obiectivul: Product and brand consideration (pentru TrueView) sau Brand awareness and reach (pentru Bumper/Non-Skippable). Configurează: Daily budget, Locations, Languages, Devices. Adaugă Companion Banner (300x60) pentru a crește vizibilitatea pe desktop — se afișează lângă video gratuit dacă ai imagini.

### Pas 4: Targetarea audienței cu precizie
YouTube oferă targetare superioară față de TV: demografic (vârstă, gen, venituri), interese (In-Market, Affinity), intenție (Custom Intent — targetezi pe baza keyword-urilor căutate pe Google în ultimele 7 zile), plasament (canale YouTube specifice, videos specifice), remarketing (utilizatori care au interacționat cu videoclipurile tale anterior). Combină maximum 2 tipuri de targetare. Creează Ad Groups separate pentru fiecare segment de audiență.

### Pas 5: Setează exclusele și Brand Safety
Exclude conținut ne-potrivit pentru brand: activează Content Exclusions la nivel de campanie (Sensitive content categories, Embedded YouTube videos dacă vrei doar YouTube.com). Adaugă Topic Exclusions pentru topicuri irelevante. Excludele Placements specific pe canale YouTube cu conținut controversat sau irrelevant. Pentru campanii de brand, adaugă Keyword Exclusions pentru a nu apărea lângă conținut cu cuvinte cheie negative.

### Pas 6: Adaugă Call-to-Action Overlay și Cards
CTA Overlay: adaugă un banner overlay cu CTA text și URL de destinație — se afișează în video la momentul ales (recomandat: 5-10 sec după start). Video Cards: adaugă Cards (anunțuri interactive) care apar în video cu link la site sau un alt video. End Screens: configurează End Screen (ultimele 20 sec din video) cu CTA buton, link site, abonare canal. Aceste elemente cresc CTR-ul cu 20-30% fără cost suplimentar.

### Pas 7: Monitorizează View Rate, CPV și conversii la 7/14/30 zile
Metrici cheie YouTube Ads: View Rate (% persoane care au urmărit minimum 30 sec — benchmark: 20-30% pentru TrueView), CPV (Cost Per View — benchmark: 0.01-0.05 EUR), CTR pe CTA (benchmark: 0.5-1%). Dacă View Rate sub 15%, video-ul nu captează atenția — rescrie hook-ul sau testează video nou. Configurează YouTube Remarketing Lists (persoane care au urmărit 25%, 50%, 75%, 100% din video) pentru campanii de follow-up. Documentează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "M-030 executat: Campanie YouTube Ads configurată. Format video: [tip]. Durata video: [sec]. Audiențe targetate: [tipuri]. Ad Groups: [N]. View Rate target: [%]. CPV actual: [EUR/RON]. CTA Overlay: DA/NU. Remarketing lists create: [N]. Conversii atribuite YouTube: [N].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-030",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "youtube-ads", "video-advertising", "trueview", "skippable", "bumper-ads", "video-remarketing", "cpv"],
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
- Campanie YouTube lansată fără CTA Overlay sau End Screen configurat → violation
- Video folosit fără hook clar în primele 5 secunde pentru format Skippable → violation
- Campanie lansată fără excluseri Brand Safety configurate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] YouTube Remarketing Lists create (25%/50%/75%/100% view)?
- [ ] View Rate monitorizat la 7 zile și acțiune luată dacă sub 15%?

**VK-uri obligatorii:**
1. `✅ [PROC] M-030 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "YouTube Ad Campaign Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| YouTube Channel (Brand) | Upload video-uri și gestiunea canalului |
| Conversion Tracking (M-022) | Atribuirea conversiilor din YouTube |
| Google Analytics 4 | Analiza comportamentului post-view |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| View Rate (TrueView Skippable) | Minim 25% (benchmark industrie: 20%) |
| CPV mediu | Maximum 0.04 EUR |
| CTR pe CTA Overlay | Minim 0.5% |
