---
type: procedure
created: 2026-03-22
status: active
slug: m-056-infographic-visual-content
tags: [procedure, nexus]
---

# Infographic and Visual Content Creation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces de planificare, design și distribuire a infograficelor și conținutului vizual care simplifică informații complexe și generează engagement și shares.

---

## 1. Problema

Conținutul pur text are rate de angajament tot mai scăzute pe rețelele sociale și în articolele de blog. Infograficele și vizualurile bine executate sunt partajate de 3x mai mult decât textul simplu, dar producerea lor fără un proces structurat duce la output inconsistent ca calitate, brand alignment și eficiență de time-to-publish.

Situații acoperite:
- Crearea unui infografic pentru a susține un articol de blog sau campanie
- Producerea de vizualuri explicative pentru concepte complexe sau date statistice
- Generarea de conținut visual pentru campanii de social media

---

## 2. Procedura

### Pas 1: Definirea scopului și tipului de vizual
Determină tipul de conținut vizual necesar bazat pe obiectiv: infografic statistic (date și cifre), infografic de proces (pași sau workflow), comparație (versus sau before/after), timeline, hartă sau distribiție geografică, sau carousel pentru social media. Fiecare tip are reguli de design și formate de output diferite. Scopul clar previne redesign-ul costisitor.

### Pas 2: Colectarea și verificarea datelor/conținutului
Adună toate informațiile, statisticile și citatele care vor fi incluse. Verifică acuratețea fiecărei date cu sursa originală — niciodată statistici din second-hand sources fără verificare. Organizează informațiile în ordinea logică a narațiunii vizuale. Simplifică datele complexe la mesajele cheie — un infografic nu trebuie să conțină tot, ci să selecteze ce este esențial.

### Pas 3: Planificarea structurii și wireframe
Creează un wireframe (schiță) al structurii vizuale înainte de design: titlu principal, secțiuni cu ierarhie clară, flux de citire (top-to-bottom sau left-to-right), plasarea CTA sau branding. Wireframe-ul poate fi o schiță pe hârtie sau în Figma. Aprobarea wireframe-ului de stakeholder înainte de design final reduce iterațiile costisitoare.

### Pas 4: Design conform brand identity
Execută design-ul infograficului respectând strict Brand Visual Guidelines (M-055): culori de brand, fonturi definite, stil de iconografie consistent, logo-ul brandului și URL-ul în footer. Utilizează templates predefinite pentru eficiență când sunt disponibile. Asigură lizibilitate la dimensiunile de afișare țintă — textul trebuie să fie citibil fără zoom pe mobil.

### Pas 5: Optimizarea pentru platforme specifice
Exportă infograficul în formatele și dimensiunile corecte pentru fiecare canal de distribuție: blog post (800-1200px lățime, PNG sau JPEG), Pinterest (1000x1500px vertical), Instagram (1080x1080px sau 1080x1350px), LinkedIn (1200x628px pentru link preview). Comprimă imaginile fără pierdere vizibilă de calitate (TinyPNG sau similar). Adaugă watermark sau URL sursă dacă materialul este publicat standalone.

### Pas 6: Scriere copy pentru distribuție
Fiecare infografic are nevoie de copy de distribuție pregătit: titlu/headline pentru blog post, caption pentru social media per platformă (adaptate ca ton și lungime), alt text pentru accesibilitate și SEO, meta title și description dacă este publicat pe landing page dedicat. Copy-ul anticipează și amplifică mesajul vizual — nu repetă, ci completează.

### Pas 7: Publicare, distribuire și tracking
Publică infograficul pe blog cu embed code pentru ușurarea share-ului. Distribuie pe toate canalele planificate cu copy-ul pregătit în Pas 6. Submitează la directoare de infografice relevante pentru backlink-uri (Visual.ly, Infographic Journal). Configurează tracking pentru a măsura shares, backlink-uri generate și trafic referit. Documentează performanța pentru optimizarea viitoarelor vizualuri.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-056 executată: Infografic și conținut vizual creat și distribuit — concept, design, optimizare per platformă și tracking configurate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-056",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["infographic", "visual-content", "design", "social-media", "content-distribution"],
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
- Infografic publicat fără respectarea brand visual guidelines → violation
- Date statistice utilizate fără verificarea sursei originale → violation
- Conținut vizual distribuit fără copy adaptat per platformă → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Toate datele verificate la sursa originală?
- [ ] Formate exportate corect pentru toate platformele de distribuție?

**VK-uri obligatorii:**
1. `✅ [PROC] M-056 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Infographic and Visual Content Creation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Canva / Figma / Adobe Illustrator | Design și export infografice |
| Brand Identity Design (M-055) | Guidelines vizuale pentru design consistent |
| TinyPNG / Squoosh | Compresie imagini fără pierdere de calitate |
| Buffer / Hootsuite | Scheduling distribuție social media |
| Google Analytics | Tracking trafic și shares |
| Content Repurposing (M-052) | Infograficele sunt formate ideale pentru repurposing |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Shares per infografic | > 50 (în primele 30 zile) |
| Backlink-uri generate | Minimum 3 per infografic |
| Engagement rate vizualuri vs. text | +200% față de posts text |
| Timp producție infografic | < 3 ore cu templates |
