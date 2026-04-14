---
type: procedure
created: 2026-03-22
status: active
slug: m-027-google-ads-optimization-scaling
tags: [procedure, nexus]
---

# Google Ads Campaign Optimization and Scaling — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Optimizarea sistematică a campaniilor Google Ads active și scalarea bugetului pe baza datelor de performanță pentru maximizarea ROAS la volum crescut.

---

## 1. Problema

Campaniile Google Ads lăsate să ruleze fără optimizare periodică degradează constant în performanță: keywords irelevante se acumulează în search terms, anunțurile obosesc, Quality Score scade, iar CPA crește. Scalarea fără un proces definit duce la diminuarea ROAS — mărești bugetul dar eficiența scade. Această procedură standardizează ciclul de optimizare săptămânal și decizia de scalare bazată pe date.

Situații acoperite:
- Review săptămânal de optimizare pentru campanii active
- Decizie de scalare a bugetului pentru campanii care performează sub capacitate
- Diagnoza și recuperarea unei campanii cu CPA în creștere sau ROAS în scădere

---

## 2. Procedura

### Pas 1: Audit Search Terms Report și adaugă negative keywords
Descarcă Search Terms Report pentru ultimele 7 zile. Identifică și adaugă ca negative keywords toți termenii cu: CTR sub 0.5% și nicio conversie după minimum 50 impresii, termeni complet irelevanți pentru business, termeni cu CPA de 3x față de CPA target. Adaugă minimum 10 negative keywords noi per săptămână în primele 4 săptămâni ale campaniei.

### Pas 2: Evaluează performanța keyword-urilor și ajustează bids
Identifică keywords în 3 categorii: (A) Performers — converge la sau sub CPA target → menține sau crește bid cu 10-15%, (B) Learners — au clicks dar fără conversii după 50+ clicks → reduce bid cu 20% și monitorizează, (C) Wasters — fără conversii după 100+ clicks → pauzează sau adaugă ca negative. Nu pauzezi niciodată keyword-uri fără minimum 100 clicks de date.

### Pas 3: Revizuiește și testează anunțurile (A/B test RSA)
Verifică Ad Strength pentru toate RSA-urile active. Înlocuiește orice RSA cu rating "Poor" sau "Average". Testează câte o variantă nouă per Ad Group în fiecare lună: schimbă 1-2 headlines pentru a valida mesaje noi. Pauzează RSA-ul cu CTR mai mic după minimum 200 impresii per anunț. Documentează ce mesaje convertesc cel mai bine.

### Pas 4: Analizează Quality Score și rezolvă problemele de relevanță
Exportă Quality Score per keyword (Expected CTR, Ad Relevance, Landing Page Experience). Orice keyword cu QS sub 5/10 necesită acțiune: (1) rescrie anunțul pentru relevanță mai mare, (2) creează landing page dedicat, (3) mută keyword-ul în propriul Ad Group dedicat. Îmbunătățirea QS de la 5 la 7 reduce CPC efectiv cu ~20%.

### Pas 5: Optimizează bid adjustments pe dispozitiv, locație și orar
Analizează performanța segmentată pe: Device (dacă mobile are CPA cu 30%+ mai mare → ajustare negativă -30%), Location (dacă anumite județe sau orașe au CPA sub medie → ajustare pozitivă), Hour of day (dacă orele 22-6 nu convertesc → ajustare -100% sau Schedule off). Actualizează bid adjustments cu datele din ultimele 30 de zile.

### Pas 6: Evaluează decizia de scalare cu regula 3-2-1
Aplică regula de scalare: scalezi bugetul DOAR dacă în ultimele 14 zile: (1) CPA actual este sub CPA target cu minimum 15%, (2) Campania nu este limitată de buget, (3) Calitatea lead-urilor/vânzărilor este bună (confirmă cu echipa de vânzări). Mărește bugetul cu maximum 20-30% o dată și monitorizează 7 zile înainte de o nouă creștere.

### Pas 7: Documentează optimizările și setează next review
Creează un log de optimizare cu: data, ce s-a schimbat, motivul schimbării, rezultatul așteptat. Compară KPI-urile cheie față de săptămâna anterioară: CPA, ROAS, CTR, Impression Share, Conversion Rate. Documentează în Cortex. Setează reminder pentru review la 7 zile.

---

## 3. Cortex Logging

```json
{
  "text": "M-027 executat: Ciclu optimizare Google Ads completat. Campanie: [Nume]. Negative keywords adăugate: [N]. Keywords pauzate: [N]. RSA actualizate: [N]. QS mediu înainte/după: [X/Y]. CPA săptămâna curentă vs. anterioară: [%]. Decizie scalare: DA/NU. Buget nou: [RON].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-027",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "optimization", "scaling", "quality-score", "negative-keywords", "bid-adjustments", "search-terms"],
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
- Keyword pauzat fără minimum 100 clicks de date → violation
- Buget crescut cu mai mult de 30% o dată fără documentare justificare → violation
- Review ciclu efectuat fără verificarea Search Terms Report → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 10 negative keywords noi adăugate (primele 4 săptămâni)?
- [ ] Regula 3-2-1 aplicată înainte de orice decizie de scalare?

**VK-uri obligatorii:**
1. `✅ [PROC] M-027 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Ads Campaign Optimization and Scaling" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Ads Reports | Search Terms, QS, Performance data |
| Google Analytics 4 | Calitate trafic și comportament post-click |
| CRM / Sales Team | Validarea calității lead-urilor pentru decizie scalare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CPA improvement lunar | Minim -10% față de luna anterioară |
| Quality Score mediu campanie | Minim 7/10 menținut |
| Impression Share (Search) | Minim 60% pentru campanii prioritare |
