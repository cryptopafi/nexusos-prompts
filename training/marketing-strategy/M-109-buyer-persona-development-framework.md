---
type: procedure
created: 2026-03-22
status: active
slug: m-109-buyer-persona-development-framework
tags: [procedure, nexus]
---

# Buyer Persona Development Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea buyer persona-urilor detaliate bazate pe date reale pentru a fundamenta toate deciziile de marketing, mesagerie și product development.

---

## 1. Problema

Buyer persona-urile create pe baza asumpțiilor interne fără validare cu date reale duc la mesaje care nu rezonează, campanii cu ROI scăzut și produse/servicii care nu se aliniază cu nevoile reale ale clienților. Fără persona-uri bine definite, toate activitățile de marketing sunt calibrate pe intuiție, nu pe înțelegere profundă.

Situații acoperite:
- Crearea buyer persona-urilor de la zero pentru un business nou sau un segment nou de piață
- Actualizarea persona-urilor existente care nu mai reflectă realitatea bazei de clienți actuale
- Dezvoltarea de persona-uri pentru un produs/serviciu nou adresat unui segment diferit față de cel existent

---

## 2. Procedura

### Pas 1: Colectarea datelor cantitative
Extrage date din sursele disponibile: CRM (demografice, comportament de cumpărare, valoare client), Google Analytics 4 (audiențe, fluxuri de navigare, device-uri), date sociale (vârstă, locație, interese), survey-uri existente. Identifică segmentele naturale din baza de clienți (clustering pe comportament, valoare, sursă de achiziție). Documentează 5-10 atribute cantitative clare per segment.

### Pas 2: Colectarea datelor calitative prin interviuri
Realizează minim 8-10 interviuri cu clienți reali din segmentul țintă (30-45 min fiecare). Structura interviului: situația înainte de achiziție, cum au căutat soluția, criteriile de decizie, obiecțiile avute, rezultatele obținute, ce ar schimba. Nu pune întrebări leading. Înregistrează (cu permisiune) și transcrie pentru analiză. Datele calitative sunt mai valoroase decât cele cantitative pentru înțelegerea motivațiilor.

### Pas 3: Identificarea Jobs-to-be-Done și pain points
Din datele colectate, extrage: Job-uri funcționale (ce trebuie să facă produsul), Job-uri emoționale (cum vrea clientul să se simtă), Job-uri sociale (cum vrea să fie perceput). Identifică top 3-5 pain points per segment cu frecvența menționării în interviuri. Citează direct din interviuri — nu parafrazează. Acestea vor fi baza mesageriei.

### Pas 4: Construirea profilului complet al persona-ului
Completează template-ul de persona pentru fiecare segment identificat (maximum 3 persona-uri primare): date demografice și psihografice, un citat reprezentativ din interviuri, goals și motivații, frustrations și obiecții, canale de informare preferate, trigger-e de cumpărare, criterii de decizie. Dă un nume și o fotografie stock reprezentativă pentru a facilita utilizarea internă.

### Pas 5: Validarea și prioritizarea persona-urilor
Validează persona-urile cu echipa de sales/support (ei cunosc clienții direct) și cu un sub-eșantion de 2-3 clienți din fiecare segment ("recunoști această descriere?"). Prioritizează persona-urile după: dimensiunea segmentului, valoarea medie a clientului, ușurința de targeting. Definește persona primară (focus principal) și persona secundare.

### Pas 6: Distribuirea și integrarea în procese
Creează un document de persona vizual, accesibil întregii echipe. Integrează persona-urile explicit în: brieful de content, copywriting guidelines, targeting de ads, script-uri de sales. Fiecare inițiativă de marketing trebuie să indice explicit: "pentru ce persona este aceasta?". Fără persona indicată → inițiativa se respinge.

### Pas 7: Actualizare periodică și validare continuă
Programează review semestrial al persona-urilor cu date noi din CRM și interviuri noi (minim 3-4 per ciclu). Piața și clienții evoluează — persona-uri mai vechi de 18 luni fără update sunt considerate obsolete. Actualizează procedure-health.json și Cortex cu versiunea și data ultimei actualizări.

---

## 3. Cortex Logging

```json
{
  "text": "Buyer persona-uri create pe baza datelor reale: interviuri calitative realizate, JTBD identificate, profiluri validate cu echipa, integrate în procese de marketing.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-109",
    "domain": "marketing-strategy",
    "rule_id": "META-H-002",
    "tags": ["buyer-persona", "jtbd", "market-research", "customer-research", "segmentation"],
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
- Persona construit fără interviuri cu clienți reali (bazat exclusiv pe asumpții) → violation
- Mai mult de 3 persona-uri primare definite simultan → violation (diluare focus)
- Persona-uri mai vechi de 18 luni utilizate fără revizuire → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum marketing-strategy

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 8 interviuri cu clienți reali realizate și documentate?
- [ ] Persona-urile validate cu echipa de sales/support?

**VK-uri obligatorii:**
1. `✅ [PROC] M-109 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Buyer Persona Development Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CRM | Date cantitative despre comportamentul clienților |
| Google Analytics 4 | Date demografice și comportamentale online |
| Interview recording tool (Loom/Zoom) | Capturarea și transcrierea interviurilor calitative |
| Notion / Confluence | Documentarea și distribuirea persona-urilor |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Interviuri calitative realizate | ≥8 per persona |
| Validare echipă internă | 100% persona-uri validate |
| Review periodic executat | Semestrial |
