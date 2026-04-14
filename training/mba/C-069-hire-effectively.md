---
type: procedure
created: 2026-03-22
status: active
slug: c-069-hire-effectively
tags: [procedure, nexus]
---

# Hire Effectively (Slow Hire, Fast Fire) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces structurat de angajare deliberată cu criterii clare de terminare rapidă când integrarea eșuează.

---

## 1. Problema

Angajările grăbite produc erori costisitoare: cultura organizațională este contaminată, performanța echipei scade, iar costul înlocuirii unui angajat nepotrivit depășește de 1,5–3x salariul anual. Principiul "slow hire, fast fire" inversează tendința naturală de a angaja rapid și de a trage concluzia lent.

Situații acoperite:
- Nevoia urgentă de personal care presează spre o decizie prematură
- Angajat care nu performează dar managerul amână decizia de terminare
- Lipsa unui proces structurat de interviu care să separe candidații buni de cei mediocri

---

## 2. Procedura

### Pas 1: Definește profilul rolului înainte de a posta
Scrie o fișă detaliată cu: responsabilități principale (3–5), KPI-uri clare, competențe obligatorii vs. dezirabile și cultura-fit necesară. Nu posta anunțul fără acest document finalizat.

### Pas 2: Screening inițial structurat (30 min)
Aplică un set fix de 5 întrebări comportamentale de screening telefonic sau video. Elimină candidații care nu trec pragul minim înainte de a invita la interviu față-în-față. Documentează răspunsurile.

### Pas 3: Minimum 3 runde de interviu
Runda 1 — competențe tehnice (recruiter sau hiring manager). Runda 2 — cultural fit și valori (alt membru al echipei). Runda 3 — scenarii reale de lucru și referințe live. Nicio angajare fără toate cele 3 runde completate.

### Pas 4: Testare practică / trial project
Solicită un deliverable mic plătit (2–5 ore) care simulează munca reală. Evaluează: calitatea output-ului, respectarea deadline-ului, comunicarea în proces. Acesta este filtrul final înainte de ofertă.

### Pas 5: Perioadă de probă cu check-in-uri săptămânale (90 zile)
Stabilește din ziua 1 criteriile clare de succes pentru perioada de probă. Check-in structurat săptămânal în primele 4 săptămâni, bilunar după. Documentează toate observațiile.

### Pas 6: Declanșează "fast fire" la primele semne clare
Dacă în primele 90 zile apar: pattern de subperformanță persistentă, incompatibilitate dovedită de valori sau comportament toxic — acționează în maxim 2 săptămâni de la identificare. Întârzierea nu ajută pe nimeni.

### Pas 7: Terminare cu demnitate și documentare
Finalizează procesul direct, clar și respectuos. Documentează motivele în scris. Extrage learningul: ce semne au fost ignorate în proces? Actualizează profilul rolului și procesul de screening.

---

## 3. Cortex Logging

```json
{
  "text": "C-069 Hire Effectively executat: proces slow-hire aplicat, decizie de angajare sau terminare documentată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-069",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["hiring", "team-building", "slow-hire", "fast-fire", "HR", "management"],
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
- Angajare grăbită fără minimum 3 runde de interviu → violation
- Terminare întârziată mai mult de 2 săptămâni după identificarea problemei → violation critică
- Ofertă extinsă fără trial project completat → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 3 runde de interviu documentate?
- [ ] Trial project evaluat înainte de ofertă?

**VK-uri obligatorii:**
1. `✅ [PROC] C-069 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Hire Effectively (Slow Hire, Fast Fire)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Fișa de rol | Document de referință pentru evaluare |
| Rubrica de interviu | Scorecard structurat per rundă |
| Sistem de tracking candidați | Documentare progres și decizii |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Timp mediu hiring | < 30 zile de la postare la ofertă |
| Rata retenție angajați la 12 luni | > 80% |
| Timp maxim la decizie de terminare după identificare | < 14 zile |
