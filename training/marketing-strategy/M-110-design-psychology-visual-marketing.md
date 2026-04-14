---
type: procedure
created: 2026-03-22
status: active
slug: m-110-design-psychology-visual-marketing
tags: [procedure, nexus]
---

# Design Psychology and Visual Marketing — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea principiilor de psihologie vizuală și design cognitiv în crearea materialelor de marketing pentru maximizarea atenției, înțelegerii și acțiunii.

---

## 1. Problema

Materialele de marketing create fără înțelegerea psihologiei vizuale și a procesării cognitive eșuează să ghideze atenția utilizatorului spre elementele cheie și spre acțiunea dorită. Alegeri greșite de culori, ierarhie vizuală, tipografie și layout reduc conversia și credibilitatea percepută a brandului.

Situații acoperite:
- Crearea sau redesignul unui asset de marketing: landing page, ad creativ, email template, prezentare
- Evaluarea și optimizarea materialelor existente cu rate de engagement scăzute
- Stabilirea unui sistem de design consistent pentru toate comunicările de marketing

---

## 2. Procedura

### Pas 1: Definirea obiectivului vizual și a acțiunii dorite
Înainte de orice decizie de design, documentează: ce acțiune unică vrei să facă utilizatorul (click, scroll, read, fill form), care este informația cea mai importantă care trebuie reținută, care este emoția principală pe care materialul trebuie să o evoce. Un asset de marketing = o acțiune principală. Fără claritate pe obiectiv, designul devine decorativ.

### Pas 2: Ierarhia vizuală și F/Z pattern
Structurează layout-ul pe baza pattern-urilor naturale de scanare: F-pattern pentru conținut text-heavy (email, blog), Z-pattern pentru pagini cu puțin text (landing pages, ads). Plasează elementul cu cel mai mare impact în colțul stânga-sus (primul fixation point). Stabilește 3 niveluri clare de ierarhie: titlu principal → informație secundară → CTA. Testează cu eye-tracking sau heatmap dacă posibil.

### Pas 3: Psihologia culorilor aplicată la context
Alege culorile bazat pe: emoția comunicată (albastru = încredere/profesionalism, roșu = urgență/energie, verde = succes/creștere, portocaliu = accesibilitate/entuziasm), contrastul pentru lizibilitate (minim 4.5:1 ratio conform WCAG), consistența cu brand identity. CTA-urile trebuie să fie în culoarea cea mai contrastantă față de background. Evită mai mult de 3 culori dominante per asset.

### Pas 4: Tipografia pentru lizibilitate și emoție
Selectează maximum 2 fonturi: unul pentru titluri (personalitate/emoție) și unul pentru body text (lizibilitate). Font size minim pentru body: 16px web, 10pt print. Line height: 1.5-1.6 pentru text lung. Alinierea stângă pentru text lung (nu centrat — reduce viteza de citire). Utilizează weight și size pentru ierarhie, nu culori multiple de text.

### Pas 5: Principii Gestalt și ghidarea atenției
Aplică principiile Gestalt strategic: Proximity (grupează elementele corelate), Similarity (uniformitate vizuală pentru elemente de același tip), Contrast (face elementul cheie să iasă în evidență), Whitespace (nu "spațiu gol" — spațiu activ care ghidează privirea). Folosește linii de direcție, priviri ale oamenilor din imagini și elemente de contrast pentru a ghida ochiul spre CTA.

### Pas 6: Testarea vizuală și validarea
Realizează un "5-second test": arată materialul cuiva 5 secunde și întreabă ce a reținut și ce acțiune crede că trebuie să facă. Dacă răspunsurile nu coincid cu obiectivele din Pas 1, redesign este necesar. Testează pe dimensiuni de ecran multiple (mobile first). Rulează A/B test pe variantele de creativ dacă traficul permite.

### Pas 7: Documentare în sistem de design și actualizare
Documentează deciziile de design în brand guidelines sau sistem de design: paletă de culori cu coduri HEX, fonturi cu sizing scale, template-uri validate pentru tipurile de materiale recurente. Creează un library de asset-uri aprobate refolosibile. Actualizează sistemul de design trimestrial cu noile learninguri din teste. Actualizează procedure-health.json și Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Design psychology aplicat pe material de marketing: ierarhie vizuală definită, psihologie culori aplicată, principii Gestalt utilizate, 5-second test trecut.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-110",
    "domain": "marketing-strategy",
    "rule_id": "META-H-002",
    "tags": ["design-psychology", "visual-marketing", "gestalt", "color-psychology", "visual-hierarchy", "cro"],
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
- Asset creat fără definirea obiectivului unic și acțiunii dorite (Pas 1) → violation
- Design lansat fără 5-second test sau alt tip de validare cu utilizatori reali → violation
- Utilizarea a mai mult de 3 culori dominante fără justificare strategică → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum marketing-strategy

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] 5-second test executat cu cel puțin 2 persoane externe?
- [ ] Deciziile de design documentate în brand guidelines?

**VK-uri obligatorii:**
1. `✅ [PROC] M-110 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Design Psychology and Visual Marketing" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Figma / Adobe XD | Design și prototipare |
| Hotjar / Clarity | Heatmaps pentru validarea ierarhiei vizuale |
| Contrast checker tool | Verificarea accesibilității culorilor (WCAG) |
| Brand guidelines doc | Referință pentru consistență vizuală |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| 5-second test pass rate | ≥80% identificare corectă acțiune |
| Contrast ratio text/background | ≥4.5:1 (WCAG AA) |
| Assets documentate în brand guidelines | 100% din template-uri recurente |
