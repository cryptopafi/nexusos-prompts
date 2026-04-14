---
type: procedure
created: 2026-03-22
status: active
slug: m-066-website-cro
tags: [procedure, nexus]
---

# Website Conversion Rate Optimization (CRO) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces complet de CRO bazat pe date — de la baseline și identificarea friction points până la A/B testing și implementarea variantelor câștigătoare.

---

## 1. Problema

Un site cu trafic bun dar conversii slabe pierde venituri zilnic. Fără un proces structurat de CRO, îmbunătățirile sunt bazate pe intuiție, testele sunt invalide statistic și nu există prioritizare. Procesul standard elimină ghicitul și aduce îmbunătățiri măsurabile.

Situații acoperite:
- Rată de conversie sub benchmarkul industriei
- Bounce rate ridicat pe pagini cheie
- Funnel de cumpărare cu drop-off mare
- Site nou care trebuie optimizat proactiv

---

## 2. Procedura

### Pas 1: Stabilește baseline-ul CRO
Colectează metricile actuale din Google Analytics:
- **Rata de conversie** per obiectiv (purchase / lead / signup)
- **Bounce rate** per pagină
- **Time on page** per pagină cheie
- **Funnel drop-off rate** per pas

Documentează toate valorile cu data de referință. Aceasta este linia de bază față de care vei măsura îmbunătățirile.

### Pas 2: Instalează heatmap și session recording
Instalează Hotjar sau Microsoft Clarity (gratuit):
- Activează heatmaps pe paginile cu cel mai mult trafic
- Activează session recordings
- Instalează polls sau surveys pe pagini cu bounce rate ridicat

Colectează minimum 500 sesiuni per pagină înainte de analiză.

### Pas 3: Identifică friction points
Analizează datele colectate pentru a găsi probleme reale:
- **Rage clicks** — elemente pe care lumea apasă fără să funcționeze
- **Drop-off pages** — unde ies vizitatorii din funnel
- **Scroll depth** — unde se opresc din citit
- **Confusing navigation** — click-uri pe elemente non-interactive

Documentează minimum 5 friction points cu dovezi (screenshot heatmap / recording).

### Pas 4: Formulează ipoteze de îmbunătățire
Pentru fiecare friction point, scrie o ipoteză structurată:
`Dacă [modificare], atunci [metric] va crește cu [X%], deoarece [raționament bazat pe date].`

Exemplu: "Dacă mutăm CTA-ul above-the-fold, CR va crește cu 15%, deoarece heatmap-ul arată că 70% din vizitatori nu scrollează."

### Pas 5: Prioritizează cu ICE Score
Calculează ICE Score pentru fiecare ipoteză (scală 1-10 fiecare):
- **I (Impact)** — cât de mult poate crește CR?
- **C (Confidence)** — cât de sigur ești că va funcționa?
- **E (Ease)** — cât de ușor este de implementat?

ICE = (I + C + E) / 3. Sortează lista descrescător și lucrează de sus în jos.

### Pas 6: Rulează A/B teste
Configurează testele cu VWO (plan gratuit disponibil), AB Tasty sau Optimizely (Google Optimize a fost retras în sept. 2023):
- Testează o singură variabilă per test
- Asigură semnificativitate statistică: minimum 95% confidence
- Durată minimă test: 2 săptămâni (nu mai puțin, chiar dacă ai trafic mare)
- Sample size: calculat cu calculator A/B (minim 100 conversii per variantă)

Documentează fiecare test: ipoteză, variantă A vs B, rezultat.

### Pas 7: Implementează variantele câștigătoare
Pentru testele câștigate (confidence ≥ 95%):
- Implementează varianta câștigătoare permanent în site
- Documentează uplift-ul real față de baseline
- Actualizează baseline-ul cu noile valori
- Adaugă câștigul în raportul lunar CRO

Repornește ciclul de la Pas 3 cu noi friction points identificate.

---

## 3. Cortex Logging

```json
{
  "text": "Website CRO executat: baseline stabilit, heatmap instalat, friction points identificate, ipoteze formulate cu ICE score, A/B teste configurate, variante câștigătoare implementate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-066",
    "domain": "website-landing-pages",
    "rule_id": "META-H-002",
    "tags": ["cro", "ab-testing", "heatmap", "conversion", "ice-score", "optimization"],
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

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum website-landing-pages

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-066 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Website Conversion Rate Optimization (CRO)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics 4 | Baseline metrics și conversii |
| Hotjar / Microsoft Clarity | Heatmap, session recording, polls |
| VWO / AB Tasty / Optimizely | A/B testing platform |
| ICE Score framework | Prioritizare ipoteze |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Friction points identificate | Minimum 5 |
| A/B test confidence | ≥ 95% |
| CR improvement per ciclu | ≥ 5% |
